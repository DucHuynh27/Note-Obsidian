---
tags:
  - tool/pm2
  - it/devops
  - dev/nodejs
  - type/tutorial
aliases:
  - Hướng dẫn PM2
  - Chạy ngầm Node.js
---
### 🛠️ Cẩm nang Quản lý Bot bằng PM2 (Mới)

  ## 1. 🔄 Khởi động lại bot (Restart)
  Thường dùng nhất sau khi cập nhật code hoặc đổi cấu hình (file .env):
  pm2 restart zalo-tg

  ## 2. 📊 Kiểm tra trạng thái (Status & Logs)
  Xem bot có đang chạy không và ngốn bao nhiêu RAM:
  pm2 ls (sẽ hiện ra một cái bảng xanh lá rất đẹp)
  (Nếu cột status hiện chữ online (màu xanh) nghĩa là đang chạy ngon lành. Nếu
  hiện stopped hoặc errored (màu đỏ) nghĩa là đang tắt hoặc bị lỗi).
  💡 Tính năng "Ăn tiền" của PM2 (Xem Log trực tiếp):
  Để xem Bot đang làm gì, nhận tin nhắn nào theo thời gian thực (Giống như đang
  mở màn hình terminal):
  pm2 logs zalo-tg (Bấm Ctrl + C để thoát màn hình xem log)

  ## 3. ⏹️ Dừng bot (Stop)

  Tắt bot tạm thời, hệ thống sẽ không tự động bật lại nó:
  pm2 stop zalo-tg

  ## 4. ▶️ Bật lại bot (Start)

  Bật bot lên sau khi đã bị dừng (bị stop):
  pm2 start zalo-tg

  ## (Trường hợp lỡ tay xóa mất cấu hình thì dùng lệnh bật gốc: pm2 start       
  dist/index.js --name "zalo-tg").

  Anh cứ lấy điện thoại mở Termux, chui vào Debian (/data/local/start-debian.sh)
  rồi gõ thử lệnh pm2 ls xem cái bảng xanh lá của nó hiện ra nhìn có "công nghệ
  cao" hơn cái của runit không nhé! 😂
