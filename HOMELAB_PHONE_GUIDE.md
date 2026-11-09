# 📱 CẨM NANG HOMELAB — MOTOROLA EDGE PLUS 2022
> Bản cập nhật: kiến trúc **rootless (proot-distro)** sau khi cài lại máy — thay cho bản `chroot`-as-root cũ.
> Mục tiêu: biến điện thoại thành Home Server mini chạy 24/7, host Spring Boot + MariaDB + Cloudflare Tunnel.

---

## 1. ⚙️ PHẦN CỨNG & MÔI TRƯỜNG

| Thông số | Chi tiết |
| :--- | :--- |
| **Thiết bị** | Motorola Edge Plus 2022 |
| **Vi xử lý (SoC)** | Qualcomm Snapdragon 8 Gen 1 (ARM64 / `aarch64`) |
| **RAM / Bộ nhớ** | 8–12GB LPDDR5 / 128–256GB UFS 3.1 |
| **Quyền Root** | ❌ **KHÔNG có** (đã mất sau khi cài lại máy) |
| **Vỏ chạy server** | **Termux** → **`proot-distro` Debian GNU/Linux 13 (Trixie)** — không cần root |
| **Mạng riêng ảo** | **Tailscale** (app Android) |
| **IP Tailscale (hiện tại)** | `100.120.114.66` (tên máy: `motorola-edge-plus-2022`) |
| **SSH vào máy** | Termux `sshd` cổng `8022`, user `u0_a388`<br>`ssh -p 8022 u0_a388@100.120.114.66` |

> Tên user Termux (`u0_aXXX`) **đổi mỗi lần cài lại app** → luôn kiểm tra bằng `whoami` trong Termux.

---

## 2. 🏗️ KIẾN TRÚC TỔNG THỂ

```text
        [ Khách truy cập Internet ]
                    │  (HTTPS)
         [ Cloudflare Quick Tunnel ]  (trycloudflare.com)
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
   n1-service :8082        auth-service :9000
   (BE + AdminLTE)         (login + phát JWT)
        │                       │
        └───────────┬───────────┘
                    ▼
            [ MariaDB 11.8 :3306 ]
            (DB quanlydaotao + seed RBAC)

     ── tất cả chạy bên trong ──
     Termux → 1 phiên proot-distro Debian sống lâu
              (giữ bởi launcher + Termux:Boot)
```

**Điểm mấu chốt:** vì không có root, ta **không** bind được cổng <1024 (không dùng Caddy ở cổng 80). Thay vào đó mỗi service nghe cổng cao (9000/8082) và **cloudflared** (userspace) phơi ra HTTPS qua Cloudflare.

---

## 3. 🧩 CÁC FILE & THÀNH PHẦN CHỐT HẠ

| Nơi | Đường dẫn | Vai trò |
| :--- | :--- | :--- |
| Termux | `~/start-homelab.sh` | Vòng lặp giữ **1 phiên proot** sống mãi; tự dựng lại nếu chết |
| Termux | `~/.termux/boot/00-homelab.sh` | Hook **Termux:Boot**: bật `sshd` + chạy launcher khi khởi động máy |
| Termux | `~/url.sh` | In URL công khai hiện tại |
| Debian | `/root/homelab-init.sh` | Khởi động **MariaDB → 2 tunnel → PM2**, rồi `tail -f /dev/null` |
| Debian | `/root/homelab/ecosystem.config.js` | Cấu hình 2 app cho PM2 (đọc env động) |
| Debian | `/root/homelab.env` | `APP_JWT_SECRET` (dùng chung cho cả 2 service) |
| Debian | `/root/Project/quanlydaotaoit24ab` | Mã nguồn (nhánh `homelab` **= `nhom1`**, đã hỗ trợ MariaDB sẵn) |
| Debian | `/root/homelab-init.out` | Log của phiên proot (chứa cả URL tunnel) |

