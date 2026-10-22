Khi bạn muốn tải phiên bản mới của các phần mềm, kernel, extensions (tương đương apt update && apt upgrade):

  • Bước 1: Cập nhật file flake.lock (Kéo danh sách package mới nhất từ github/nixpkgs):
    cd ~/nix-config
    nix flake update
  (Hoặc nếu chỉ muốn update riêng 1 input cụ thể, ví dụ: nix flake update nixpkgs)
  • Bước 2: Rebuild lại hệ thống để tải và cài đặt các phiên bản mới:
    sudo nixos-rebuild switch --flake ~/nix-config#nixos

  ──────
  #### 3. Dọn dẹp các bản build cũ để giải phóng dung lượng ổ cứng (Garbage Collection)

  Sau khi update nhiều lần, các phiên bản cũ sẽ vẫn được lưu lại để dự phòng rollback. Khi cần dọn ổ cứng:

    # Dọn các package không còn dùng và dọn lịch sử cũ quá 7 ngày
    nix-collect-garbage --delete-older-than 7d
    sudo nix-collect-garbage --delete-older-than 7d

    # Tối ưu hoá store (gộp các file trùng lặp bằng hardlink)
    nix store optimise