# 📁 Supplementary Docs

Thư mục này được dùng để lưu **các tài liệu hoặc nhiệm vụ phát sinh trong quá trình làm việc nhóm**

---

## 🧭 Mục đích

Cụ thể:

- Dành cho **tài liệu cá nhân**, **yêu cầu riêng**, **hướng dẫn tạm**, **ghi chú thử nghiệm**, hoặc bất kỳ nội dung nào **phát sinh thêm** trong quá trình làm việc.
- Không cần được duyệt hoặc hợp nhất vào proposal chính.

Ví dụ:

- “Front viết tài liệu định hướng nhóm”
- “Backend thử thiết kế API mẫu”
- “QA viết checklist test nội bộ”
  → Tất cả đều được đặt trong `supplementary_docs/`.

---

## 🚀 Quy định làm việc với thư mục này

- Thành viên **push trực tiếp lên nhánh `main`**.  
  (Không cần pull request hoặc review, vì đây là task cá nhân)

---

## 🧩 Cách đặt tên file

### 🔹 Cấu trúc:

```

<ngày>_<người_thực_hiện>_<mô_tả_ngắn>.md

```

### 🔹 Quy ước:

| Thành phần          | Ý nghĩa                               | Ví dụ                                           |
| ------------------- | ------------------------------------- | ----------------------------------------------- |
| `<ngày>`            | Ngày tạo file, định dạng `YYYY-MM-DD` | `2025-10-26`                                    |
| `<người_thực_hiện>` | Tên hoặc vai trò thành viên           | `frontend`, `backend`, `qa`, `vuong`            |
| `<mô_tả_ngắn>`      | Mô tả ngắn gọn nội dung               | `team-guideline`, `api-draft`, `test-checklist` |

### 🔹 Ví dụ:

```

2025-10-26_frontend_team-guideline.md
2025-10-27_backend_api-draft.md
2025-10-28_qa_test-checklist.md

```

---

## 🧾 Mẫu nội dung file cá nhân

```markdown
# 🧩 Task: <tên tài liệu hoặc yêu cầu>

**Người thực hiện:**
**Ngày giao:**
**Hạn hoàn thành:**
**Trạng thái:** (Đang làm / Hoàn thành)

---

## 🎯 Mục tiêu

Mô tả ngắn gọn mục tiêu hoặc lý do của tài liệu / nhiệm vụ.

---

## 📋 Nội dung thực hiện

Liệt kê các bước, kết quả hoặc phần việc đã thực hiện.
```

---

## ⚠️ Lưu ý

- Đây **không phải khu vực làm việc chung**, mà là nơi để mỗi thành viên **nộp kết quả hoặc tài liệu phát sinh**.
- **Push trực tiếp lên nhánh `main`**, không cần Leader duyệt lại.
- Giữ quy tắc đặt tên file thống nhất để dễ tra cứu và quản lý.
