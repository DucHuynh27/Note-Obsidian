---
tags:
  - it/ai
  - tool/agy
  - type/cheatsheet
aliases:
  - AGY Commands
  - Lệnh Antigravity
---
## 1. Nhóm Lệnh Hoạch định & Học hỏi
- /learn (Học hỏi & Ghi nhớ): Khi vừa sửa xong một lỗi khó hoặc có một thói quen mới (như cách commit code, cách dùng nh), gõ /learn [nộidung] để AGY tự động tạo file Rule và ghi nhớ mãi mãi.
- /plan (Lên kế hoạch cẩn thận): Thay vì lao vào code ngay và làm hỏng hệ thống, dùng /plan (VD: /plan hãy thiết lập lại toàn bộ cấu hình Neovim). AGY sẽ dừng lại, phân tích, tạo ra một bản kế hoạch từng bước (Artifact) và đợi duyệt trước khi chạy bất kỳ dòng code nào.
- /grill-me (Phỏng vấn truy vấn): Nếu có ý tưởng mờ nhạt (VD: "Tôi muốn đổi Window Manager"). AGY sẽ đóng vai người phỏng vấn, liên tục hỏi (chọn Sway hay Hyprland? Dùng Waybar hay Polybar?) để làm rõ yêu cầu trước khi bắt tay vào làm.
## 2. Nhóm Lệnh Thực thi & Tự động hóa
- /goal (Nhiệm vụ dài hơi/Cày cuốc): Dùng cho các tác vụ tốn thời gian. Ví dụ: /goal Hãy rà soát toàn bộ file trong ~/nix-config, tìm các gói bị lỗi thời và cập nhật chúng. AGY sẽ kiên nhẫn làm việc liên tục (có thể qua đêm) và không dừng lại cho đến khi đạt được mục tiêu 100%.
- /schedule (Lên lịch trình/Hẹn giờ): Giúp giao việc tự động chạy ngầm.
	- Đặt lịch định kỳ: /schedule "0 0 * * *" chạy lệnh [[Quản lý phiên bản nnlt bằng flake|nix flake]] update và dọn rác hệ thống mỗi ngày.
	- Hẹn giờ một lần: /schedule "nhắc tôi check lại log build sau 10 phút nữa".
- /browser (Tự động lướt web): Cấp quyền cho AGY mở trình duyệt ẩn để tự tìm kiếm giải pháp. VD: /browser Lên Github tìm xem cấu hình mặc định mới nhất của công cụ Yazi là gì và báo cáo lại.
## 3. Nhóm Lệnh Đội nhóm
- /teamwork-preview (Triệu hồi đội đặc nhiệm): Dành cho các dự án lớn. Giao một việc phức tạp, AGY sẽ tự động đẻ ra các "Đặc vụ phụ" (Subagents). Một con đi research tài liệu, một con viết code, một con chạy test độc lập với nhau để đẩy nhanh tiến độ.