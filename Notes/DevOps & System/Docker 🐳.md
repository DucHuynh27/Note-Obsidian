---
tags:
  - it/docker
  - it/devops
  - type/tutorial
aliases:
  - Học Docker
  - Cơ bản Docker
---
## 1. Hai Khái Niệm Quan Trọng Nhất

Để dễ hình dung, hãy tưởng tượng Docker giống như việc đi mua một chiếc đĩa game:
*   **Image (Cái đĩa game):** Là một file nén đóng gói sẵn mọi thứ (Hệ điều hành, mã nguồn, thư viện,...). Image mang tính chất **chỉ đọc (read-only)**. Ví dụ: Image của MS SQL Server, Image của Ubuntu, Image của NodeJS...
*   **Container (Máy chơi game):** Bỏ cái đĩa (Image) vào máy ảo để nó chạy lên, nó biến thành một **Container**. Một Image (1 cái đĩa) có thể tạo ra được hàng trăm cái Container (hàng trăm cái máy đang chơi game đó) chạy độc lập với nhau.
## 2. Các Lệnh Vòng Đời Cơ Bản
### Khởi tạo và chạy (Lệnh dùng nhiều nhất)
```bash
# Lệnh tổng quát
docker run [tùy_chọn] [tên_image]

# Ví dụ: Chạy một con web server Nginx
docker run -d -p 8080:80 --name web_cua_tui nginx
```
> [!TIP]
> **Giải ngố các Tùy chọn (Flags):**
> *   `-d` (Detached): Báo Docker hãy đem cái thùng này chạy ngầm đi, trả lại con trỏ terminal cho tui gõ lệnh khác. (Rất quan trọng!)
> *   `--name`: Đặt một cái tên gợi nhớ cho Container, nếu không Docker sẽ tự đặt một cái tên ngẫu nhiên rất buồn cười (ví dụ: *crazy_einstein*).
> *   `-p [Port_ngoài]:[Port_trong]`: Đục một lỗ thủng trên thùng container để giao tiếp với bên ngoài. Ví dụ `-p 8080:80` nghĩa là: Lấy cổng `8080` của máy tính sếp, cắm vào cổng `80` của cái container.
### Quản lý các Container đang có
```bash
# Xem các Container ĐANG CHẠY
docker ps

# Xem TẤT CẢ Container (kể cả những cái đã bị tắt)
docker ps -a
```
### Dừng và Bật lại
Khi không dùng nữa, tắt đi để đỡ tốn RAM.
```bash
# Tắt 1 container
docker stop web_cua_tui

# Mở lại 1 container đã tắt
docker start web_cua_tui
```
### Xóa bỏ (Dọn rác)
```bash
# 1. Để xóa một Container (Phải tắt nó đi trước khi xóa)
docker rm web_cua_tui

# Lệnh tắt ép buộc và xóa luôn (cực mạnh)
docker rm -f web_cua_tui

# 2. Xóa đĩa cài đặt (Image) cho đỡ tốn ổ cứng
docker rmi nginx
```
## 3. Khái Niệm Nâng Cao Bắt Buộc Phải Biết: #VOLUME

> [!WARNING]
> **Vấn đề nguy hiểm:** Bản chất của Container là "tạm thời". Nếu chạy MS SQL Server bằng Docker, sếp tạo bảng, thêm dữ liệu chán chê. Xong sếp lỡ tay gõ lệnh `docker rm` xóa cái Container đó đi... **BÙM! Mất sạch toàn bộ database vĩnh viễn!**