**Rootfs proot-distro thực tế:** `/data/data/com.termux/files/usr/var/lib/proot-distro/containers/debian/rootfs/`
(⚠️ layout mới là `containers/<name>/rootfs`, **không phải** `installed-rootfs/<name>`.)

---

## 4. 🚀 QUY TRÌNH DỰNG TỪ ĐẦU (rootless)

### Bước 1 — Termux
```bash
pkg update -y && pkg upgrade -y
pkg install -y proot-distro openssh curl ca-certificates
proot-distro install debian
proot-distro login debian
```

### Bước 2 — Toolchain trong Debian
```bash
export DEBIAN_FRONTEND=noninteractive
printf "nameserver 8.8.8.8\nnameserver 1.1.1.1\n" > /etc/resolv.conf
apt update && apt -y upgrade
apt -y install openjdk-21-jdk-headless git mariadb-server curl ca-certificates nodejs npm
npm install -g pm2

# cloudflared (ARM64)
curl -sL -o /tmp/cloudflared.deb \
  https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64.deb
dpkg -i /tmp/cloudflared.deb || apt -y -f install
```

### Bước 3 — 🔴 Sửa MariaDB cho môi trường rootless (QUAN TRỌNG)
Trong proot, UID **thật** là `u0_aXXX` (không phải 0), nên `unix_socket` auth của `root@localhost` **luôn thất bại** (`ERROR 1698`). Phải khởi động `--skip-grant-tables` một lần để tạo user + đổi plugin:
```bash
pkill -9 mariadbd; sleep 2
mkdir -p /run/mysqld; chown -R mysql:mysql /run/mysqld
nohup mariadbd --user=root --skip-grant-tables --skip-networking=0 >/tmp/mdb.log 2>&1 &
sleep 6
mariadb -u root <<'SQL'
FLUSH PRIVILEGES;
CREATE DATABASE IF NOT EXISTS quanlydaotao CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER IF NOT EXISTS "quanlydaotao"@"localhost" IDENTIFIED BY "123123@";
ALTER USER "quanlydaotao"@"localhost" IDENTIFIED VIA mysql_native_password USING PASSWORD("123123@");
GRANT ALL PRIVILEGES ON quanlydaotao.* TO "quanlydaotao"@"localhost";
ALTER USER "root"@"localhost" IDENTIFIED VIA mysql_native_password USING PASSWORD("root123");
FLUSH PRIVILEGES;
SQL
mariadb-admin -u root shutdown || pkill -9 mariadbd
```

### Bước 4 — Lấy code + build
```bash
# Deploy key cho điện thoại (tạo trong Debian, thêm vào GitHub repo → Settings → Deploy keys)
ssh-keygen -t ed25519 -N "" -C "hp-homelab" -f /root/.ssh/id_ed25519
cat /root/.ssh/id_ed25519.pub

mkdir -p /root/Project && cd /root/Project
git clone git@github.com:DucHuynh27/quanlydaotaoit24ab.git
cd quanlydaotaoit24ab && git checkout homelab      # = nhánh nhom1 của nhóm (đã hỗ trợ MariaDB)
chmod +x mvnw
./mvnw clean package -DskipTests                   # ~1 phút, tạo 2 JAR trong */target
```

### Bước 5 — Nạp database từ dump MariaDB của nhóm
Nhóm cung cấp sẵn `database/quanlydaotao-mariadb.sql` (schema + data, ~67 bảng, hash mật khẩu đã fix):
```bash
cd /root/Project/quanlydaotaoit24ab
mariadb -u root -proot123 -e "DROP DATABASE IF EXISTS quanlydaotao;"
mariadb -u root -proot123 < database/quanlydaotao-mariadb.sql
```
Kết quả đúng: ~70 bảng · **users 103** · **role_permissions 518** · admin hash `$2a$10$ymhDhhyE…` (= `admin123`).

