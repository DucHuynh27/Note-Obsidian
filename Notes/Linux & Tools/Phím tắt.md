---
tags:
  - os/linux
  - tool/terminal
  - tool/editor
  - tool/cli
  - tool/git
  - tool/docker
  - type/cheatsheet
aliases:
  - Phím tắt Neovim
  - Phím tắt Kitty
  - Phím tắt WezTerm
  - Phím tắt Yazi
  - Phím tắt LazyGit
  - Phím tắt LazyDocker
---
## 1. Phím tắt WezTerm (Quản lý Pane/Cửa sổ Terminal)
- **`Ctrl + Shift + Enter`** : Chia ngang (Thường dùng để mở Server chạy ngầm bên cạnh Editor)
- **`Ctrl + Shift + O`** : Chia dọc (Hợp lý để mở Log chạy dài xuống dưới)
- **`Ctrl + Shift + H / J / K / L`** : Di chuyển con trỏ qua lại giữa các Pane (giống phong cách Vim)
- **`Ctrl + Shift + Z`** : Phóng to toàn màn hình một Pane (Đổi sang layout Stack để tập trung gõ code), ấn lại để thu nhỏ.
- **`Ctrl + Shift + W`** (hoặc gõ `exit`) : Đóng Pane/Tab hiện tại
- **`Ctrl + Shift + T`** : Mở Tab mới
- **`Ctrl + Tab`** / **`Ctrl + Shift + Tab`** : Nhảy qua lại giữa các Tab (có thể dùng `Ctrl + Shift + Trái / Phải`)
- **`Ctrl + Shift + F`** : Bật thanh tìm kiếm chữ. Sau khi gõ, dùng phím **Mũi tên Lên / Xuống** (hoặc `Ctrl+P`/`Ctrl+N`) để nhảy qua lại giữa các kết quả. Bấm `Esc` để thoát.
---
## 2. Phím tắt Neovim (Hệ thống Which-Key & Đầy đủ Plugins)
> **Ghi chú**: Phím **`<leader>`** được gán mặc định là phím **`Space`** (Phím cách). Nhấn giữ `Space` một chút sẽ hiển thị menu gợi ý phím tắt `Which-Key`.
### 2.1. Thao tác Cơ bản & Chỉnh sửa Dòng
| Phím tắt | Chế độ | Chức năng |
| :--- | :--- | :--- |
| **`jk`** | Insert | Thoát chế độ gõ (về Normal mode) siêu tốc, không cần với tay bấm `ESC`. |
| **`s`** | Normal / Visual | **Flash Jump**: Bấm `s` + gõ ký tự, màn hình gán nhãn để nhảy tức thì đến vị trí đó. |
| **`J`** | Visual | Kéo cả khối dòng đang chọn **dịch chuyển xuống** (tự căn chỉnh indent). |
| **`K`** | Visual | Kéo cả khối dòng đang chọn **dịch chuyển lên** (tự căn chỉnh indent). |
| **`<` / `>`** | Visual | Thụt lề trái/phải và **giữ nguyên vùng chọn** để thụt tiếp nhiều lần. |
| **`gcc`** | Normal | Bật / Tắt comment trên dòng hiện tại. |
| **`gc`** | Visual | Bật / Tắt comment cho cả đoạn code đang bôi đen. |
| **`<leader>nh`** | Normal | **Xóa highlight**: Xóa các vệt màu vàng sau khi tìm kiếm xong. |
### 2.2. Nhóm `<leader>s`: Quản lý Cửa sổ (Splits)
| Phím tắt | Chức năng |
| :--- | :--- |
| **`<leader>sv`** | Chia đôi màn hình theo chiều **Dọc** (Split Vertical). |
| **`<leader>sh`** | Chia đôi màn hình theo chiều **Ngang** (Split Horizontal). |
| **`<leader>se`** | **Làm đều** lại kích thước tất cả các cửa sổ đang mở. |
| **`<leader>sx`** | **Đóng** cửa sổ chia (split) hiện tại. |
### 2.3. Nhóm `<leader>f`: Tìm kiếm & Định dạng (Find)
| Phím tắt | Chức năng |
| :--- | :--- |
| **`<leader>ff`** | Mở bảng tìm kiếm file theo tên (`FzfLua files`). |
| **`<leader>fg`** | Tìm kiếm nội dung đoạn chữ/code trong toàn bộ dự án (`FzfLua live_grep`). |
| **`<leader>fb`** | Chuyển đổi qua lại giữa các file đang mở trong RAM (`FzfLua buffers`). |
| **`<leader>fm`** | **Auto-format code**: Tự động căn chỉnh code chuẩn đẹp (Prettier, Alejandra, Stylua, Black,...). |
### 2.4. Nhóm `<leader>c` & LSP: Lập trình & Thông minh (Code / Intelligence)
| Phím tắt | Chức năng |
| :--- | :--- |
| **`gd`** | **Go to Definition**: Nhảy ngay đến định nghĩa của biến/hàm/component. |
| **`gr`** | **References**: Tìm toàn bộ danh sách các nơi đang gọi hàm/biến này trong dự án. |
| **`gI`** | **Implementation**: Nhảy đến interface implementation của hàm/class. |
| **`K`** | **Hover Docs**: Xem tài liệu, kiểu dữ liệu và docstring của hàm ngay tại con trỏ. |
| **`<leader>ca`** | **Code Action**: Mở danh sách gợi ý sửa lỗi tự động / import còn thiếu (bóng đèn 💡). |
| **`<leader>cr`** | **Rename**: Đổi tên biến/hàm an toàn trên toàn bộ tất cả các file dự án. |
| **`<leader>cs`** | Mở danh sách các Symbols (hàm, class, biến) trong file qua bảng Trouble. |
| **`<leader>cl`** | Mở danh sách LSP Definitions / References qua bảng Trouble. |
| **`<leader>a`** | **Aerial Outline**: Bật/Tắt cây cấu trúc hàm/biến dạng mục lục ở bên phải. |
### 2.5. Nhóm `<leader>d` & `<leader>x`: Săn lỗi (Diagnostics & Trouble)
| Phím tắt | Chức năng |
| :--- | :--- |
| **`gl`** | Xem chi tiết lỗi/cảnh báo dạng cửa sổ nổi (Float window) tại con trỏ. |
| **`[d` / `]d`** | Nhảy lùi về lỗi trước đó / Nhảy tới lỗi tiếp theo. |
| **`<leader>dd`** | Mở bảng danh sách xem **toàn bộ lỗi** trong file hiện tại (`FzfLua`). |
| **`<leader>xx`** | Mở bảng **Trouble**: Quản lý toàn bộ lỗi & cảnh báo của cả dự án ở đáy màn hình. |
| **`<leader>xX`** | Mở bảng **Trouble**: Lọc danh sách lỗi chỉ trong file đang mở. |
### 2.6. Nhóm `<leader>g`: Quản lý Git & LazyGit
| Phím tắt | Chức năng |
| :--- | :--- |
| **`<leader>gg`** | **Mở LazyGit**: Giao diện quản lý Git toàn màn hình cực mạnh ngay trong Neovim. |
| **`]h`** | Nhảy đến đoạn code thay đổi (Git Hunk) tiếp theo. |
| **`[h`** | Nhảy lùi về đoạn code thay đổi (Git Hunk) trước đó. |
| **`<leader>hp`** | **Preview Hunk**: Xem trước đoạn code gốc đã bị sửa/xóa trước khi commit. |
### 2.7. Nhóm `<leader>q`: Phiên làm việc (Session - Persistence)
| Phím tắt         | Chức năng                                                                             |
| :--------------- | :------------------------------------------------------------------------------------ |
| **`<leader>qs`** | **Khôi phục Session**: Mở lại toàn bộ các tabs, buffers, layout của thư mục hiện tại. |
| **`<leader>ql`** | **Khôi phục Session gần nhất**: Mở lại phiên làm việc vừa đóng trước đó.              |
| **`<leader>qd`** | **Dừng lưu Session**: Không lưu trạng thái phiên làm việc khi thoát Neovim lần này.   |
### 2.8. Giao diện, Quản lý File & Tab
| Phím tắt | Chức năng |
| :--- | :--- |
| **`<leader>e`** | **Oil File Manager**: Mở và quản lý thư mục như một file text (tạo/xóa/sửa tên file rồi `:w`). |
| **`<leader>un`** | Xóa ngay lập tức mọi thông báo nổi (Notification dismiss). |
| **`Shift + H`** | Nhảy sang Tab bên trái (Buffer trước). |
| **`Shift + L`** | Nhảy sang Tab bên phải (Buffer kế tiếp). |
| **`<leader>x`** | Đóng Tab/Buffer hiện tại. |
### 2.9. Terminal tích hợp
| Phím tắt | Ngữ cảnh | Chức năng |
| :--- | :--- | :--- |
| **`Ctrl + \`** | Bất kỳ đâu | Bật/Tắt thanh Terminal nổi ngang ở dưới đáy màn hình (`ToggleTerm`). |
| **`jk`** | Trong Terminal | Thoát chế độ gõ dòng lệnh, chuyển sang Normal mode để cuộn chuột xem Log. |
### 2.10. Siêu năng lực Java (`nvim-jdtls`)
| Phím tắt | Chức năng |
| :--- | :--- |
| **`<leader>jo`** | **Java Organize Imports**: Tự động gỡ thư viện thừa, nạp thư viện còn thiếu. |
| **`<leader>jv`** | **Extract Variable**: Tách đoạn code đang bôi đen thành Biến cục bộ. |
| **`<leader>jc`** | **Extract Constant**: Tách đoạn code đang bôi đen thành Hằng số (Constant). |
| **`<leader>jm`** | **Extract Method**: Gói đoạn code đang bôi đen thành một Hàm/Phương thức mới. |

---
## 3. Thao tác với Yazi (Terminal File Manager)
- **`Space`** (Phím cách) : Chọn hoặc Bỏ chọn file (để thao tác nhiều file cùng lúc)
- **`y`** : Copy (Yank)
- **`x`** : Cắt (Cut)
- **`p`** : Dán (Paste)
- **`d`** : Xóa bỏ file vào thùng rác (Trash)
- **`D`** (Shift + d) : Xóa vĩnh viễn không thể khôi phục!
- **`a`** : Tạo file hoặc thư mục mới (gõ thêm `/` ở cuối tên để tạo thư mục, ví dụ `tailieu/`)
- **`r`** : Đổi tên file (Rename)
- **`/`** hoặc **`f`** : Lọc/tìm kiếm nhanh file trong thư mục hiện tại
- **`s`** : Tìm kiếm nội dung file (tích hợp `fd`/`ripgrep`)
### Tác vụ Nâng cao (Custom & Task Manager)
- **`w`** : **Mở Task Manager** (Để xem tiến độ % của các việc đang chạy ngầm như Copy/Paste, Nén/Giải nén, hoặc để Hủy ngang bằng phím `x`)
- **`e`** : **Giải nén** file (zip, rar, 7z...) tuôn hết ra thư mục hiện hành
- **`E`** (Shift + e) : **Giải nén nâng cao** (Sẽ hiện hộp thoại hỏi bạn muốn xả nén vào đâu)
- **`c`** : **Nén file** (Cách dùng: Bấm `Space` chọn nhiều file -> Bấm `c` -> Đặt tên file nén `ten_file.zip` -> Enter)
- **`g`** rồi bấm **`u`** : **Truy cập USB** (Nhảy nhanh vào thư mục `/run/media/hinne/` để xem USB)
---
## 4. Phím tắt LazyGit (Quản lý Git trực quan)
> Khởi động bằng lệnh `lazygit` trên Terminal hoặc bấm **`<leader>gg`** ngay trong Neovim.
### Điều hướng & Cơ bản
- **`1` - `5`** : Nhảy nhanh giữa các panel:
  - **`1`** : Status (Trạng thái repo / branch)
  - **`2`** : Files (Các file có thay đổi)
  - **`3`** : Branches (Danh sách nhánh Git)
  - **`4`** : Commits (Lịch sử commit)
  - **`5`** : Stash (Các stash đang lưu)
- **`h` / `l`** (hoặc Mũi tên Trái/Phải): Di chuyển qua lại giữa các panel.
- **`j` / `k`** (hoặc Mũi tên Lên/Xuống): Di chuyển giữa các mục trong danh sách.
- **`?`** : Mở bảng trợ giúp tra cứu toàn bộ phím tắt.
- **`q`** : Thoát LazyGit.
### Thao tác với File (Panel Files - `2`)
- **`Space`** : Stage / Unstage một file (đưa vào hoặc bỏ khỏi vùng chuẩn bị commit).
- **`a`** : Stage / Unstage **toàn bộ** các file.
- **`c`** : Mở hộp thoại nhập Commit message.
- **`C`** (Shift + c) : Commit với công cụ mở rộng (nếu có cấu hình).
- **`d`** : Mở menu xóa/hủy bỏ các thay đổi của file (Discard changes).
- **`Enter`** : Xem chi tiết diff của file và stage từng dòng (line-by-line stage).
### Thao tác với Branch & Remote (Panel Branches - `3`)
- **`Space`** : Checkout (chuyển sang) branch được chọn.
- **`n`** : Tạo một branch mới từ branch hiện tại.
- **`P`** (Shift + p) : **Push** commit lên remote repo.
- **`p`** : **Pull** code mới nhất từ remote về.
- **`F`** (Shift + f) : Fetch code từ remote.
- **`M`** (Shift + m) : Merge branch được chọn vào branch hiện tại.
### Thao tác với Commit (Panel Commits - `4`)
- **`s`** : Squash commit được chọn gộp vào commit bên dưới nó.
- **`r`** : Đổi lại tên commit (Reword).
- **`d`** : Xóa bỏ commit (Drop commit).
- **`t`** : Đảo ngược commit (Revert commit).
---
## 5. Phím tắt LazyDocker (Quản lý Docker trực quan)
> Khởi động bằng lệnh `lazydocker` trên Terminal.
### Điều hướng Panels
- **`1` - `4`** : Nhảy nhanh đến các bảng:
  - **`1`** : Projects / Services (Docker Compose)
  - **`2`** : Containers (Các container đang có)
  - **`3`** : Images (Các image đã tải về)
  - **`4`** : Volumes (Các volume lưu trữ)
- **`[` / `]`** : Chuyển qua lại các tab chi tiết bên phải (Logs, Stats, Config, Env, Top,...).
- **`Enter`** : Phóng to xem toàn màn hình tab hiện tại (ví dụ xem toàn màn hình Live Logs). Bấm `Esc` để thu lại.
- **`?`** : Mở bảng tra cứu phím tắt chi tiết.
- **`q`** : Thoát LazyDocker.
### Thao tác với Container (Panel Containers - `2`)
- **`d`** : Xóa container (`docker rm`).
- **`s`** : Dừng container (`docker stop`).
- **`r`** : Khởi động lại container (`docker restart`).
- **`p`** : Tạm dừng / Tiếp tục chạy (`Pause / Unpause`).
- **`a`** : Gắn terminal vào container (`docker attach`).
- **`e`** : Mở Terminal/Shell trực tiếp bên trong container (`docker exec -it ... /bin/sh`).
- **`m`** : Xem luồng logs chi tiết của container.
### Thao tác với Images & Volumes
- **`d`** (ở Panel Images/Volumes): Xóa image hoặc volume được chọn.
- **`c`** : Dọn dẹp thùng rác Docker (**Prune** các container/image/volume không còn sử dụng).
- **`b`** : Mở menu thực hiện lệnh hàng loạt (Bulk actions: dừng tất cả, xóa tất cả,...).