---
tags:
  - tool/obsidian
  - type/tutorial
aliases:
  - Bí kíp Obsidian
  - Mẹo Obsidian
---
# TỔNG HỢP TÍNH NĂNG TRÊN OBSIDIAN
Obsidian không chỉ là một ứng dụng ghi chú đơn thuần, nó là một "Não bộ thứ hai" (Second Brain) thực thụ. Dưới đây là những tính năng cốt lõi làm nên tên tuổi của Obsidian và cách sử dụng chúng.

---
## 1. Properties (YAML Frontmatter) - Khai báo siêu dữ liệu
Đây là tính năng giúp định hình cấu trúc cho một file Note. Nó nằm ở vị trí cao nhất của file (dòng 1), được bao bọc bởi 3 dấu gạch ngang `---`.

Khi gõ đúng cú pháp, Obsidian sẽ tự động biến nó thành một bảng giao diện đồ họa cực đẹp.

**Ví dụ cách gõ:**
```yaml
---
tags:
  - tool/obsidian
  - type/tutorial
aliases:
  - Bí kíp Obsidian
  - Obsidian Tips
---
```
> [!TIP] Mẹo nhỏ
> Thuộc tính `aliases` giúp file này có nhiều "bí danh". Khi bạn gõ tìm kiếm `Bí kíp Obsidian`, nó sẽ tự động trỏ về đúng file này!

---
## 2. Wikilinks & Mạng nhện (Graph View)
Điểm "ăn tiền" nhất của Obsidian là khả năng liên kết các Note lại với nhau như mạng lưới nơ-ron thần kinh thay vì sắp xếp theo kiểu thư mục cứng nhắc.

*   **Cách dùng:** Bạn chỉ cần gõ 2 dấu ngoặc vuông `[[` là Obsidian sẽ hiện ra danh sách toàn bộ các file để bạn trỏ tới.
*   **Ví dụ:** "Hôm nay tôi vừa học xong kiến thức về [[Docker 🐳]] và setup môi trường bằng [[Quản lý phiên bản nnlt bằng flake]]".

> [!NOTE] Backlinks (Liên kết ngược)
> Khi bạn bấm vào file [[Docker 🐳]], ở cột bên phải sẽ hiện ra phần **Backlinks** cho biết: "À, file *Tính năng Obsidian* vừa mới nhắc đến tôi này!". Tính năng này giúp các kiến thức không bao giờ bị trôi vào dĩ vãng.

---
## 3. Callouts - Khối văn bản nổi bật
Obsidian hỗ trợ làm nổi bật các đoạn chú ý, cảnh báo, hoặc mẹo nhỏ bằng màu sắc cực kỳ bắt mắt (bạn đang dùng tính năng này rất tốt trong các Note của mình).

**Cách dùng và Ví dụ:**

> [!INFO] Thông tin
> Dùng để bổ sung thêm các thông tin bên lề.

> [!SUCCESS] Thành công
> Dùng để đánh dấu một tác vụ đã hoàn thành hoặc một mẹo hay.

> [!WARNING] Cảnh báo
> Dùng để nhắc nhở những lỗi nguy hiểm hoặc dễ mắc phải (như vụ xung đột NPM).

> [!BUG] Bắt lỗi
> Dùng để ghi chú lại các bug hoặc sự cố vừa gặp phải.

---
## 4. Hệ thống Tag lồng nhau (Nested Tags)
Khác với các app ghi chú khác, Tag của Obsidian có thể tạo thành cây thư mục.

*   **Cách dùng:** Dùng dấu gạch chéo `/` để lồng tag.
*   **Ví dụ:**
    *   `#dev/nodejs`
    *   `#dev/java`
    *   `#it/docker`
*   **Tác dụng:** Cột Menu Tag sẽ tự động tạo ra một thư mục cha tên là `dev` chứa `nodejs` và `java` ở bên trong. Giúp bạn dù có 1000 tag vẫn không bị rối mắt.

---
## 5. Trích xuất nhanh một Block (Block References)
Đôi khi bạn không muốn link tới toàn bộ một bài viết dài, mà chỉ muốn link tới đúng 1 dòng (hoặc 1 đoạn văn) trong bài viết đó.

*   **Cách dùng:** Gõ `[[Tên file^]]` (nhớ thêm dấu `^` sau tên file), Obsidian sẽ xổ xuống toàn bộ các đoạn văn trong file đó để bạn chọn.
*   **Ví dụ:** [[Quản lý phiên bản nnlt bằng flake]]` -> Khi click vào, nó sẽ nhảy thẳng đến đúng đoạn văn cụ thể đó thay vì đưa lên đầu trang.

---
## 6. Canvas (Bảng Trắng Vô Cực)
Tính năng này mới được Obsidian bổ sung. Bạn có thể tạo một file Canvas, sau đó kéo thả các file Note, hình ảnh, trang web vào một không gian 2D vô cực và nối chúng bằng các mũi tên (giống như sơ đồ tư duy Mindmap). Rất phù hợp để phác thảo các luồng kiến trúc (Architecture) của dự án.