### Bước 6 — Sinh JWT secret + PM2 + phiên proot sống lâu + Termux:Boot
```bash
echo "APP_JWT_SECRET=$(head -c 48 /dev/urandom | base64 | tr -d '\n')" > /root/homelab.env
```
Cài `~/start-homelab.sh`, `~/.termux/boot/00-homelab.sh`, `/root/homelab-init.sh`, `/root/homelab/ecosystem.config.js` (xem mục 3). PM2 chạy 2 service bằng **profile `prod`** + env `DB_*` (không còn profile `home`/`application-home.properties`).
Sau đó cài app **Termux:Boot** từ **F-Droid**, mở 1 lần, và **tắt tối ưu pin** cho Termux.

---

## 5. 🧠 BÀI HỌC "XƯƠNG MÁU"

### A. Vì sao phải là Debian + MariaDB (không Docker/MSSQL/Postgres)
1. **Docker/Podman:** F2FS không hỗ trợ `overlay`; kernel chặn `mount --make-rshared /`
   (`remount /, flags: 0x44000: invalid argument`).
2. **MSSQL / Azure SQL Edge:** không có bản ARM64 native; bản Edge chỉ chạy qua Docker → bị chặn.
3. **PostgreSQL:** kernel Android tắt `CONFIG_SYSVIPC=n` → `initdb` văng
   `FATAL: could not create shared memory segment ... shmget`.
4. **MariaDB/MySQL:** dùng mmap/POSIX (không `shmget`) → **chạy được**, nhẹ (~40–60MB RAM).

### B. Proot-distro khác chroot ở điểm chết người
- **Dịch vụ khởi động trong một phiên `proot-distro login` sẽ CHẾT khi phiên đó thoát.**
  → Phải chạy **một phiên proot sống lâu** (`tail -f /dev/null` ở cuối) giữ toàn bộ dịch vụ.
- **`root@localhost` của MariaDB không đăng nhập được** (UID thật ≠ 0) → dùng chiêu `--skip-grant-tables`.
- Vẫn **truy cập/xem tiến trình chéo phiên được** (proot không tạo PID namespace riêng) → `pm2 list` từ phiên khác vẫn thấy daemon của phiên chính.

### C. `crypto.subtle` = "spinner xoay mãi"
Trang login Frontend dùng **WebCrypto (`crypto.subtle.digest`)** cho PKCE. API này **chỉ có trong secure context** (HTTPS hoặc `localhost`). Truy cập qua `http://<IP>` → `crypto.subtle` = `undefined` → `login()` ném lỗi → **spinner quay mãi**. Vì vậy **buộc phải HTTPS** (Cloudflare Tunnel).

### D. Vặt vãnh nhưng tốn thời gian
- Termux mới cài: `curl` báo `cannot locate symbol ... libcurl.so` → `pkg update && pkg upgrade`.
- `pkill -f 'http.server 8000'` **khớp luôn chính lệnh đang chạy** → tự kill shell. Dùng mẹo ngoặc `pkill -f '[h]ttp.server'` hoặc tra PID qua cổng.
- GitHub fork **copy toàn bộ nhánh** khi fork → không cần push lại `nhom1` nếu đã có.

---

## 6. 🔧 VẬN HÀNH 24/7

### Lấy URL công khai (đổi sau mỗi lần reboot)
```bash
~/url.sh
# hoặc
grep -oE 'https://[a-z0-9-]+\.trycloudflare\.com/n1/login.html' \
  /data/data/com.termux/files/usr/var/lib/proot-distro/containers/debian/rootfs/root/homelab-init.out | tail -1
```

### Tài khoản demo
- Quản trị: `admin` / `admin123`
- Còn lại (SV, GV…): `123456`

### Database (dùng chung cho cả 2 service)
- Host/port: `localhost:3306` · DB: `quanlydaotao`
- App user: `quanlydaotao` / `123123@` (chỉ quyền trên DB này)
- Root (quản trị): `root` / `root123` (đã đổi sang `mysql_native_password` để dùng được trong proot)

