---
tags:
  - career/path
  - it/devops
  - dev/fullstack
aliases:
  - Lộ trình DevOps
  - Lộ trình Full-stack
---
Project: "**Todo App Pro**" ([[NestJS]] API + React SPA)
- **Backend**: [[NestJS]] + Prisma + PostgreSQL (viết CRUD + JWT Auth).
- **Frontend**: React/TS gọi API.
- **DevOps** (Phần quan trọng):
- Viết **Dockerfile** cho **Backend** và **Dockerfile** cho **Frontend**.
- Viết docker-compose.yml để chạy **App** + **Postgres** + **Caddy** (bạn đã biết Caddy, dùng nó làm Reverse Proxy ngay trong compose).
- Đẩy code lên **GitHub**, viết **GitHub Actions** để tự động build image và push lên registry.
- Dùng **Tailscale** kết nối vào Home Server, ssh vào và kéo image mới về chạy (hoặc dùng **Ansible** để automate bước này).