---
tags:
  - os/nixos
  - tool/nix
  - dev/nodejs
  - dev/python
  - dev/java
aliases:
  - Nix Flake
  - Setup NixOS
  - Cài môi trường code
---
> [!NOTE]
> **Hai tầng môi trường, chọn theo độ "nặng" của project:**
> - **Nhẹ (CRUD, script, học tập)** → dùng `fnm` cho Node, hoặc không cần gì cho HTML/CSS/JS.
> - **Nặng (native deps, cần cài lại chính xác)** → dùng **flakes + direnv**.
>
> Không cài `nodejs`/`yarn` global. Tầng toolchain cá nhân nằm ở `~/nix-config` (flake này), cấp qua `nix develop`.

> [!TIP]
> **Chiến thuật Workspace**: gom các project cùng ngôn ngữ vào một thư mục cha, đặt `flake.nix` + `.envrc` ở cha. `direnv` (nix-direnv) tự mang môi trường áp xuống các thư mục con.
> ```bash
> mkdir Java-Workspace && cd Java-Workspace
> # tạo flake.nix + .envrc ở đây, rồi cứ tạo project con bên trong bình thường
> ```

## 0. Quyết định dùng gì (Decision Tree)

```
Bắt đầu project Node.js
        │
        ▼
Project có native deps không?
(sharp, canvas, bcrypt, node-gyp, prisma...)
        │
   ┌────┴────┐
  KHÔNG      CÓ
   │          │
   ▼          ▼
fnm +     flake + direnv
.node-version   (hoặc `nix develop` từ ~/nix-config)
   │          │
   ▼          ▼
pnpm init   nix develop
pnpm add    pnpm install
```

| Tiêu chí | fnm | flake + direnv |
|---|---|---|
| Tốc độ setup | tức thì | cần viết/khởi tạo `flake.nix` |
| Reproducibility | thấp (máy nào cũng khác) | 100% khớp |
| Team không dùng Nix | dùng chung được (`.node-version`) | không dùng được |
| Native deps (gcc, libvips…) | tự lo, hay fail | có sẵn trong shell |
| Cognitive load | thấp | cao hơn |

> [!IMPORTANT]
> Đừng dùng `nix develop` cho project CRUD nhẹ (thừa), và đừng dùng `fnm` cho project có native deps nặng (sẽ fail).

---
## 1. NODE.JS (React, Vue, [[NestJS]], TS thuần...)

### 1a. Project nhẹ — `fnm` + `.node-version`

`fnm` (Fast Node Manager) đã cài global và tích hợp sẵn vào Zsh (`modules/home-manager/tui/zsh.nix`), `FNM_DIR=~/.local/share/fnm` đặt trong `home.sessionVariables`.

- **Cài & đặt bản mặc định:**
  ```bash
  fnm install 24.22.1
  fnm default 24.22.1
  ```
- **Ghim version cho từng project / workspace** — tạo `.node-version` ở thư mục dự án (hoặc cha):
  ```bash
  echo "24.22.1" > .node-version
  ```
  `fnm env --use-on-cd` lo việc `cd` vào là tự đổi Node, **không cần** `direnv` hay `flake.nix`. (`fnm` cũng đọc `.nvmrc`.)

### 1b. Project có native deps — flake + direnv

Mẫu đang dùng ở `~/Projects/AI-Interview/flake.nix` (Node 22 + pnpm + fix Prisma trên NixOS):

```nix
devShells.default = pkgs.mkShell {
  packages = with pkgs; [ nodejs_22 pnpm prisma-engines ];
  env.PRISMA_SCHEMA_ENGINE_BINARY = "${pkgs.prisma-engines}/bin/schema-engine";
};
```

`.envrc` tương ứng:
```bash
use flake
```

Hoặc dùng tầng toolchain có sẵn của repo (`~/nix-config`): `nix develop ~/nix-config` cho `fnm`, `gcc`, `gnumake`, `jdk25`, `pnpm`.

### 1c. Tạo project con

- **Frontend (React/Vue bằng Vite)** — sinh vào thư mục hiện tại nhờ dấu `.`:
  ```
  mkdir frontend && cd frontend
  pnpm create vite@latest . --template react-ts
  pnpm install
  pnpm run dev
  ```
- **TS cơ bản / Script (các buổi học):**
  ```
  mkdir buoi4 && cd buoi4
  pnpm init
  pnpm add -D typescript @types/node
  ```

> [!NOTE]
> BẮT BUỘC dùng `pnpm exec` thay vì `npx` để tránh xung đột hệ sinh thái: `pnpm exec tsc --init`

Cấu hình cho dự án TS:
1. Mở `tsconfig.json`, bỏ comment 2 dòng sau:
   ```
   "rootDir": "./src"
   "outDir": "./dist"
   ```
