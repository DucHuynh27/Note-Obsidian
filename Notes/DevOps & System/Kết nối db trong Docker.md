---
tags:
  - it/docker
  - it/database
  - it/devops
aliases:
  - Docker Database
  - Setup DB Docker
---
## 1. Nếu dùng DataGrip:
- Mở DataGrip lên -> Bấm dấu + chọn Microsoft SQL Server.
- Host: localhost
- Port: 1433
- User: sa
- Password: SuperStrong!123
## 2. Nếu dùng SSMS (Ở trong Máy ảo Win 10):
- Mở SSMS lên.
- Server name: 10.0.2.2,1433 (Lưu ý: Chữ 10.0.2.2 là cánh cửa để máy ảo nhìn ra cái máy thật).
- Authentication: Chọn SQL Server Authentication.
- Login: sa
- Password: SuperStrong!123
## 3. Khi nào nên dùng DataGrip? (Nên dùng cho 90% công việc hằng ngày)
Ưu tiên dùng DataGrip ngay trên máy Linux thật của mình cho các công việc phát triển (Development) thông thường.
- Viết truy vấn (Query) hằng ngày: Lấy dữ liệu (SELECT), thêm, sửa, xóa (INSERT, UPDATE, DELETE).
- Viết code SQL: DataGrip là một IDE xịn của JetBrains, nên tính năng gợi ý code (auto-complete), tô màu cú pháp, định dạng code (format), và cảnh
  báo lỗi của nó vượt trội hơn SSMS rất nhiều.
- Thao tác cơ bản với bảng: Tạo bảng mới, thêm cột, sửa kiểu dữ liệu, tạo View...
- Làm việc với nhiều loại CSDL: Nếu dự án của bạn dùng cả SQL Server, PostgreSQL và Redis, DataGrip có thể kết nối quản lý tất cả trong cùng một
  cửa sổ.
- Lợi ích lớn nhất: Chạy trực tiếp trên Linux (máy thật), rất nhẹ, mượt và không cần phải bật máy ảo Windows lên.
## 4. Khi nào BẮT BUỘC (hoặc nên) vào máy ảo để dùng SSMS?
SSMS là "con đẻ" của Microsoft, được thiết kế riêng cho SQL Server. Do đó, bạn chỉ nên bật máy ảo và dùng SSMS khi cần làm các tác vụ Quản trị hệ thống (DBA) hoặc dùng các tính năng đặc thù sâu bên trong SQL Server:
- Cấu hình Server: Chỉnh sửa bộ nhớ (RAM) tối đa cho SQL Server, cấu hình bảo mật, phân quyền chi tiết cấp độ máy chủ.
- SQL Server Agent: Nếu bạn cần tạo các tác vụ chạy tự động theo lịch (Jobs), ví dụ: "Cứ 12h đêm tự động chạy script này", giao diện của SSMS là số 1.
- Backup & Restore qua giao diện: Mặc dù DataGrip có thể chạy lệnh T-SQL để restore, nhưng giao diện chọn file backup, chọn điểm restore của SSMS trực quan và an toàn hơn rất nhiều.
- Phân tích hiệu năng (Query Execution Plan): Khi một câu query chạy quá chậm và bạn cần xem biểu đồ phân tích chi tiết xem nó chậm ở khâu nào (quét bảng hay thiếu index), công cụ của SSMS hiển thị sơ đồ này rất chi tiết và chuẩn xác.
- Quản lý các dịch vụ đi kèm: Ví dụ như SQL Server Replication (đồng bộ dữ liệu), SSIS (tích hợp dữ liệu), SSAS (phân tích dữ liệu).
- Nhập/Xuất lượng dữ liệu lớn (Import/Export Data): SSMS có công cụ Import/Export Wizard hỗ trợ đọc dữ liệu từ Excel, CSV nạp thẳng vào database với giao diện rất dễ click.

> [!NOTE]
> Tóm lại:
> - Là một Lập trình viên (Developer): Hãy dùng DataGrip trên Linux. Nhanh, tiện, code sướng tay.
> - Là một Người quản trị CSDL (DBA): Khi cần "bảo trì, sửa chữa, cài đặt chuyên sâu" thì hãy bật máy ảo lên và vào SSMS.