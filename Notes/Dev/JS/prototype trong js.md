# Cần hiểu
## *Prototype*
- Mọi hàm và đối tượng trong JS đều có 1 liên kết ẩn được gọi là `Prototype`
- Có thể dùng `Prototype` để thêm thuộc tính và phương thức vào 1 hàm khởi tạo
- Các đối tượng kế thừa thuộc tính và phương thức đó từ `Prototype`
- Sau khi khai báo hàm khởi tạo, thêm thuộc tính hoặc phương thức bổ sung bằng `Prototype` : `object.prototype.method/properties = parameter`. Đối tượng k có phương thức A nhưng có thể kế thừa từ `Prototype` để có phương thức A
- Nếu giá trị `Prototype` bị đổi, toàn bộ những object mới sẽ mang giá trị thuộc tính mới. Những cái đã tạo trước đó sẽ giữ nguyên giá trị cũ 