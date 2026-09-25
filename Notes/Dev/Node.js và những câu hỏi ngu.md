---
tags:
  - dev/nodejs
  - type/qna
aliases:
  - Hỏi đáp Node.js
  - Node.js QnA
---
# 1. Quy trình Setup chuẩn xác nhất sẽ là:
### 1. Cài `fnm` vào hệ thống trước.
### 2. Dùng `fnm` để tải và cài phiên bản Node LTS hiện tại:
- Ví dụ lệnh: `fnm install --lts`
### 3. Set phiên bản LTS vừa tải làm mặc định (global) cho toàn bộ máy tính:
- Ví dụ lệnh: `fnm default <tên-bản-lts>`
### 4. Node.js mặc định trên máy đang được quản lý bởi fnm. 
Bây giờ tiến hành cài `pnpm`. Hiện tại pnpm không còn khuyến khích dùng corepack, nên hãy cài theo 1 trong 2 cách sau:
- **Cách 1 (Khuyên dùng - Cài qua script độc lập):** Giúp cài bản pnpm mới nhất mà không phụ thuộc vào `npm`: `curl -fsSL https://get.pnpm.io/install.sh | sh -`
- **Cách 2 (Cài qua npm - Truyền thống):** Vì fnm đã cấp quyền ghi thư mục độc lập nên hoàn toàn an toàn: `npm install -g pnpm`

**💡 Ghi chú sử dụng hằng ngày:**
Sau này, nếu làm dự án mới, máy sẽ tự động dùng bản LTS mặc định. Nếu gặp dự án cũ cần bản Node khác (vd: v16), chỉ cần gõ `fnm install 16` rồi `fnm use 16`.

# 2. Làm sao để biết dự án cần dùng phiên bản Node.js nào?
Khi đi làm hoặc tải code trên mạng về, kiểm tra theo thứ tự ưu tiên sau:
- Ưu tiên 1: Tìm các file cấu hình phiên bản
  Một dự án chuẩn chỉnh thường sẽ khai báo rõ phiên bản Node.js nó cần. Bạn hãy tìm ở thư mục gốc của dự án:
	- File .nvmrc hoặc .node-version: Trong file này thường chỉ chứa 1 con số (ví dụ: 18.17.0 hoặc 16).
	- Trong file package.json: Hãy tìm mục "engines". Ví dụ:
	    "engines": {
	      "node": ">=18.0.0"
	    }
Nghĩa là phải dùng Node.js từ phiên bản 18 trở lên.

- Ưu tiên 2: Xem các file cấu hình triển khai (CI/CD / Docker)
  Nếu không có các file trên, bạn có thể "soi" vào các file dùng để deploy:
	- File Dockerfile: Tìm dòng FROM node:18-alpine (nghĩa là nó xài Node 18).
	- Thư mục .github/workflows/: Các file YAML test tự động thường có ghi phiên bản Node.js được dùng để test.
