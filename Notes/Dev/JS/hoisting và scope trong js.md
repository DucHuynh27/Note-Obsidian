# Cần hiểu
## *Hoisting là di chuyển phần khai báo lên đầu phạm vi (scope) trước khi thực thi*
- `var` vừa khai báo vừa khởi tạo giá trị, hoisting nhận giá trị `undefined`
- `let` được hoisted nhưng k được khởi tạo giá trị nào
### 1. Cơ chế hoạt động của biến:
 - Vòng đời: khai báo -> khởi tạo -> gán giá trị
 - Xử lí qua 2 giai đoạn: khởi tạo -> thực thi
### 2. So sánh
- `var` khi hoisting vừa khai báo vừa khởi tạo nên ban đầu là `undefined`
- `let` và const chỉ được khởi tạo giá trị khi chạy đến đúng dòng code

## *Scope*
- Scope quyết định khả năng truy cập của biến
1. Global scope: truy cập ở đâu cũng được
2. Function scope: truy cập trong hàm
3. Block scope: let và const chỉ truy cập được trong khối lệnh { } (ví dụ `if`, `for`, `while`)
4. Lexical scope: định nghĩa global thì khi được gọi sẽ lấy giá trị global chứ k lấy giá trị nơi hàm được gọi