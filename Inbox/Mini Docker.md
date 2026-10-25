2. Các tab
Containers
- Hiện cả container đang chạy lẫn đã dừng.
- Chọn container để xem image, trạng thái, port, thời gian tạo.
- Nút: Start / Stop / Restart / Remove.
- Remove xoá ngay, không hỏi lại — cẩn thận.
Images
- Hiện toàn bộ image, đã có tên đầy đủ.
- Chọn image → Run để tạo container mới.
- Chọn image → Remove để xoá image. Image đang được container dùng sẽ không xoá được.
Volumes
- Xem và xoá volume.
- Volume đang được container dùng sẽ không xoá được.
Networks
- Xem và xoá network.
- Network mặc định như podman, bridge, host, none sẽ bị chặn xoá.
3. Chạy một image
Ví dụ chạy PostgreSQL:
1. Vào tab Images
2. Chọn docker.io/library/postgres:17
3. Bấm Run
4. Điền:
Container name: postgres-test
Network:        podman
Publish port:   bật
Port:           5432
Environment:
POSTGRES_PASSWORD=secret
5. Bấm Run
Plugin sẽ chạy lệnh tương đương:
docker run -d \
  --name postgres-test \
  --network podman \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=secret \
  docker.io/library/postgres:17
Trên máy bạn, docker thực chất là Podman-compatible CLI, nên container sẽ nằm trong Podman. Kiểm tra bằng:
podman ps -a
podman logs postgres-test
4. Một số giới hạn cần biết
- Form Run chỉ map cùng một port: -p 5432:5432. Không nhập kiểu 8080:80 được.
- Không hỗ trợ mount volume, entrypoint, command, restart policy trong UI.
- Không xem log trong panel; dùng podman logs <tên>.
- Muốn chạy nâng cao thì dùng CLI:
podman run -d --name myapp \
  -p 8080:80 \
  -v mydata:/data \
  --restart unless-stopped \
  nginx:latest
- Nếu plugin mini-docker được update, bản patch hiển thị tên image có thể bị ghi đè; khi đó Images lại có thể hiện <none>:<none>.