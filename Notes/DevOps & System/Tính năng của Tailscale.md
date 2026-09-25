---
tags:
  - it/networking
  - os/linux
  - it/homelab
aliases:
  - Hướng dẫn Tailscale
  - Mạng VPN Tailscale
---
### 1. 🛡️ Shields up (Bật khiên bảo vệ):
Giống như tàng hình. Vẫn có thể chui vào máy của bạn bè, nhưng KHÔNG AI trong mạng Tailscale được phép chui vào máy của. Bật cái này khi xài mạng công cộng mà sợ bị người khác nhòm ngó.
### 2. 💻 Enable SSH (Bật điều khiển từ xa):
Bình thường muốn điều khiển máy bằng Terminal phải cấu hình SSH lằng nhằng. Bật nút này lên, Tailscale sẽ tự tạo luôn một kênh SSH cực an toàn. Cầm máy khác gõ đúng 1 dòng ssh tên-máy-này là chui thẳng vào Terminal được luôn!
### 3. 🔀 Accept routes (Nhận đường truyền ngoài):
Giả sử cài Tailscale trên máy tính ở công ty, và share luôn cả cái mạng công ty lên đó. Khi máy tính ở nhà bật nút này, sếp có thể kết nối thẳng vào cái máy in hoặc ổ cứng nội bộ ở công ty luôn, y như đang ngồi tại văn phòng!
### 4. 🌐 Advertise exit (Trở thành Cục phát VPN):
Đây là tính năng đáng tiền nhất! Bật nút này sẽ biến cái máy tính hiện tại của sếp thành một trạm VPN (Exit Node).
  - Ví dụ: Ra quán cafe, wifi không an toàn. RMở điện thoại lên, bật mạng Tailscale và chọn kết nối vào cái máy tính ở nhà này. Lúc đó, toàn bộ dữ liệu duyệt web trên điện thoại sẽ được mã hóa, bay thẳng về máy tính ở nhà rồi mới đi ra Internet. Mạng cực kỳ an toàn!
### 5. 🏠 Allow LAN (Cho phép mạng cục bộ):
Khi đang mượn mạng của máy khác (dùng Exit Node), máy mình sẽ bị "mù" với các thiết bị trong nhà. Bật nút này lên để sếp vừa dùng được VPN, vừa có thể điều khiển TV hay bấm in tài liệu ở cái máy in ngay sát bên cạnh.