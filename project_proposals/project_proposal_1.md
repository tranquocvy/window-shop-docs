# 🧾 Project Report Template

## 0. Thông tin các thành viên

| STT | Họ và tên             | MSSV     | Nhiệm vụ chính | Ghi chú |
| --- | --------------------- | -------- | -------------- | ------- |
| 1   | Nguyễn Văn Bình Dương | 23120242 | BE             |         |
| 2   | Nguyễn Phúc Hậu       | 23120252 | FE             |         |
| 3   | Nguyễn Phúc Hoàng     | 23120264 | BE             |         |
| 4   | Nguyễn Khắc Vượng     | 23120409 | FE             |         |
| 5   | Trần Quốc Vỹ          | 23120410 | Lead           |         |

---

## 1. 🧩 Chức năng

- Liệt kê các chức năng chính của ứng dụng.

**Danh sách chức năng:**

- [ ] Chức năng 1: ...
- [ ] Chức năng 2: ...
- [ ] Chức năng 3: ...

---

## 2. 🎨 Giao diện (Prototype)

- Link Figma: [Figma Prototype](#)
- Mô tả ngắn gọn các luồng chức năng chính (VD: Màn hình đăng nhập → Trang chủ → Quản lý sản phẩm → Báo cáo doanh thu).

---

## 3. 👥 Làm việc nhóm

### 3.1 Phân công công việc

- Phân công và cập nhật công việc được thực hiện qua **Google Drive** (file `.xlsx` dùng để theo dõi tiến độ và nhiệm vụ của từng thành viên).

---

### 3.2 Công cụ quản lý & Chiến lược làm việc với Git

- **Theo dõi tiến độ & quản lý tài liệu:**

  - Nhóm sử dụng **2 repository trên GitHub**:
    - Repo 1: Lưu **tài liệu** (meeting notes, design, v.v.)
    - Repo 2: Lưu **mã nguồn dự án** (source code chính của ứng dụng)

- **Kênh liên lạc:** Zalo (trao đổi hằng ngày), Google Meet (họp nhóm định kỳ)

- **Chiến lược làm việc với Git:**
  - Nhánh chính: `main`
  - Nhánh Frontend: `vuong`, `hau`
  - Nhánh Backend: `hoang`, `duong`
  - **Quy trình làm việc:**
    1. Thành viên FE/BE làm việc và **push code lên nhánh cá nhân** tương ứng.
    2. **Lead sẽ review** nội dung trên GitHub.
    3. Sau khi kiểm tra và xác nhận ổn định, **Lead sẽ merge các nhánh vào `main`**.

---

## 4. 🧱 Kiến trúc phần mềm

Mô tả cách **Clean Architecture**, **3-Layer Architecture** và **MVVM** được áp dụng:

- Clean Architecture: ...
- 3 Layer (Presentation – Business – Data): ...
- MVVM: ...

(Sơ đồ hoặc ảnh minh họa nếu có)

---

## 5. 🧠 Design Patterns

Liệt kê các **Design Pattern** nhóm áp dụng (mỗi thành viên ít nhất 1 pattern, không tính Builder & Singleton).

| Pattern | Mục đích | Vị trí áp dụng trong dự án | Người thực hiện |
| ------- | -------- | -------------------------- | --------------- |
|         |          |                            |                 |
|         |          |                            |                 |

---

## 6. ✅ Đảm bảo chất lượng

### 6.1 Coding Convention

- Quy tắc đặt tên class, biến, method, file: ...
- Cấu trúc thư mục: ...
- Cách viết comment/documentation: ...

### 6.2 Testing

- **Manual Test:** ...
- **Unit Test:** ...
- **UI Automation Test:** ...

---

## 7. 🚀 Nâng cao

Nhóm có **5** thành viên → cần **5** tính năng nâng cao.

| Tính năng nâng cao | Người thực hiện | Mô tả ngắn gọn |
| ------------------ | --------------- | -------------- |
|                    |                 |                |
|                    |                 |                |

---

## 8. 🗓️ Kế hoạch nháp ban đầu

- **Ý tưởng ban đầu:**  
  Phát triển ứng dụng **Windows (WinUI)** dành cho **chủ cửa hàng nhỏ** để quản lý sản phẩm, doanh thu và các báo cáo bán hàng.  
  Ứng dụng hướng đến người dùng đơn lẻ (1 user – chủ cửa hàng), giúp họ quản lý dữ liệu bán hàng trực tiếp trên máy tính mà không cần kết nối internet.

- **Phạm vi dự kiến:**

  - Quản lý danh sách sản phẩm (thêm, sửa, xóa, tìm kiếm).
  - Ghi nhận và quản lý hóa đơn bán hàng.
  - Quản lý doanh thu, báo cáo theo ngày/tháng.
  - Lưu trữ dữ liệu cục bộ (local database hoặc file).
  - Giao diện thân thiện, dễ thao tác với người dùng phổ thông.

- **Thời gian dự kiến thực hiện:**

  - Tổng thời gian đồ án: **8–10 tuần** (tùy theo kế hoạch của môn học).
  - Hiện tại đang trong giai đoạn **Proposal 1** với **deadline còn 2 tuần**.

- **Mốc tiến độ chính (dự kiến):**

  - **Tuần 1:**

    - Họp nhóm lần đầu: xác định đề tài, phân chia vai trò (Lead, 2 FE, 2 BE).
    - Thống nhất yêu cầu hệ thống và chuẩn bị nội dung **Proposal 1**.

  - **Tuần 2:**

    - Hoàn thiện tài liệu **Proposal 1** (chức năng, giao diện mẫu, kiến trúc tổng quan).
    - Lead tổng hợp và nộp proposal.

  - **Tuần 3:**

    - Bắt đầu thiết kế **UI chi tiết** (Figma).
    - Thiết kế cấu trúc thư mục và khởi tạo **repository mã nguồn**.
    - Backend bắt đầu tạo các model và service cơ bản.

  - **Tuần 4:**

    - FE triển khai các màn hình cơ bản (Dashboard, Product List).
    - BE hoàn thiện xử lý CRUD sản phẩm.

  - **Tuần 5:**

    - Kết nối FE – BE, hoàn thiện chức năng thêm/sửa/xóa sản phẩm.
    - Bắt đầu phần quản lý hóa đơn.

  - **Tuần 6:**

    - Hoàn thiện module báo cáo, doanh thu.
    - Bổ sung xử lý ngoại lệ và validation dữ liệu.

  - **Tuần 7:**

    - Viết Unit Test cho các chức năng chính.
    - Tinh chỉnh UI, fix bug.

  - **Tuần 8:**
    - Chuẩn bị demo và tài liệu báo cáo.
    - Lead tổng hợp, kiểm tra toàn bộ trước khi nộp.
