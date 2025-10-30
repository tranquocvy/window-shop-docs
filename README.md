# Window Shop Docs

Kho lưu trữ **tài liệu dự án WinUI - Ứng dụng bán hàng cho cửa hàng nhỏ**.  
Repo này được sử dụng để **quản lý tài liệu Markdown** trong suốt quá trình phát triển project, bao gồm đề xuất, biên bản họp, thiết kế, và báo cáo tiến độ.

---

## Cấu trúc thư mục

```
window-shop-docs/
├── README.md                 # Giới thiệu và hướng dẫn sử dụng repo
├── meeting_notes/            # Biên bản họp (meeting minutes)
├── design_docs/              # Tài liệu thiết kế UML, UI, class diagram
├── templates/                # Mẫu file Markdown do lead cung cấp
├── project_proposals/        # Các project proposal
└── (các thư mục khác nếu phát sinh trong quá trình làm việc)
```
---

## Cách sử dụng

### 1. Clone repo

Thành viên clone repo về máy:

```bash
git clone https://github.com/tranquocvy/window-shop-docs.git
cd window-shop-docs
```

### 2. Thêm hoặc chỉnh sửa tài liệu

- Viết báo cáo theo mẫu có sẵn trong thư mục `templates/`.
- Lưu file vào thư mục tương ứng (vd. `meeting_notes/2025-10-21.md`).

### 3. Commit và push (đẩy trực tiếp lên `main`)

Vì repo này **chỉ chứa tài liệu**, nhóm **đẩy trực tiếp lên nhánh `main`** để tiết kiệm thời gian.
Các góp ý, phản hồi sẽ được **gửi trực tiếp qua Zalo hoặc comment trên file**.

```bash
git add .
git commit -m "add: week01 progress report"
git push origin main
```

---

## Quy tắc commit

Cần **ghi rõ mục đích commit** để dễ theo dõi lịch sử thay đổi.

| Prefix      | Ý nghĩa                           |
| ----------- | --------------------------------- |
| `add:`      | thêm tài liệu mới                 |
| `fix:`      | chỉnh sửa nội dung hiện có        |
| `update:`   | cập nhật tiến độ hoặc ghi chú nhỏ |
| `refactor:` | cấu trúc lại bố cục tài liệu      |

**Ví dụ:**

```bash
git commit -m "fix: adjust meeting summary format"
```

---

## Quy tắc làm việc nhóm

* Mỗi người tự phụ trách phần tài liệu được giao.
* Khi hoàn thành, push trực tiếp lên `main`.
* Nếu cần chỉnh sửa hoặc phản hồi, lead/team sẽ:

  * Comment trực tiếp trên file `.md`, **hoặc**
  * Góp ý qua nhóm **Zalo**.

---

## Lưu ý

- Chỉ commit file `.md` hoặc tài liệu liên quan (không commit file nhị phân, ảnh, hoặc build).
- Mỗi file Markdown nên có tiêu đề rõ ràng, dùng cú pháp [**Mermaid**](https://mermaid.js.org/) để vẽ UML hoặc flowchart nếu cần.
- Giữ lịch sử commit gọn gàng, tránh ghi “update” chung chung.

---

## Công cụ hỗ trợ

- **Markdown Preview Enhanced** (VS Code extension) để xem trước biểu đồ.
- **Mermaid** để vẽ UML trực tiếp trong file `.md`.

---

> _Repo này chỉ dành cho lưu trữ tài liệu Markdown của dự án WinUI.
> Mã nguồn của ứng dụng được lưu riêng trong repo `window-shop-app`._
