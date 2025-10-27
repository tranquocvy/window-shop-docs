# 🧾 Window Shop Docs

Kho lưu trữ **tài liệu dự án WinUI - Ứng dụng bán hàng cho cửa hàng nhỏ**.  
Repo này được sử dụng để **quản lý tài liệu Markdown** trong suốt quá trình phát triển project, bao gồm đề xuất, biên bản họp, thiết kế, và báo cáo tiến độ.

---

## 📂 Cấu trúc thư mục

```
window-shop-docs/
├── README.md # Giới thiệu và hướng dẫn sử dụng repo
├── meeting_notes/ # Biên bản họp (meeting minutes)
├── design_docs/ # Tài liệu thiết kế UML, UI, class diagram
├── templates/ # Mẫu file Markdown do lead cung cấp
└── project_proposals/ # Các project proposal
```

---

## 🧭 Cách sử dụng

### 🧱 1. Clone repo

Thành viên clone repo về máy:

```bash
git clone https://github.com/tranquocvy/window-shop-docs.git
cd window-shop-docs
```

### 🌿 2. Chuyển sang nhánh của bạn

Mỗi thành viên làm việc trên nhánh riêng:

```bash
git checkout <tên-nhánh-của-bạn>
```

Ví dụ:

```bash
git checkout duong
```

### ✏️ 3. Thêm hoặc chỉnh sửa tài liệu

- Viết báo cáo theo mẫu có sẵn trong thư mục `templates/`.
- Lưu file vào thư mục tương ứng (vd. `meeting_notes/2025-10-21.md`).

### 💾 4. Commit và push

```bash
git add .
git commit -m "add: week01 progress report"
git push origin <tên-nhánh-của-bạn>
```

### 🔀 5. Merge về nhánh chính (`main`)

Lead sẽ review và merge các nhánh vào `main` sau khi kiểm tra nội dung.

---

## 🔧 Quy trình làm việc nhóm (Workflow)

Để tránh chồng chéo và giúp việc review dễ dàng, nhóm tuân theo quy trình sau:

### 🧩 1. Giao task

- Lead giao task cụ thể trên file `Task Tracker` (Google Sheets).
- Mỗi task có **tên và mô tả ngắn**, **người phụ trách**, **deadline**.

---

### ✍️ 2. Thực hiện

- Thành viên **checkout nhánh riêng của mình** (`duong`, `hoang`, `hau`, `vuong`) hoặc tạo nhánh con tạm thời nếu task lớn.
- Thực hiện chỉnh sửa / bổ sung nội dung trong file `.md` tương ứng.
- Khi commit, dùng cú pháp rõ ràng:

  ```bash
  git commit -m "fix(docs): standardize technical tone in backend guideline document"
  ```

📘 **Gợi ý prefix commit:**

| Prefix      | Ý nghĩa                           |
| ----------- | --------------------------------- |
| `add:`      | thêm tài liệu mới                 |
| `fix:`      | chỉnh sửa nội dung hiện có        |
| `update:`   | cập nhật tiến độ hoặc ghi chú nhỏ |
| `refactor:` | cấu trúc lại bố cục tài liệu      |

---

### 🔍 3. Review

- Khi hoàn tất, tạo **Pull Request** vào nhánh `main`.
- Lead (hoặc reviewer được phân công) sẽ:

  - Kiểm tra format Markdown, văn phong, và độ chính xác nội dung.
  - Comment trực tiếp trong PR nếu cần chỉnh sửa.

---

### ✅ 4. Merge & theo dõi

- Sau khi PR được duyệt, Lead sẽ merge vào `main`.
- Thành viên **không tự merge hoặc push trực tiếp lên `main`**.
- Task được đánh dấu là **“Completed”** trong bảng quản lý task.

---

### ⚙️ Ví dụ luồng thực tế:

1. Lead tạo task: “Backend chuẩn hóa văn phong kỹ thuật trong tài liệu định hướng nhóm”.

2. Thành viên chỉnh sửa trên nhánh của mình.

3. Sau khi xong, commit và push:

   ```bash
   git commit -m "fix(docs): standardize technical tone in backend guideline document"
   git push origin duong
   ```

4. Tạo Pull Request → Lead review → Merge.

---

## 💡 Lưu ý

- Chỉ commit file `.md` hoặc tài liệu liên quan (không commit file nhị phân, ảnh, hoặc build).
- Mỗi file Markdown nên có tiêu đề rõ ràng, dùng cú pháp [**Mermaid**](https://mermaid.js.org/) để vẽ UML hoặc flowchart nếu cần.
- Giữ lịch sử commit gọn gàng, tránh ghi “update” chung chung.

---

## 🧰 Công cụ hỗ trợ

- **Markdown Preview Enhanced** (VS Code extension) để xem trước biểu đồ.
- **Mermaid** để vẽ UML trực tiếp trong file `.md`.
- **GitHub Pages (tuỳ chọn)** nếu nhóm muốn hiển thị tài liệu như website.

---

> 📘 _Repo này chỉ dành cho lưu trữ tài liệu Markdown của dự án WinUI.
> Mã nguồn của ứng dụng được lưu riêng trong repo `window-shop-app`._
