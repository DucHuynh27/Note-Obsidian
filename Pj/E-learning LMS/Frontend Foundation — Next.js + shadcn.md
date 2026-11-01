---
tags:
  - e-learning-lms
  - frontend
  - nextjs
  - shadcn
  - typescript
created: 2026-10-30
---

# Frontend Foundation — Next.js + shadcn

Kiến thức & gotchas rút ra khi dựng nền tảng frontend cho **E-learning LMS**
(Next.js 16 + React 19 + Tailwind v4 + shadcn/ui + TypeScript).

---
## 1. shadcn/ui (CLI 4.x)

- `shadcn init` phải chạy **trong `frontend/`**; chạy ở repo root sẽ rơi vào luồng **tạo project mới** (hỏi template/monorepo/tên).
- Phân biệt các option:
  - **template** = framework (next/vite/…)
  - **base** = thư viện headless: Base UI `/` Radix `/` Aria
  - **preset/style** = Nova/Vega/Maia… (combo font + icon + radius)
  - **base color** = neutral / zinc / stone / mauve / olive / mist / taupe
- Lựa chọn đã dùng: template Next · monorepo **No** · base **Base UI** · preset **Nova** (Lucide + Geist).
- `init` ghi `components.json`, tạo `lib/utils.ts` (export `cn` từ package **`cn`**, **không** phải clsx + tailwind-merge), component dựng trên **Base UI** (`@base-ui/react`), và **ghi đè `globals.css`**.
## 2. Theme token (Tailwind v4 + shadcn)

- `globals.css`: `@theme inline` map `--color-*` → biến; token dùng **`oklch()`**.
- Đổi brand: `--primary` / `--accent` + **cặp `*-foreground`** đảm bảo tương phản. Đừng để cả hai `*-foreground` "sáng".
- Lấy giá trị **chính xác** từ `node_modules/tailwindcss/theme.css` (blue-600, orange-500…) thay vì tự convert hex.
- **`.dark` phải đổi riêng** (quy ước shadcn: nền tối → màu nhấn nhạt hơn + chữ tối).
- **Tailwind JIT**: chỉ sinh class **thấy được dưới dạng chuỗi literal** trong source → `` `bg-${x}` `` **hỏng**; phải ghi thẳng `"bg-primary"`.
## 3. Font (next/font)

- **Static vs variable font**: variable = 1 file chứa cả dải nét → được **bỏ `weight`**; static = mỗi weight 1 file → **buộc khai `weight`**. Be Vietnam Pro là **static**.
  - Kiểm chứng qua CSS2 API: `wght@100..900` → `400: Font family not found` (là static).
- **Không đoán qua tên** font — xem thanh trượt trên Google Fonts hoặc thử CSS2 API.
- `Be_Vietnam_Pro({})` là **config lúc build** (quyết định tải file font nào); `className`/`lang` là **runtime**. Vì vậy `weight` nằm ở lời gọi hàm, không nằm trong JSX.
- Nối với Tailwind: `@theme inline { --font-sans: var(--font-sans) }` + `next/font` đặt `variable: "--font-sans"`.
- Bỏ 1 **override** (`--font-mono`) khác với bỏ tính năng — Tailwind có default nên `font-mono` vẫn chạy.

## 4. TypeScript / JSX (phần luyện nhiều nhất)

- Component function: **dữ liệu (`const`) trước**, **`return ( JSX )` sau**. JSX chỉ nhận **thẻ**, **biểu thức `{}`**, **fragment**; **không nhận câu lệnh** (`const`).
- `return` phải có **một gốc** → bọc `<>...</>`.
- **JSX phân biệt hoa/thường**: `<tag>` = thẻ DOM tĩnh (chuỗi), `<Tag>` = biến/component động. Biến tag **buộc viết hoa** (nên `{ as: Tag = "p" }` phải đổi tên).
- `cva` `VariantProps` trả `| null | undefined` và kiểu dễ bị nới `string` → khi làm **thẻ động** phải khai **union literal** (`"h1" | "h2" | "h3"`).
- **`ComponentProps` vs `ComponentPropsWithoutRef`**: cái sau bỏ `ref` (tránh xung đột khi spread polymorphic).
- Vì sao prop `as` khó: **mỗi element có interface DOM riêng** (`HTMLParagraphElement` ≠ `HTMLLabelElement`) → `ref` và **event handler** (`onCopy`, `onClick`…) bị gõ theo từng element, không tương thích. **Polymorphic `as` type-safe tuyệt đối gần như không đạt được.** Chọn:
  - **(A)** cast ở render → mất autocomplete cho người dùng.
  - **(B)** generics `<C extends TextTag>` + **1 cast nội bộ** → giữ autocomplete theo thẻ.
- `string → ComponentType` không cast thẳng được → **`as unknown as`** (escape hatch hợp lệ).
- **`tsc` (kiểu) ≠ `eslint` (quy tắc)** — lỗi `no-explicit-any` do ESLint, không phải tsc. Thay `any` bằng `Record<string, unknown>`.

## 5. Kỹ năng môi trường / gỡ lỗi

- Đọc **đường dẫn thật** (`ls -la`): thư mục tên ` components` (dấu cách đầu) làm file nằm sai chỗ; `tsc` vẫn quét vì `include: **/*.tsx`.
- Luôn **kiểm chứng bằng tài liệu/công cụ** thay vì tin trí nhớ — nhiều kiến thức shadcn/Next bị cũ và phải sửa lại giữa chừng.
---
## Roadmap 5 trang cốt lõi

1. **Home** — layout tĩnh + danh sách khóa học (`GET /v1/courses`)
2. **Course Listing** — fetch + phân trang (`GET /v1/courses?page&limit&q`)
3. **Course Single** — route động `[id]`, sections/lessons (`GET /v1/courses/:id`)
4. **Login/Register** — form + lưu token (`POST /v1/auth/login`)
5. **My Learning** — route bảo vệ (`GET /v1/enrolments/me`)