2. Mở `package.json`, thêm vào `"scripts"`:
   ```
   "scripts": {
         "dev": "node --experimental-strip-types src/index.ts",
         "run-ts": "node --experimental-strip-types",
         "build": "tsc",
         "start": "node dist/index.js"
       }
   ```

> [!NOTE]
> Chạy code TS:
> - Chạy trực tiếp: `node --experimental-strip-types <tên-file.ts>`
> - Dịch ra JS rồi chạy: `pnpm build && pnpm start`
> - React/Vue/Angular (Vite): `pnpm run dev`
> - Backend ([[NestJS]]): `pnpm start:dev`

- **Backend ([[NestJS]]):**
  ```
  nest new backend --package-manager pnpm   # đổi "backend" thành tên muốn tạo
  cd backend
  # nếu pnpm chặn thiếu thư viện: pnpm install
  pnpm start:dev
  ```

---
## 2. PYTHON (Data Science, AI, Backend Python)

Python quản lý thư viện bằng **virtualenv**, không phải bằng file lock của pnpm/maven.

- Nếu setup flake ở **thư mục cha**: các project con **dùng chung một rổ thư viện** (tuyệt cho học tập, bài tập).
- Nếu là **project công ty**: KHÔNG setup ở cha — mỗi project tự có flake riêng để không dính chùm thư viện.

Quy trình chuẩn cho 1 project (hoặc 1 workspace học tập):
1. Khởi tạo flake Python:
   ```
   nix flake init -t github:the-nix-way/dev-templates#python
   ```
   Template này đặt `venvDir = ".venv"` và tự tạo virtualenv khi vào shell.
2. Bật direnv (quan trọng — thêm `layout python3` để `pip` không lỗi read-only):
   ```
   echo "use flake" > .envrc
   echo "layout python3" >> .envrc
   direnv allow
   ```

> [!NOTE]
> Từ giờ `pip install <tên>` chạy bình thường; thư viện nằm trong `.venv` của project.
> Neovim tự ưu tiên `./.venv/bin` (xem mục 5) nên `black`/`isort`/`pyright` sẽ lấy đúng bản trong venv.

---
## 3. JAVA (Spring Boot, Java Core)

Giống Node: Maven/Gradle tự quản lý file thư viện, nên dùng chiến thuật **Workspace cấp cha** thoải mái.
```
nix flake init -t github:the-nix-way/dev-templates#java
echo "use flake" > .envrc
direnv allow
```
Flake Java dùng biến `javaVersion` (ví dụ đang là `21`) — sửa số đó để đổi cả JDK + Maven/Gradle.

> [!NOTE]
> Lệnh này chỉ "bơm" JDK/Maven/Gradle vào máy. Sau đó dùng Spring Initializr hoặc IntelliJ/Eclipse để sinh project con bình thường.
> Nếu chỉ cần JDK để nghịch: `nix develop ~/nix-config` đã có `jdk25`.

---
## 4. HTML / CSS / JS (Giao diện tĩnh)

Không cần Nix hay setup gì:
- Trình duyệt hiểu sẵn HTML/CSS.
- Muốn Live Server (tự reload khi lưu): chạy trong môi trường có Node rồi `pnpm dlx live-server`. Trình duyệt tự mở `http://127.0.0.1:8080`; `Ctrl+C` để tắt.

---
## 5. Neovim tự ưu tiên binary của project

`~/nix-config/modules/home-manager/editors/nvim/lua/core/local-bin.lua` prepend `node_modules/.bin`, `.venv/bin`, `venv/bin` vào `PATH` mỗi khi vào Neovim / đổi thư mục. Nhờ đó:

- `typescript-language-server`, `tailwindcss-language-server`, `prettier` → lấy từ `./node_modules/.bin`.
- `pyright`, `black`, `isort` → lấy từ `./.venv/bin`.

Phía LSP được chia 2 tier:
- **Global** (ổn định, không phụ thuộc version project): `nil`, `alejandra`, `stylua`, `lua-language-server`, `bash-language-server`, `dockerfile-language-server`, `yaml-language-server`, `jdt-language-server`, `texlab`, `tree-sitter`, `vscode-langservers-extracted` (html/css/json/eslint).
- **Project-local**: `typescript-language-server`, `tailwindcss-language-server`, `prettier`, `pyright`, `black`, `isort`.

---
## 6. Định hướng mở rộng (chưa triển khai)

- [ ] Thư mục `templates/` trong dotfiles: `nestjs-shell.nix`, `spring-shell.nix`, `fastapi-shell.nix`.
- [ ] Script helper trong `tui/scripts.nix`: `nix-init-nestjs`, `nix-init-spring`, `nix-init-fastapi` (tạo `flake.nix` + `.envrc` + `direnv allow` trong 1 lệnh).
- [ ] Thêm `pnpm` vào `home.packages` nếu muốn bản pnpm do Nix quản thay vì pnpm tự cài ở `~/.local/share/pnpm`.
