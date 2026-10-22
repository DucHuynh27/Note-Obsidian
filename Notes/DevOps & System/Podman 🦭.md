---
tags:
  - it/podman
  - it/docker
  - it/devops
  - type/tutorial
aliases:
  - Học Podman
  - Cơ bản Podman
  - Chuyển Docker sang Podman
---
## 1. Hai Khái Niệm Quan Trọng Nhất

Để dễ hình dung, hãy tưởng tượng Podman/Docker giống như việc đi mua một chiếc đĩa game:

- **Image (Cái đĩa game):** Là một file nén đóng gói sẵn mọi thứ (Hệ điều hành, mã nguồn, thư viện,...). Image mang tính chất **chỉ đọc (read-only)**. Ví dụ: Image của MS SQL Server, Image của Ubuntu, Image của MySQL, NodeJS...
- **Container (Máy chơi game):** Bỏ cái đĩa (Image) vào máy ảo để nó chạy lên, nó biến thành một **Container**. Một Image (1 cái đĩa) có thể tạo ra được hàng trăm cái Container (hàng trăm cái máy đang chơi game đó) chạy độc lập với nhau.

---

## 2. Vì Sao Chuyển Từ Docker Sang Podman?

- **Daemonless (Không cần Daemon chạy ngầm):** Docker luôn cần một tiến trình `dockerd` chạy nền liên tục ngốn RAM/CPU kể cả khi không dùng. Podman chạy trực tiếp qua tiến trình fork/exec, tắt hết container là máy nghỉ ngơi 100%.
- **Rootless & Bảo mật cao:** Podman có thể chạy container mà không cần quyền `root` (User hinne tự quản lý container mà không sợ lộ bảo mật hệ thống).
- **Tương thích hoàn toàn (Docker Compatible):** NixOS hỗ trợ `virtualisation.podman.dockerCompat = true`, gõ `docker` thực chất là gọi `podman`, các công cụ như `lazydocker` (`lzd`) vẫn dùng bình thường.
- **Podman Pods:** Có thể gom nhiều container vào 1 Pod chung localhost giống Kubernetes.

---

## 3. Các Lệnh Vòng Đời Cơ Bản Trong Podman

> [!NOTE]
> Do Podman yêu cầu phân giải tên image chuẩn (không tự ngầm định prefix như Docker), nên dùng tên đầy đủ như `docker.io/library/nginx` hoặc `docker.io/library/mysql:8.0`.

### Khởi tạo và chạy

```bash
# Lệnh tổng quát
podman run [tùy_chọn] [tên_image]

# Ví dụ: Chạy một con web server Nginx
podman run -d -p 8080:80 --name web_cua_tui docker.io/library/nginx
```

> [!TIP]
> **Giải ngố các Tùy chọn (Flags):**
>
> - `-d` (Detached): Báo Podman đem thùng này chạy ngầm, trả lại con trỏ terminal để gõ lệnh khác.
> - `--name`: Đặt tên gợi nhớ cho Container (ví dụ: `web_cua_tui`).
> - `-p [Port_ngoài]:[Port_trong]`: Đục cổng giao tiếp với bên ngoài (ví dụ `-p 8080:80` lấy cổng `8080` của máy tính cắm vào `80` container).

### Quản lý các Container

```bash
# Xem các Container ĐANG CHẠY
podman ps

# Xem TẤT CẢ Container (kể cả những cái đã tắt)
podman ps -a

# Xem mức ngốn RAM / CPU realtime
podman stats
```

### Dừng, Bật lại và Xóa

```bash
# Tắt 1 container
podman stop web_cua_tui

# Mở lại 1 container đã tắt
podman start web_cua_tui

# Xóa một Container (phải stop trước)
podman rm web_cua_tui

# Buộc tắt và xóa luôn
podman rm -f web_cua_tui

# Xóa image không dùng
podman rmi docker.io/library/nginx
```

---

## 4. Quản Lý Dữ Liệu: #VOLUME & #BIND_MOUNT

> [!WARNING]
> **Vấn đề nguy hiểm:** Bản chất của Container là "tạm thời". Nếu chạy Database bằng Podman/Docker mà không map volume, khi xóa container thì toàn bộ Database sẽ **MẤT SẠCH vĩnh viễn**!

Để giải quyết, chúng ta có 2 phương pháp gắn ổ cứng:

### 1. Bind Mount (Đường dẫn tĩnh trên Host - Khuyên dùng trên NixOS) ⭐

- Map thẳng thư mục thật trên máy vào container.
- **Cú pháp:** `-v /var/lib/mysql_data:/var/lib/mysql`
- **Ưu điểm lớn:**
  - Toàn quyền kiểm soát vị trí lưu trữ (dễ backup ra USB Ventoy cứu hộ).
  - Độc lập hoàn toàn với Podman/Docker. Chuyển đổi giữa Docker $\leftrightarrow$ Podman không lo mất hay phải convert volume.
  - Không sợ bị lệnh `podman system prune --volumes` dọn dẹp nhầm.

### 2. Named Volume (Do Podman tự quản lý)

- **Cú pháp:** `-v mssql_data:/var/opt/mssql`
- Podman tự tạo một thư mục quản lý ngầm tại `/var/lib/containers/storage/volumes/`.
- Sẽ hiển thị trong tab Volumes của `podman-tui` hoặc `podman volume ls`.

---

## 5. Cấu Hình Declarative Database Trên NixOS (`databases.nix`)

Thay vì gõ lệnh `run` thủ công, trên NixOS chúng ta khai báo toàn bộ Database dưới dạng **OCI Containers chạy qua systemd**:

- Cấu hình trong file `nixos/databases.nix`:
  - Backend: `virtualisation.oci-containers.backend = "podman"`
  - MS SQL Server 2022: Cổng `1433`, volume `/var/lib/mssql_data:/var/opt/mssql`
  - MySQL 8.0: Cổng `3306`, volume `/var/lib/mysql_data:/var/lib/mysql`
  - MongoDB 7.0: Cổng `27017`, volume `/var/lib/mongo_data:/data/db`

### Bật / Tắt Database cực nhanh bằng Zsh Alias (Không cần mật khẩu):

- **MS SQL Server:** `sql-start` | `sql-stop` | `sql-status` | `sql-log`
- **MySQL:** `mysql-start` | `mysql-stop` | `mysql-status` | `mysql-log`
- **MongoDB:** `mongo-start` | `mongo-stop` | `mongo-status` | `mongo-log`

---

## 6. Giao Diện Trực Quan (TUI) Với Lazydocker

Nhờ cấu hình `DOCKER_HOST = "unix:///run/podman/podman.sock"`, công cụ TUI **Lazydocker** hoạt động hoàn hảo với Podman:

- Chỉ cần gõ:
  ```bash
  lzd
  ```
- Bấm các phím:
  - `[` / `]`: Chuyển đổi giữa Containers, Images, Volumes.
  - `j` / `k`: Lên / xuống chọn container.
  - `Enter`: Xem logs thời gian thực.
  - `s` / `d` / `r`: Start / Stop / Restart container.
  - `x`: Mở menu thao tác nhanh.
  - `q`: Thoát.

