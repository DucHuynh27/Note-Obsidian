### NGUYÊN TẮC VÀNG SỐ 1: "Normal Mode là Nhà"
Sai lầm lớn nhất của người chuyển từ VSCode sang là: Cứ ở lì trong chế độ gõ (Insert mode), dùng phím mũi tên lạch cạch để di chuyển và dùng chuột bôi đen.
- Tư duy đúng của Neovim:
	- Hãy coi Normal mode là "trạng thái nghỉ" mặc định của bạn.
	- Chỉ vào Insert mode khi thực sự cần gõ chữ.
	- Gõ xong một cụm từ hoặc một dòng → Lập tức bấm jk để về ngay Normal mode!
- Luật thép rèn luyện: Hãy thử rút chuột ra hoặc dán băng keo vào 4 phím mũi tên trong 3 ngày. Bạn sẽ bị chậm trong 2 ngày đầu,
  nhưng đến ngày thứ 3, tốc độ của bạn sẽ bứt phá không tưởng!
### NGUYÊN TẮC VÀNG SỐ 2: Học "Ngữ pháp" của Vim
Neovim không bắt bạn nhớ hàng trăm phím tắt rời rạc. Nó hoạt động như một ngôn ngữ có cấu trúc: Động từ + Phạm vi + Danh từ
(Text Object).
#### 1. Động từ (Hành động):
- d = Delete (Xóa)
- c = Change (Xóa xong chuyển luôn sang chế độ gõ)
- y = Yank (Copy)
- v = Visual (Bôi đen)
#### 2. Phạm vi:
- i = Inside (Bên trong)
- a = Around (Bao gồm cả dấu bao quanh)
#### 3. Danh từ (Vật thể code):
- w (word - từ) | " (dấu nháy kép) | ' (dấu nháy đơn)
- ( hoặc ) (trong ngoặc tròn) | { hoặc } (trong ngoặc nhọn) | t (thẻ HTML tag)
#### 👉 Ghép lại thành câu lệnh siêu tốc:
- Bạn muốn sửa nội dung trong chuỗi "Hello World"? → Gõ ci" (Change inside quotes). Nó sẽ xóa sạch chữ bên trong và cho bạn gõ chữ mới ngay lập tức!
- Bạn muốn xóa toàn bộ tham số trong hàm func(a, b, c)? → Gõ di( (Delete inside parentheses).
- Bạn muốn xóa cả một khối hàm { ... }? → Gõ da{ (Delete around braces).
- Bạn muốn đổi nội dung trong thẻ `<div>Bấm vào đây</div>?` → Gõ cit (Change inside tag).
- Bạn muốn copy toàn bộ đoạn văn/khối lệnh? → Gõ yap (Yank around paragraph).
### NGUYÊN TẮC VÀNG SỐ 3: Bay nhảy không ma sát
Đừng bấm h/j/k/l từng bước một. Hãy nhảy theo nhịp điệu của code:
1. Nhảy theo từ:
	- w: Nhảy tới đầu từ tiếp theo.
	- b: Nhảy lùi về đầu từ trước đó.
	- e: Nhảy tới đuôi của từ.
2. Nhảy trong dòng (f và t):
	- Bạn muốn nhảy tới dấu = tiếp theo trong dòng? → Gõ f= (Find =).
	- Muốn nhảy tới ký tự đứng ngay trước dấu chấm phẩy ;? → Gõ t; (Till ;).
3. Nhảy toàn màn hình (Dùng Flash đã cài sẵn):
	- Bấm s + gõ chữ bạn đang nhìn thấy trên màn hình → bấm phím gợi ý để con trỏ bay thẳng đến đó trong 0.2 giây!
4. Nhảy giữa các ngữ nghĩa code (LSP):
	- Muốn biết hàm này viết ở đâu? → gd (Go to Definition).
	- Xem xong muốn quay lại chỗ cũ? → Bấm Ctrl + O.
	- Muốn nhảy qua các chỗ có lỗi đỏ? → Bấm ]d / [d.
### NGUYÊN TẮC VÀNG SỐ 4: Phím . (Dấu chấm) - Phép thuật tối thượng
Phím . trong Normal mode có chức năng: Lặp lại chính xác hành động sửa đổi gần nhất của bạn.
- Kịch bản thực tế: Bạn vừa dùng ciw để đổi biến userName thành fullName.
- Giờ bạn thấy ở dòng dưới cũng có chữ userName cần đổi?
	- Bạn chỉ việc di chuột tới chữ đó và bấm đúng một phím . → Nó tự động đổi thành fullName ngay lập tức!
- Kết hợp tìm kiếm với *: Đặt con trỏ vào 1 từ → bấm * (tìm các từ giống nó) → bấm cgn sửa từ đó → sau đó bấm phím . liên tục để đổi hàng loạt từ tiếp theo mà không cần mở hộp thoại Find & Replace!
### 🗺️ LỘ TRÌNH LUYỆN TẬP 14 NGÀY DÀNH CHO BẠN:
- Tuần 1 (Tạo phản xạ không dùng chuột):
	- Mỗi ngày dành 10 phút mở terminal gõ lệnh vimtutor để rèn phản xạ cơ bản.
	- Bắt buộc tay bấm jk ngay sau khi gõ xong code.
	- Luyện thuần thục bộ ba thần thánh: ci", ci(, ci{.
- Tuần 2 (Tận dụng bộ đồ chơi đã cài):
	- Quản lý file: Dùng <leader>e (Oil) để tạo/đổi tên file như gõ text, không mở file manager ngoài.
	- Nhảy file: Bỏ thói quen tìm file bằng mắt, gõ <leader>ff gõ 2-3 ký tự tên file để mở.
	- Quản lý Git: Thay vì gõ lệnh terminal, bấm <leader>gg (LazyGit) để stage và commit code.