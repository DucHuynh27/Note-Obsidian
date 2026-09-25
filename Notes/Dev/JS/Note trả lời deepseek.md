- typeof null trả về "string", bug js từ 1995
- số thực trong js dùng IEEE 754 double nên số thực không thể biểu diễn chính xác được
- `[] + []` -> "": mảng ép về string
  `[] + {}` -> `[object Object]`
  Quy tắc: + khi có non-primitive (không nguyên thuỷ) sẽ ép về primitive trước
- `NaN === NaN` là false. cách kiểm tra 1 số có phải NaN không là `Number.isNaN(x)` hoặc `x!=x` vì NaN khác chính nó, === với NaN luôn sai
- typrof NaN là "number" vì kiểu nó phải vậy, NaN là 1 giá trị số không hợp lệ, không phải kiểu riêng
- `Boolean("")` → `false` (chuỗi rỗng falsy) 
	`Boolean("0")` → `true` (chuỗi không rỗng, dù nội dung là "0")
	`Boolean([])` → `true` (mảng, kể cả rỗng, là truthy)
	`Boolean({})` → `true` (object luôn truthy)  
    **Falsy chỉ có:** `false, 0, -0, 0n, "", null, undefined, NaN`.
- `null == underfined` -> true vì == có quy tắc đặc biệt coi chúng bằng nhau
	`null === underfined` -> false vì khác kiểu (null là string còn underfined là )
- khi console.log 1 vị trí không có trong mảng sẽ in ra underfined thay vì lỗi. nhưng nếu khai báo giá trị ở vị trí nằm ngoài mảng thì mảng sẽ tự mở rộng theo index cao nhất +1
- Dấu `+` đứng trước chuỗi là **unary plus** — ép chuỗi thành số. Nên đây là phép cộng số, không phải nối chuỗi. Ví dụ: `+"3" + +"4"` → `3 + 4 = 7`
- `var`: function scope, hoisted, khởi tạo `undefined`. 
	`let`/`const`: block scope, hoisted nhưng nằm trong **TDZ (Temporal Dead Zone)** — truy cập trước khi khai báo sẽ lỗi
	`const`: không được gán lại, nhưng **object bên trong vẫn sửa được**.
- trong vòng lặp:
	- `var` chỉ có 1 biến `i` duy nhất trong function scope, khi callback chạy thì `i` đã bằng 3.
	- `let` tạo binding mới cho mỗi lần lặp nên mỗi callback giữ 1 gái trị riêng
- Closure = hàm "nhớ" scope nơi nó sinh ra, kể cả sau khi scope đó kết thúc. Ví dụ:
```
function counter() {
  let n = 0;
  return () => ++n;
}
const c = counter();
c(); // 1
c(); // 2
```
- Trong **method**: `this` = object gọi method (`obj.method()` → `this = obj`).
	Trong **arrow function**: `this` **lấy từ scope bao quanh**, không có `this` riêng. Đây là khác biệt cốt lõi.
- Non-strict: `this = globalThis` (window trong browser, global trong Node). Strict (`"use strict"`): `this = undefined` (16)