### Cập nhật code khi nhóm push `nhom1`
Vì `homelab` **= `nhom1`**, cập nhật = kéo `nhom1` mới → đặt `homelab` bằng nó → push fork → `hp` kéo về build lại:
```bash
# 1) Trên LAPTOP:
git fetch origin                                   # lấy commit mới nhất của nhom1 (repo gốc)
git checkout homelab && git reset --hard origin/nhom1
git push --force-with-lease fork homelab           # cập nhật fork để hp kéo về

# 2) Trên hp (Termux -> proot): kéo + build
proot-distro login debian -- /bin/bash -lc '
  cd /root/Project/quanlydaotaoit24ab && git fetch origin && git reset --hard origin/homelab &&
  ./mvnw clean package -DskipTests'

# 3) Nạp lại DB CHỈ KHI nhóm đổi schema/data.
#    Dấu hiệu: file database/quanlydaotao-mariadb.sql thay đổi. Kiểm tra trên LAPTOP:
#      git diff --name-only <commit_cu> origin/nhom1 -- database/
proot-distro login debian -- /bin/bash -lc '
  cd /root/Project/quanlydaotaoit24ab &&
  mariadb -u root -proot123 -e "DROP DATABASE IF EXISTS quanlydaotao;" &&
  mariadb -u root -proot123 < database/quanlydaotao-mariadb.sql'

# 4) Khởi động lại phiên sống lâu (URL tunnel sẽ đổi):
pkill -f start-homelab.sh; nohup ~/start-homelab.sh >/dev/null 2>&1 &
~/url.sh    # lấy URL mới
```

> Mẹo gộp bước 2 trực tiếp từ laptop (nếu muốn): sau bước 1 có thể SSH vào `hp` chạy nguyên khối bước 2 bằng 1 lệnh.

### Xem log / trạng thái
```bash
# PM2 (từ phiên proot bất kỳ)
proot-distro login debian -- /bin/bash -lc \
  'export PM2_HOME=/root/.pm2 PATH=/usr/local/bin:/usr/bin:/bin; pm2 list'
# Log tổng của phiên proot
tail -f /data/data/com.termux/files/usr/var/lib/proot-distro/containers/debian/rootfs/root/homelab-init.out
```

### Cổng đang dùng
`8022` sshd · `3306` MariaDB · `8082` n1-service · `9000` auth-service.

---

## 7. 🗺️ CÒN LẠI / NÂNG CẤP

- **URL cố định:** hiện dùng **quick tunnel** (`trycloudflare`) — URL **đổi mỗi lần chạy/reboot**.
  Muốn cố định → **named tunnel + một domain thật đã nằm trên Cloudflare**.
- **Tên miền:** `huynhduc.id.vn` **chưa tồn tại** trong sổ đăng ký `.vn` (NXDOMAIN); `aiinterview.id.vn` đang gánh site Vercel thật. Cần đăng ký/mua một domain rồi trỏ NS về Cloudflare.
- **Dữ liệu / sao lưu:** schema + data nằm trong `database/quanlydaotao-mariadb.sql` (theo repo). Dữ liệu phát sinh lúc chạy ở `/var/lib/mysql` trong rootfs proot → `mysqldump -u root -proot123 quanlydaotao` định kỳ nếu cần.

---

## 8. 🔋 BẢO VỆ PHẦN CỨNG 24/7

1. **Chống chai pin:** lý tưởng là giới hạn sạc 70–80%. **Không root** thì các app kiểu AccA/ChargeControl không can thiệp được — phương án: dùng **sạc bypass/nguồn nuôi trực tiếp** nếu củ sạc hỗ trợ, hoặc chấp nhận đổi pin định kỳ. (Nếu lấy lại được root, bật lại kernel-module charge control.)
2. **Nhiệt độ:** tháo ốp, kê nghiêng/kê giá kim loại, để nơi thoáng.
3. **Mạng:** Wi-Fi 5GHz cố định DHCP, hoặc Hub Type-C có cổng LAN RJ45.
4. **Chống Android vặt tiến trình:** tắt **tối ưu pin** cho Termux và Termux:Boot; `termux-wake-lock` đã bật trong launcher.
