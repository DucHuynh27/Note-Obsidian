---
tags:
  - tool/editor
  - tool/neovim
  - type/cheatsheet
aliases:
  - Cẩm nang Neovim
  - Neovim Shortcuts
  - Neovim Guide
---

# Cẩm nang Toàn diện về Neovim

> **Ghi chú:** Phím `<leader>` trên hệ thống của bạn được gán là phím **`Space` (Dấu cách)**.

---

## 1. Tổng quan các Tính năng & Plugin đang có

Hệ thống Neovim được tối ưu theo tiêu chí: **Khởi động siêu tốc, tiêu tốn cực ít RAM, không rác, nhưng trang bị trọn gói cho JS/TS, React Native, Java, Python và DevOps**.

### Giao diện & Tiện ích
* **`oil.nvim` (+ `oil-git` + `oil-lsp-diagnostics`)**: Quản lý file độc đáo phong cách bộ nhớ đệm (Buffer). Cho phép sửa tên, tạo, xóa file y hệt như đang chỉnh sửa văn bản. Tự động hiển thị trạng thái Git (màu sắc, ký hiệu `+`, `~`, `D`) và dán cờ báo lỗi LSP trực tiếp trên cây thư mục.
* **`lualine.nvim`**: Thanh trạng thái đáy màn hình hiển thị Mode, Branch Git, loại file và **thời tiết/nhiệt độ thời gian thực** (cập nhật ngầm mỗi 30 phút).
* **`bufferline.nvim`**: Quản lý danh sách file đang mở dạng Tab ở đỉnh màn hình. Hoạt động ở chế độ ngủ (ẩn thanh Tab) và chỉ xuất hiện khi bạn bấm chuyển Tab (`Shift + H / L`).
* **`indent-blankline.nvim`**: Đường gióng thụt lề 7 màu cầu vồng giúp phân biệt rõ ràng các tầng code lồng nhau.
* **`dressing.nvim`**: Chuyển đổi toàn bộ menu chọn lựa và hộp thoại nhập liệu mặc định thành cửa sổ nổi bo góc sang trọng.

### Tìm kiếm & Điều hướng
* **`fzf-lua`**: Công cụ tìm kiếm siêu tốc viết bằng C/Rust. Dùng để tìm file theo tên hoặc quét sâu từng đoạn text trong dự án với tốc độ tức thì.
* **`flash.nvim`**: Tính năng dịch chuyển con trỏ chuột siêu tốc thế hệ mới (thay thế cho Hop), chỉ cần gõ 2 ký tự là bay đến vị trí bất kỳ trên màn hình.
* **`aerial.nvim`**: Thanh mục lục thông minh bên lề phải, tóm tắt toàn bộ danh sách Class, Hàm, Biến trong file hiện tại.

### Lập trình & Gợi ý Code (LSP & Formatter)
* **`nvim-lspconfig`**: Máy chủ phân tích mã nguồn cho mọi ngôn ngữ:
  * **JS / TS**: `ts_ls` (IntelliSense, định nghĩa, kiểm tra kiểu dữ liệu).
  * **React & React Native**: `eslint` (soi chuẩn cú pháp, cảnh báo code bẩn), `tailwindcss` (gợi ý class Tailwind).
  * **HTML / CSS**: `html`, `cssls`.
  * **Python**: `pyright` (bắt lỗi logic, type) kết hợp `ruff` (linter/formatter siêu tốc số 1 hiện nay).
  * **DevOps / Hệ thống**: `dockerls`, `yamlls`, `jsonls`, `bashls`, `nil_ls` (Nix).
* **`nvim-jdtls`**: Bộ công cụ độc quyền cho Java (tự động tổ chức Import, bọc biến, bọc hàm).
* **`nvim-cmp` + `LuaSnip` + `friendly-snippets`**: Hệ thống gợi ý code đa nguồn (LSP, Buffer, Path, Snippets).
* **`conform.nvim`**: Tự động căn chỉnh format code chuẩn mực quốc tế (`<leader>fm`).

### Git & Terminal
* **`gitsigns.nvim`**: Hiển thị vạch màu xanh lá / vàng / đỏ ở lề trái báo hiệu các dòng vừa thêm / sửa / xóa.
* **`toggleterm.nvim`**: Mở cửa sổ Terminal ở đáy màn hình phong cách VSCode.

---

## 2. Bảng Tra cứu Phím tắt Toàn tập

### 1. Quản lý File & Tab
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`<leader>e`** | **Mở Oil (Quản lý file):** Danh sách file hiện ra như một trang văn bản. Di chuyển tới dòng file bấm `dd` để xóa, bấm `cw` để đổi tên, sau đó gõ `:w` để lưu lại thay đổi vào ổ cứng! Bấm phím `-` để quay lại thư mục cha. |
| **`Shift + H`** | Nhảy sang Tab bên trái. |
| **`Shift + L`** | Nhảy sang Tab bên phải. |
| **`<leader>x`** | Đóng Tab đang mở hiện tại. |

### 2. Tìm kiếm & Bay nhảy siêu tốc
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`s`** (ở Normal mode) | **Flash:** Bấm `s` + gõ từ cần tìm $\rightarrow$ màn hình làm mờ code và nổi lên các chữ cái nhãn $\rightarrow$ bấm tiếp chữ cái đó để con trỏ bay thẳng đến đích. |
| **`<leader>ff`** | Tìm kiếm file theo tên trong toàn bộ dự án. |
| **`<leader>fg`** | Quét nội dung text (Live Grep) trong toàn bộ file của dự án. |
| **`<leader>fb`** | Chuyển đổi qua lại giữa các file đang được mở trong RAM (Buffers). |

