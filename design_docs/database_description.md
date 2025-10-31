#  Database description

> **Ghi chú:**  
> Vì đây là môn *Lập trình Window* - thầy không đặt nặng chấm diểm Database, nên cấu trúc khóa chính ở các bảng `User` hay `Customer` được đơn giản hóa.  
> Các `id` **hiển thị trên giao diện** sẽ có quy định riêng để dễ nhìn và chuyên nghiệp hơn.

---

## Quy ước ID - Primary key

| Loại ID | Phạm vi giá trị | Ghi chú |
|----------|----------------|---------|
| `user_id` | 1000 - 9999 | Hiển thị |
| `customer_id` | 10000 - 99999 | Hiển thị |
| `role_id` | 1, 2, … | Hiển thị |
| (Khác) | 1, 2, 3, … | Không hiển thị |

**Lý do:** Các ID quan trọng hiển thị cho người dùng nên có quy luật để dễ nhận diện, tăng tính thẩm mỹ và thể hiện sự chỉn chu của nhóm.

---

## 1️. User

| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|----------------|--------------|------------|----------|
| `user_id` | INT | PK, Identity | Bắt đầu từ 1000 |
| `user_full_name` | NVARCHAR(50) |  | Họ tên |
| `user_name` | VARCHAR(30) | UNIQUE, NOT NULL | Dùng đăng nhập |
| `password_hash` | VARCHAR(100) | NOT NULL | Lưu hash |
| `role_id` | INT | FK → Role | Liên kết quyền |
| `is_active` | BIT | DEFAULT 1 | Trạng thái tài khoản |
| `has_seen_guide` | BIT | DEFAULT 0 | Đã xem hướng dẫn chưa |

---

## 2️. Role

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `role_id` | INT | PK | Bắt đầu từ 1, 2, … |
| `role_name` | VARCHAR(30) | UNIQUE, NOT NULL | ‘Admin’ / ‘Sale’ / ‘Moderator’ |
| `description` | NVARCHAR(255) | NULL |  |

---

## 3️. Product

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `product_id` | INT | PK | Bắt đầu từ 1, 2, … |
| `product_name` | NVARCHAR(50) | NOT NULL |  |
| `category_id` | INT | FK → Category |  |
| `brand_name` | NVARCHAR(50) | FK → Brand |  |
| `cost_price` | DECIMAL(10,2) | NOT NULL | Giá nhập, báo cáo doanh thu |
| `sell_price` | DECIMAL(10,2) | NOT NULL | Giá bán |
| `stock_quantity` | INT | DEFAULT 0 | Tồn kho hiện tại |
| `description` | NVARCHAR(255) | NULL |  |
| `is_draft` | BIT | DEFAULT 0 | Cho phép lưu nháp khi auto-save |
| `created_at` | DATETIME | DEFAULT GETDATE() |  |
| `updated_at` | DATETIME | NULL | Thời gian update (giá, …) |

---

## 4️. Category

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `category_id` | INT | PK | Bắt đầu từ 1, 2, … |
| `category_name` | NVARCHAR(50) | UNIQUE, NOT NULL |  |
| `description` | NVARCHAR(255) | NULL |  |

---

## 5️. Order

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `order_id` | BigInt | PK | Id = "YYYYMMDD hiện tại + số tăng tự nhiên" VD: 202510300001 |
| `customer_id` | INT | FK → Customer |  |
| `user_id` | INT | FK → User | Người lập đơn |
| `order_date` | DATETIME | DEFAULT GETDATE() |  |
| `total_amount` | DECIMAL(10,2) | NOT NULL |  |
| `status` | VARCHAR(20) | DEFAULT 'Pending' | (Pending, Paid, Cancelled) |
| `payment_id` | INT | FK → Payment |  |
| `is_draft` | BIT | DEFAULT 0 | Cho auto-save |
| `note` | NVARCHAR(255) | NULL |  |

---

## 6️. OrderDetail

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `order_detail_id` | INT | PK | Bắt đầu từ 1, 2, 3 |
| `order_id` | INT | FK → Order |  |
| `product_id` | INT | FK → Product |  |
| `quantity` | INT | CHECK > 0 |  |
| `unit_price` | DECIMAL(10,2) | NOT NULL |  |
| `sub_total` | DECIMAL(10,2) | COMPUTED (Quantity * UnitPrice) |  |

---

## 7️. Customer

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `customer_id` | INT | PK | Bắt đầu từ 10000 |
| `customer_full_name` | NVARCHAR(50) | NOT NULL |  |
| `phone` | VARCHAR(15) | NULL |  |
| `email` | VARCHAR(50) | NULL |  |
| `address` | NVARCHAR(255) | NULL |  |
| `created_at` | DATETIME | DEFAULT GETDATE() |  |

---

## 8️. Payment

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `payment_id` | INT | PK | Bắt đầu từ 1 |
| `order_id` | INT | FK → Order |  |
| `payment_method` | VARCHAR(50) | NOT NULL | (Cash, Card, BankTransfer) |
| `amount` | DECIMAL(10,2) | NOT NULL |  |
| `payment_date` | DATETIME | DEFAULT GETDATE() |  |

---

## 9️. AppSetting

| Thuộc tính | Kiểu | Ràng buộc | Ghi chú |
|-------------|-------|------------|----------|
| `key` | VARCHAR(100) | PK |  |
| `value` | NVARCHAR(255) | NULL |  |
| `description` | NVARCHAR(255) | NULL |  |

---

## 10. Commissions (Hoa hồng / KPI cho Sale)

| Cột | Kiểu dữ liệu | Ràng buộc / Ghi chú |
|-----|----------------|---------------------|
| `commission_id` | INT (PK, Identity) | Bắt đầu từ 1 |
| `user_id` | INT (FK) |  |
| `month` | INT |  |
| `year` | INT |  |
| `total_sales` | DECIMAL(18,2) |  |
| `commission_rate` | DECIMAL(5,2) | % hoa hồng |
| `commission_amount` | DECIMAL(18,2) | Số tiền = commission_rate * total_sales |

---
