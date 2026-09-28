## Màn hình:
1. Màn hình Setup / Lựa chọn Role:
   - Dropdown chọn ngành nghề (Frontend, Backend, PM, Sale...)
   - Chọn ngôn ngữ (Tiếng Anh, Tiếng Việt)
   - Chọn loại (Technical, Behavioral)
   - Nút "Bắt đầu phỏng vấn"
2. Màn hình Live Interview (Cốt lõi):
   - Giao diện chính: Giao diện tối màu (đỡ mỏi mắt), ở giữa là cục "sóng âm" (Visualizer) đập nhịp nhàng, biểu thị AI đang nói hoặc đang nghe.
   - Transcript (Phụ đề): Text chữ to ở nửa dưới màn hình hiển thị lời AI (và lời bạn) để lỡ nghe không kịp thì đọc.
   - Control Bar: Micro (Mute/Unmute), Camera (Bật/tắt video nếu có), Mở code editor (nếu phỏng vấn tech), End Call (Nút đỏ to).
3. Màn hình Results / Feedback:
   - Sau khi kết thúc, hiển thị điểm số, đánh giá chi tiết (Điểm mạnh, Điểm yếu) và gợi ý cải thiện.
## Cần học:
1. 🔷 Frontend (React)

Để vẽ giao diện và xử lý tương tác người dùng:

- HTML: Gần như không cần học sâu, chỉ cần hiểu các thẻ semantic cơ bản `(<div>, <span>, <button>, <input>)` và đặc biệt là thẻ `<audio>` (nếu play âm thanh) hoặc `<canvas>` (nếu bạn muốn vẽ sóng âm phức tạp).
- CSS:
  - Flexbox: Cực kỳ quan trọng (dùng để căn giữa màn hình, dàn hàng ngang dọc các nút bấm).
  - CSS Animations (@keyframes): Cần thiết để làm cái animation "vòng tròn nhấp nháy" khi AI đang nói.
  - (Grid cũng tốt nhưng Flexbox dùng 90% thời gian).
- Javascript / TypeScript (Nền tảng quan trọng nhất):
  - Async/Await & Promises: Bắt buộc (để gọi API, chờ AI trả lời).
  - Destructuring & Spread Operator: const { data } = response hay [...list].
  - Array methods: .map(), .filter() (để render danh sách câu hỏi/log chat).
- React Core:
  - JSX: Cách viết HTML trộn lẫn Javascript.
  - useState: Lưu trạng thái (app đang ghi âm hay đang dừng? ai đang nói?).
  - useEffect: Chạy logic khi mở app (như xin quyền dùng Microphone).
  - useRef: Rất quan trọng để quản lý phần tử Microphone/Audio thực tế trong DOM.

2. 🔶 Backend (NestJS)

Nơi xử lý logic nghiệp vụ và kết nối Database/AI:

- TypeScript / OOP: NestJS thuần tuý hướng đối tượng. Cần hiểu về Class và Interface.
- NestJS Core Concepts:
  - Decorators: Cách dùng @Controller(), @Get(), @Post().
  - Dependency Injection: Khái niệm Tách biệt logic xử lý ra các Service và tiêm vào Controller.
- API & Protocol:
  - REST API: Viết các endpoint để đăng nhập, lưu lịch sử.
  - WebSockets (Nâng cao nhưng cần thiết): Để có độ trễ thấp khi nói chuyện live với AI, WebSockets là công cụ bắt buộc.

3. 🗄️ Database (MySQL)

Lưu trữ thông tin người dùng và kết quả đánh giá:

- SQL cơ bản: SELECT, INSERT, UPDATE, JOIN (để lấy danh sách bài phỏng vấn của một User).
- Thiết kế Database (Schema): Khái niệm Bảng (Table), Khóa chính (Primary Key), Khóa ngoại (Foreign Key - ví dụ 1 User có nhiều Interviews).
- Gợi ý: Trong thực tế với NestJS, bạn nên học thêm Prisma ORM (một công cụ giúp bạn không phải tự gõ lệnh SQL mà dùng code TypeScript để thao tác dữ liệu, an toàn và nhàn hơn rất nhiều).

4. 🤖 Kỹ năng phụ (Domain-specific cho app AI Voice)

- Web Audio API / MediaRecorder: API của trình duyệt để bắt âm thanh từ Mic người dùng.