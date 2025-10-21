# 🗓️ Meeting Notes

Thư mục này lưu trữ **biên bản họp (meeting notes)** của nhóm trong suốt quá trình phát triển dự án **Window Shop App**.

---

## 🧭 Mục đích

- Ghi lại nội dung, quyết định, và phân công công việc trong mỗi buổi họp.
- Theo dõi tiến độ, vấn đề phát sinh và hướng giải quyết.
- Giúp lead và giảng viên nắm rõ quá trình làm việc của nhóm.

---

## 📁 Cấu trúc & Quy tắc đặt tên file

Mỗi biên bản họp là **một file `.md`**, được đặt tên theo ngày họp:

```

meeting_notes/
├── meeting_2025-10-21.md
├── meeting_2025-10-28.md
├── meeting_2025-11-04.md
└── ...

```

📌 **Cú pháp tên file:**

```

meeting_YYYY-MM-DD.md

```

Ví dụ: `meeting_2025-10-21.md`

---

## 🧱 Cấu trúc mẫu của một biên bản họp

```markdown
# 📝 Meeting - 2025-10-21

**Thời gian:** 20:00 – 21:00
**Hình thức:** Online (Google Meet)
**Người ghi biên bản:** Vỹ

---

## 🎯 Mục tiêu cuộc họp

- Thống nhất kiến trúc tổng thể của ứng dụng.
- Phân chia công việc cho tuần tới.

---

## 👥 Thành viên tham dự

| Tên   | Có mặt |
| ----- | ------ |
| Vỹ    | có     |
| Vượng | có     |
| Dương | có     |
| Hoàng | không  |
| Hậu   | có     |

---

## 💬 Nội dung chính

- Xem lại tiến độ tuần trước.
- Chốt UI layout và tính năng chính.
- Phân tích yêu cầu của module quản lý sản phẩm.

---

## 📌 Quyết định & Phân công

| Công việc                | Người phụ trách | Deadline   |
| ------------------------ | --------------- | ---------- |
| Hoàn thiện thiết kế UI   | Vượng           | 2025-10-28 |
| Thiết kế CSDL sơ bộ      | Dương           | 2025-10-28 |
| Viết proposal hoàn chỉnh | Vỹ              | 2025-10-25 |

---

## 🚧 Vấn đề tồn tại

- Một số component WinUI chưa hoạt động ổn định.
- Cần xác định trước flow tạo hóa đơn trước khi code.

---

## 🗓️ Kế hoạch họp tiếp theo

**Thời gian:** 2025-10-28
**Nội dung dự kiến:**

- Review UI mockup
- Thử nghiệm giao tiếp giữa các module

---

> 📘 _Mỗi buổi họp nên có người ghi biên bản cụ thể. Các thành viên cần đọc lại và xác nhận nội dung sau khi đăng lên._
```

---

## 🧩 Quy trình làm việc

1. **Sau mỗi buổi họp**, người ghi biên bản:

   - Tạo file mới trong `meeting_notes/`.
   - Viết nội dung theo mẫu trên.

2. **Commit và push** vào nhánh của mình:

   ```bash
   git add meeting_notes/meeting_2025-10-21.md
   git commit -m "docs: add meeting note 2025-10-21 (Hậu)"
   git push origin hau
   ```

3. **Lead** review và merge về `main`.

---

## 💡 Gợi ý công cụ

- VS Code + extension _Markdown Preview Enhanced_ để xem nội dung rõ ràng.
- Có thể dùng **Mermaid** để vẽ sơ đồ quyết định hoặc luồng họp.
