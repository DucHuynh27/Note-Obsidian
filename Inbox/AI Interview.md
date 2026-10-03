# Kế Hoạch Triển Khai Hoàn Chỉnh: Nền Tảng AI-Interview (3 Tháng)

## Goal Description
Xây dựng web app **AI-Interview** - nền tảng phỏng vấn giả lập thông minh hỗ trợ **Song ngữ (Tiếng Việt & Tiếng Anh)** dành cho sinh viên mới ra trường và người chuyển ngành.
Ứng viên upload **CV (PDF)** và nhập **Job Description (JD)**; hệ thống tự động **che thông tin cá nhân (PII Masking)** để bảo mật dữ liệu, phân tích so khớp CV - JD, cho phép chọn **phong cách người phỏng vấn (Interviewer Persona)**, và tiến hành buổi phỏng vấn giọng nói tương tác theo phương pháp **STAR Framework**.

Dự án được xây dựng với mục tiêu tham gia **cuộc thi khởi nghiệp** trong vòng **3 tháng**, tối ưu chi phí (~200 VNĐ / buổi), áp dụng phương pháp **Smart Vibe Coding** với khả năng làm việc linh hoạt trên **mọi AI Agent** (Antigravity, Cursor, Claude Code, GitHub Copilot, Windsurf...) thông qua hệ thống Context Markdown chuẩn hóa.

---

## User Review Required

> [!IMPORTANT]
> **Hệ thống Context Files dành cho AI Agents (Agent-Ready Repository):**
> Để đảm bảo sau này bạn có thể chuyển đổi mượt mà giữa bất kỳ AI agent nào (Antigravity, Claude Code, Cursor, Copilot...) mà không sợ AI bị "mất trí nhớ" hay tự ý đổi tech stack, chúng ta sẽ tạo ngay **Bộ 3 File Markdown cốt lõi** ngay từ đầu dự án:
> 1. `AGENT.md` (hoặc `CLAUDE.md` / `.cursorrules`): Quy tắc ứng xử, lệnh dev/build, quy chuẩn code, những điều cấm kỵ (do's & don'ts) cho AI.
> 2. `docs/ARCHITECTURE.md`: Thiết kế luồng dữ liệu, cấu trúc thư mục, định nghĩa schema DB, prompt templates và mô hình STAR.
> 3. `docs/TASK_LIST.md`: Toàn bộ checklist 5 Sprint được chia nhỏ. Mỗi khi mở tool mới, bạn chỉ cần gõ: *"Đọc AGENT.md và TASK_LIST.md, hãy làm tiếp Task X"*.

---

## Technical Specifications & Stack

| Hạng mục | Công nghệ lựa chọn | Vai trò trong dự án |
| :--- | :--- | :--- |
| **Framework** | Next.js 15 (App Router) | Fullstack Monolith (Frontend + Server Actions / Route Handlers) |
| **Language** | TypeScript | Kiểm soát type chặt chẽ, giảm thiểu lỗi runtime |
| **UI Library** | Tailwind CSS + shadcn/ui + Lucide Icons | Dựng giao diện chuyên nghiệp, responsive và hiện đại |
| **Database** | PostgreSQL Serverless (Neon hoặc Supabase) | Miễn phí, scale tự động, backup an toàn trên cloud |
| **ORM** | Prisma ORM | Schema định nghĩa rõ ràng, type-safe query |
| **Authentication** | Clerk Auth hoặc NextAuth.js | Đăng nhập tài khoản Google 1 chạm |
| **AI Brain** | Google Gemini 2.0 Flash / 1.5 Flash | Đọc trực tiếp PDF CV, so khớp JD, sinh câu hỏi & chấm điểm STAR |
| **Voice Engine** | Web Speech API (Client STT) + Edge-TTS | Nhận diện & phát âm tiếng Việt/Anh chuẩn tự nhiên, 0 đồng |
| **Hosting** | Vercel | Deploy tự động sau mỗi commit GitHub |
| **Agent Context** | `AGENT.md`, `ARCHITECTURE.md`, `TASK_LIST.md` | Bộ tài liệu điều hướng mọi AI agent |

---

## Cấu Trúc Agent Context Files Đề Xuất

```text
AI-Interview/
├── AGENT.md                # Cẩm nang chỉ dẫn cho TẤT CẢ các AI agents
├── docs/
│   ├── ARCHITECTURE.md     # Bản vẽ kỹ thuật & luồng dữ liệu chi tiết
│   └── TASK_LIST.md        # Danh sách công việc theo Sprint để đánh dấu tiến độ
├── src/                    # Source code Next.js
...
```

### Nội dung của `AGENT.md` gồm những gì?
- **Project Overview:** Định nghĩa bài toán, đối tượng người dùng (sinh viên/chuyển ngành).
- **Tech Stack Guardrails:** Cấm AI tự ý cài thêm framework khác hoặc chuyển sang JS thuần.
- **Commands:** Các lệnh chuẩn (`npm run dev`, `npm run build`, `npx prisma db push`).
- **Code Standards:** Cách đặt tên file, cách viết Server Actions, cách xử lý lỗi.
- **Tone & Persona:** Yêu cầu AI luôn giải thích ngắn gọn 2-3 gạch đầu dòng về lý do kỹ thuật.

---