Để giải quyết, chúng ta dùng Volume (như kiểu cắm cái USB hoặc ổ cứng ngoài vào trong thùng container). Dữ liệu sẽ được ghi thẳng ra ổ cứng thật của máy. Có 2 cách dùng phổ biến:
1. Named Volume (Do Docker quản lý - Khuyên dùng cho Database): Nên dùng cách này vì nó tránh được 99% các lỗi về quyền (permission). Docker sẽ tự tạo một thư mục an toàn ẩn dưới hệ thống (thường ở /var/lib/docker/volumes/) để chứa dữ liệu.
**Cú pháp:**
```
-v ten_volume:/thư_mục_trong_container
```
2. Bind Mount (Do mình tự chỉ định thư mục - Khuyên dùng cho chứa Code): Map thẳng một thư mục trên máy mình vào container. Sửa code trên máy thật là code trong container cập nhật theo.
**Cú pháp:**
```
-v /home/hinne/thu_muc_cua_toi:/thư_mục_trong_container
```
Bây giờ dù có xóa tung cái Container đi, thì dữ liệu Database vẫn còn nguyên vẹn nằm trên máy. Lần sau chạy container mới chỉ cần map lại đúng cái thư mục đó là Database cũ hiện hồn về!
## 4. Trải Nghiệm Thử Ngay Bây Giờ!
```bash
docker run -d -p 8080:80 --name thu_nghiem nginx
```
Mở trình duyệt (Thorium/Zen) và gõ vào thanh địa chỉ: `http://localhost:8080`

Thấy dòng chữ **"Welcome to nginx!"** là thành công triển khai một máy chủ web thực thụ bằng Docker.

Khi chán rồi, dọn dẹp bằng lệnh: `docker rm -f thu_nghiem`.
## 5. [[Kết nối db trong Docker]] để dùng với DataGrip/SMSS
Bật MS SQL Server bằng Docker:
- Tạo và chạy SQL Server ngầm (Câu lệnh chuẩn mực nhất, không lo mất dữ liệu):
```
docker run -d \
--name mssql \
--restart unless-stopped \
-e 'ACCEPT_EULA=Y' \
-e 'MSSQL_SA_PASSWORD=SuperStrong!123' \
-p 1433:1433 \
-v mssql_data:/var/opt/mssql \
mcr.microsoft.com/mssql/server:2022-latest
```
>--restart unless-stopped: Lệnh bài miễn tử! Mỗi lần bạn bật máy tính lên, Docker sẽ tự động bật luôn SQL Server này cho bạn. Khỏi mất công gõ lệnh mở lại thủ công.
>-e: Truyền biến môi trường. Bắt buộc phải đồng ý điều khoản (ACCEPT_EULA=Y) và đặt mật khẩu (MSSQL_SA_PASSWORD).
>-v mssql_data:/var/opt/mssql: Sinh ra cái "USB" tên là mssql_data để cất giữ an toàn toàn bộ database ra ổ cứng thật. Lần sau lỡ tay xóa container, chạy lại đúng câu lệnh này là data cũ tự động "hiện hồn" về!

> [!NOTE]
> Mật khẩu SQL Server bắt buộc phải cực mạnh: gồm chữ HOA, chữ thường, số và ký tự đặc biệt (VD: SuperStrong!123).
> 
  • Phần mềm chạy trên Linux thật ➡️ Nối vào CSDL bằng localhost.
  • Phần mềm chạy trong Máy ảo Windows ➡️ Nối ra CSDL bên ngoài bằng 10.0.2.2.

## 6. Hiểu sâu hơn về #VOLUME
#### 1. Vấn đề "Mất đồ khi trả phòng" (Không dùng Volume)
  Tưởng tượng một cái Container là một căn phòng khách sạn bạn vừa thuê.
  Bên trong phòng (container) có sẵn giường, tủ, máy lạnh (chính là hệ điều hành Linux và SQL Server).