### 3. Săn lỗi (Diagnostics) & Sửa lỗi
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`gl`** | Bật cửa sổ nổi (Float window) xem giải thích chi tiết lỗi tại vị trí con trỏ. |
| **`]d`** / **`[d`** | Nhảy nhanh đến vị trí lỗi tiếp theo / lỗi trước đó. |
| **`<leader>dd`** | Mở bảng danh sách xem **tất cả các lỗi** trong file hiện tại. |
| **`<leader>ca`** | Mở **Code Actions** (menu gợi ý sửa lỗi tự động hoặc Refactor của LSP). |

### 4. Thao tác Code (LSP) & Giao diện
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`gd`** | Nhảy thẳng đến nơi khai báo biến/hàm gốc (Go to Definition). Bấm `Ctrl + O` để nhảy lùi lại chỗ cũ. |
| **`gr`** | Tìm tất cả các vị trí đang gọi biến/hàm này (References). |
| **`gI`** | Nhảy đến Implementation của Interface. |
| **`K`** (Shift + k) | Xem tài liệu, kiểu dữ liệu và mô tả của hàm (Hover). |
| **`<leader>cr`** | Đổi tên biến/hàm đồng loạt trong toàn bộ file (Rename). |
| **`<leader>fm`** | Tự động format căn chỉnh code cho thẳng hàng đẹp mắt. |
| **`<leader>a`** | Bật/Tắt bảng mục lục hàm và biến bên lề phải (Outline). |
| **`gcc`** | Đóng/mở comment dòng hiện tại (hoặc bôi đen nhiều dòng rồi bấm `gc`). |
| **`jk`** | Thoát chế độ gõ (Insert mode) về Normal mode tức thì mà không cần với tay bấm ESC. |

### 5. Cửa sổ (Split) & Terminal
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`<leader>sv`** | Chia đôi màn hình theo chiều Dọc (Split Vertical). |
| **`<leader>sh`** | Chia đôi màn hình theo chiều Ngang (Split Horizontal). |
| **`<leader>se`** | Cân bằng lại kích thước của tất cả các cửa sổ đang chia. |
| **`<leader>sx`** | Đóng cửa sổ chia hiện tại. |
| **`Ctrl + \`** | Bật/Tắt Terminal ở đáy màn hình. Khi đang ở trong Terminal, bấm `jk` để thoát chế độ gõ và cuộn chuột lên xem Log. |

### 6. Git & Siêu năng lực Java
| Phím tắt | Chức năng & Cách sử dụng |
| :--- | :--- |
| **`]h`** / **`[h`** | Nhảy đến đoạn code có thay đổi so với Git tiếp theo / trước đó. |
| **`<leader>hp`** | Xem trước (Preview) đoạn code gốc đã bị sửa/xóa trước khi Commit. |
| **`<leader>jo`** *(Java)* | **Organize Imports:** Tự động xóa import thừa, thêm các import thiếu ở đầu file. |
| **`<leader>jv`** *(Java)* | Bôi đen một đoạn code $\rightarrow$ bấm để tách thành một Biến cục bộ. |
| **`<leader>jc`** *(Java)* | Bôi đen một đoạn code $\rightarrow$ bấm để tách thành một Hằng số (Constant). |
| **`<leader>jm`** *(Java)* | Bôi đen một khối lệnh $\rightarrow$ bấm để gói thành một Hàm (Method) riêng biệt. |

---

## 3. Kho Đoạn code mẫu (Snippets)

> **Cách dùng:** Trong lúc gõ code ở Insert mode, bạn gõ từ khóa viết tắt $\rightarrow$ menu gợi ý của `nvim-cmp` nổi lên $\rightarrow$ bấm **`Enter`** (hoặc `Tab`) để bung toàn bộ đoạn code mẫu ra.

### 1. React & React Native (`.tsx`, `.jsx`)
* **`rnfe`**: Tạo khung component React Native chuẩn (có sẵn `View`, `Text`, `StyleSheet` và `export default`).
* **`rfc`**: Tạo khung component React (Web) dạng Function tiêu chuẩn.
* **`usf`** / **`useState`**: Sinh nhanh khai báo State: `const [state, setState] = useState(initialState)`.
* **`uef`** / **`useEffect`**: Sinh nhanh hook Effect: `useEffect(() => { ... }, [])`.
* **`clg`**: Sinh nhanh lệnh in ra console: `console.log(...)`.

### 2. JavaScript / TypeScript (`.js`, `.ts`)
* **`for`**: Vòng lặp đếm cơ bản `for (let i = 0; i < array.length; i++)`.
* **`forof`**: Vòng lặp duyệt phần tử mảng `for (const item of object)`.
* **`anfn`**: Hàm mũi tên ẩn danh: `(param) => { ... }`.
* **`try`**: Khối bắt lỗi `try { ... } catch (error) { ... }`.

### 3. HTML / CSS (`.html`, `.css`)
* **`!`** hoặc **`html:5`**: Sinh bộ khung chuẩn HTML5 đầy đủ thẻ `<!DOCTYPE html>`, `head`, `meta`, `body`.
* **`link:css`**: Sinh thẻ liên kết file CSS: `<link rel="stylesheet" href="style.css">`.
* **`script:src`**: Sinh thẻ liên kết file Script: `<script src="..."></script>`.

### 4. Java (`.java`)
* **`main`**: Sinh hàm thực thi chính: `public static void main(String[] args) { ... }`.
* **`sout`** hoặc **`sysout`**: Sinh lệnh in ra màn hình: `System.out.println(...);`.

### 5. Python (`.py`)
* **`def`**: Sinh khung định nghĩa hàm: `def function_name(args):`.
* **`ifmain`**: Sinh khối điều kiện chạy file: `if __name__ == "__main__":`.
