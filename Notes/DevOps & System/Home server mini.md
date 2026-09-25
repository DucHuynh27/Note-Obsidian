---
tags:
  - it/homelab
  - os/linux
  - it/devops
aliases:
  - Máy chủ cá nhân
  - Setup Home Server
---
## 1. runit - Quản lý tiến trình
- Dùng để làm gì: Quản lý các phần mềm chạy ngầm.
- Tại sao lại cần: Trong Linux thông thường người ta dùng systemd, nhưng trên Android thì systemd rất dễ bị lỗi. runit là giải pháp thay thế cực kỳ nhẹ. Giả sử con bot Zalo-TG hoặc Caddy bị lỗi và văng ra ngoài (crash), runit sẽ ngay lập tức phát hiện và tự động khởi động lại. Nó đảm bảo server sống 24/7.
## 2. Tailscale - Mạng riêng ảo / VPN
- Dùng để làm gì: Cấp cho thiết bị một địa chỉ IP tĩnh và bảo mật.
- Tại sao lại cần: Thiết bị cắm ở nhà, dùng Wi-Fi hoặc 4G, IP sẽ thay đổi liên tục. Tailscale sẽ nhốt các thiết bị vào chung một "mạng LAN ảo". Dù  mang laptop ra quán cafe hay đi du lịch, vẫn có thể gõ ssh root@<IP_Tailscale_của_thiết_bị> để điều khiển nó như thể hai máy đang cắm chung một sợi dây mạng.
## 3. Cloudflare Tunnel - Đưa web ra thế giới
- Dùng để làm gì: Giúp người khác truy cập vào server qua Internet mà không cần mở Port (NAT) trên cục modem Wi-Fi.
- Tại sao lại cần: Các nhà mạng thường chặn không cho người ngoài truy cập ngược vào mạng nhà. Cloudflared sẽ chủ động đào một đường hầm từ server tới máy chủ của Cloudflare. Nhờ đó, có thể gán một tên miền xịn (ví dụ: zalo.ten-cua-ban.com) trỏ thẳng vào con bot trên server để nhận Webhook hoặc làm web, với độ bảo mật tuyệt đối.
## 4. Caddy - Web Server / Reverse Proxy
- Dùng để làm gì: Điều hướng luồng truy cập mạng và tự động gắn ổ khóa xanh HTTPS.
- Tại sao lại cần: Giả sử trên server chạy 3 ứng dụng: Bot Zalo (cổng 3000), Trang quản lý (cổng 8080), và một API cá nhân (cổng 5000). Caddy sẽ đứng ở cổng chính (cổng 80/443). Khi có người truy cập bot.ten-ban.com, Caddy sẽ dẫn họ vào cổng 3000. Nếu truy cập api.ten-ban.com, Caddy sẽ dẫn vào cổng 5000. Caddy cực kỳ nhẹ và cấu hình siêu dễ so với Nginx.
## 5. Ansible - Tự động hóa IaC
- Dùng để làm gì: Viết ra giấy cách cài đặt server, rồi máy tính tự làm theo.
- Tại sao lại cần: Với Ansible, chỉ cần viết 1 file YAML liệt kê các bước ("Cài nodejs", "Copy code bot", "Chạy runit"). Sau này nếu mua 10 cái server khác, chỉ cần bấm 1 nút chạy Ansible từ laptop, nó sẽ tự động cấu hình 10 cái y hệt nhau trong 5 phút.
---
## Tóm tắt luồng hoạt động nếu làm dự án Web/Bot lớn:
Khách hàng truy cập **ten-mien.com** ➔ **Cloudflare Tunnel** ➔ **Trỏ về điện thoại của bạn** ➔ **Caddy nhận tín hiệu** ➔ **Chia vào Bot Zalo-TG** ➔ Nếu Bot chết, **runit** lập tức gọi sống lại.
Còn mình ngồi ở công ty, cứ bật Tailscale lên là chui thẳng vào server sửa code!