- Khi bạn tạo Table, thêm dữ liệu, nó giống như việc bạn mua đồ đạc về và cất vào cái tủ trong phòng khách sạn đó.
- Bạn có thể ra ngoài, khóa cửa lại (Stop container). Khi quay lại mở cửa (Start container), đồ đạc vẫn ở đó.
- TUY NHIÊN: Nếu bạn trả phòng (chạy lệnh docker rm - xóa container), nhân viên dọn dẹp sẽ ném tất cả đồ đạc của bạn vào thùng rác để đón khách mới. Bạn mất trắng dữ liệu.
#### 2. Cách Volume giải cứu (Cánh cửa thần kỳ)
  Để không bị mất đồ, bạn thuê một Két sắt nằm cố định ở nhà bạn (chính là ổ cứng máy tính Linux của bạn). Sau đó, bạn dùng một "Cánh cửa thần kỳ" (Cú pháp -v) kết nối cái tủ trong khách sạn với cái Két sắt ở nhà.
- Lúc này, mỗi khi SQL Server (bên trong container) lưu dữ liệu vào tủ, dữ liệu thực chất sẽ rơi tuột qua cánh cửa thần kỳ và nằm gọn trong Két sắt ở nhà bạn.
- Khi bạn trả phòng khách sạn (Xóa container), cái tủ bị đập đi, nhưng Két sắt ở nhà bạn vẫn còn y nguyên.
- Lần sau, bạn thuê phòng khách sạn mới, chỉ cần mang "Cánh cửa thần kỳ" nối vào đúng Két sắt cũ, là đồ đạc lại hiện ra!
#### 3. Phân biệt 2 loại Két sắt (2 cách dùng -v)
Docker cung cấp 2 cách để tạo Két sắt:
**Cách 1:** Bind Mount (Bạn tự chỉ định chỗ để Két sắt)
- Cú pháp: -v /home/hinne/data_cua_tui:/var/opt/mssql
- Nghĩa là: Bạn bảo Docker: "Hãy đục lỗ cái thư mục /var/opt/mssql trong container, và nối thẳng nó ra cái thư mục /home/hinne/data_cua_tui nằm rành rành trên máy tính Linux của tôi".

> [!NOTE]
> - **Ưu điểm**: Bạn có thể mở ứng dụng quản lý file (File Explorer) trên Linux, click vào /home/hinne/data_cua_tui là thấy ngay các file bên trong. Rất hợp để chứa Source Code (vì bạn muốn mở VS Code lên sửa trực tiếp).
> - Nhược điểm (Rất khó chịu): Lỗi phân quyền (Permission Denied). Máy Linux của bạn là user hinne, nhưng SQL Server trong container lại chạy bằng user mssql. Thằng mssql cố gắng ghi file ra cái thư mục thuộc về thằng hinne, thế là hệ thống Linux chặn lại báo lỗi "Mày không có quyền!".

**Cách 2:** Named Volume (Giao cho Docker giữ Két sắt) - ĐÂY LÀ CÁCH BẠN ĐANG DÙNG
- Cú pháp: -v mssql_data:/var/opt/mssql
- Nghĩa là: Bạn bảo Docker: "Tao muốn một cái két sắt tên là mssql_data. Mày tự tìm chỗ cất nó trên ổ cứng cho tao, và mày tự lo mấy vụ cấp quyền đi, đừng để lỗi".
	- Bên dưới nền tảng: Docker sẽ ngoan ngoãn tạo một thư mục giấu kỹ trong hệ thống Linux của bạn (nằm ở /var/lib/docker/volumes/mssql_data/_data), và nó tự set quyền
  cho thằng user mssql tha hồ ghi/đọc mà không bị lỗi.

> [!NOTE]
> - Ưu điểm: Cực kỳ ổn định, KHÔNG BAO GIỜ bị lỗi permission. Được khuyên dùng 100% cho Database (SQL Server, MySQL, Postgres...).
> - Nhược điểm: Bạn khó mà dùng chuột click click mở cái thư mục đó ra xem trên Linux được (vì nó bị giấu sâu và đòi quyền Root/Sudo). Nhưng cũng chẳng sao, vì file dữ liệu Database là file nhị phân, bạn mở ra cũng đâu đọc được, đằng nào cũng phải dùng DataGrip để xem mà!