## Smart Vibe-Coding Checklist (Chia nhỏ 5 Sprint)

### Sprint 1: Khởi tạo Nền tảng, Agent Context & Setup UI (Tuần 1 - Tuần 2)
- [x] **Task 1.0:** Tạo bộ file Context `AGENT.md`, `docs/ARCHITECTURE.md`, `docs/TASK_LIST.md` để đồng bộ ngữ cảnh cho mọi AI agent.
- [x] **Task 1.1:** Khởi tạo dự án Next.js 15 với TypeScript, Tailwind CSS và bộ UI `shadcn/ui`.
- [ ] **Task 1.2:** Dựng Landing Page với lời kêu gọi hành động (Call To Action) và giao diện giới thiệu tính năng.
- [ ] **Task 1.3:** Xây dựng màn hình Cấu hình phỏng vấn (`/interview/setup`):
  - Component Drag & Drop Upload PDF CV.
  - Textarea dán JD.
  - Bộ chọn Ngôn ngữ (Tiếng Việt / Tiếng Anh).
  - Bộ chọn Persona (HR Thân thiện / Manager Khó tính / Tech Lead).
- [ ] **Task 1.4:** Viết module PII Masking client-side hoặc server-side (che SĐT, Email trước khi gửi).

### Sprint 2: Não bộ AI - Gemini 2.0 Flash API (Tuần 3 - Tuần 4)
- [ ] **Task 2.1:** Thiết lập API Key Gemini và cấu hình SDK trong dự án.
- [ ] **Task 2.2:** Viết Server Action gửi file PDF CV và JD đến Gemini với prompt chuyên biệt theo Persona.
- [ ] **Task 2.3:** Gemini phân tích so khớp: Trích xuất kỹ năng nổi bật, phát hiện điểm thiếu trong CV và sinh 5 câu hỏi phỏng vấn chuẩn hóa (JSON output).
- [ ] **Task 2.4:** Hiển thị preview bộ câu hỏi và thông số phân tích trước khi bước vào phòng phỏng vấn.

### Sprint 3: Phòng phỏng vấn giả lập (Text $\rightarrow$ Voice) (Tuần 5 - Tuần 7)
- [ ] **Task 3.1:** Giao diện Phòng phỏng vấn `/interview/[id]` mô phỏng phòng họp online trực quan.
- [ ] **Task 3.2:** Luồng hỏi - đáp từng lượt bằng Text cơ bản (đảm bảo logic chạy mượt trước khi gắn voice).
- [ ] **Task 3.3:** Tích hợp Web Speech API (Speech-to-Text) hỗ trợ cả Tiếng Việt và Tiếng Anh + Hiệu ứng sóng âm (Waveform).
- [ ] **Task 3.4:** Tích hợp Text-to-Speech (TTS) để AI phát âm câu hỏi bằng giọng tự nhiên theo ngôn ngữ đã chọn.

### Sprint 4: Đánh giá STAR & Báo cáo Điểm số (Tuần 8 - Tuần 10)
- [ ] **Task 4.1:** Xây dựng Engine chấm điểm STAR (Đánh giá từng tiêu chí: Situation, Task, Action, Result từ 1-10).
- [ ] **Task 4.2:** Thiết kế trang Báo cáo kết quả (`/interview/[id]/result`):
  - Biểu đồ mạng nhện (Radar Chart) thể hiện 4 chỉ số STAR.
  - Nhận xét chi tiết điểm mạnh và điểm cần cải thiện cho từng câu trả lời.
  - Cung cấp "Câu trả lời gợi ý chuẩn điểm 10" để ứng viên học hỏi.

### Sprint 5: Database, Authentication & Chuẩn bị Thi Khởi Nghiệp (Tuần 11 - Tuần 12)
- [ ] **Task 5.1:** Tích hợp Clerk Auth hoặc NextAuth với Google Login.
- [ ] **Task 5.2:** Thiết lập Prisma ORM kết nối PostgreSQL Cloud (Supabase/Neon), lưu trữ phiên phỏng vấn và kết quả.
- [ ] **Task 5.3:** Màn hình Dashboard quản lý lịch sử và biểu đồ tiến bộ của ứng viên qua từng buổi.
- [ ] **Task 5.4:** Hoàn thiện kịch bản thuyết trình, số liệu Unit Economics (~200đ/buổi), bảo mật PII và kịch bản demo trên sân khấu.

---

## Verification Plan

### Automated Tests
- Kiểm tra build hệ thống: `npm run build`
- Kiểm tra TypeScript types: `npx tsc --noEmit`

### Manual Verification
1. **Kiểm thử Multi-Agent Handover:** Mở repo bằng một AI tool khác (như Cursor, Claude Code hay Windsurf), yêu cầu AI đó đọc `AGENT.md` $\rightarrow$ Xác nhận AI nắm chính xác tech stack và tiếp tục làm task dở dang mà không bị lệch hướng.
2. **Kiểm thử PII Masking:** Upload CV có số điện thoại và email $\rightarrow$ Kiểm tra nội dung gửi tới AI đã được ẩn thành `[REDACTED_PHONE]` và `[REDACTED_EMAIL]`.
3. **Kiểm thử Persona & Song ngữ:** Đảm bảo 3 phong cách và 2 ngôn ngữ hoạt động chính xác.