# Tài liệu mô tả database

## Quy ước chung
- **Tên bảng:** snake_case, số nhiều (ví dụ: products, order_details).
- **Khóa chính:** {entity}_id, kiểu int, identity.
- **Khóa ngoại:** {ref}_id trỏ về {table}.{ref}_id, có index.
- **Thời gian:** datetime2. Mặc định khuyến nghị GETDATE()/GETUTCDATE().
- **Enum lưu int**.

## Bảng: app_setting
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| app_setting_id | int | PK, Identity, Not null | Khóa chính |
| key | nvarchar(100) | Not null, UNIQUE | Chỉ chữ/số/._ theo regex ở tầng ứng dụng |
| value | nvarchar(1000) | Null | Giá trị lưu dạng chuỗi |
| value_type | int | Not null, Default 0 | 0=String,1=Number,2=Decimal,3=Bool,4=Json |
| category | nvarchar(50) | Null | Nhóm cấu hình |
| description | nvarchar(255) | Null | Mô tả |
| is_system | bit | Not null, Default 0 | Cấu hình hệ thống |
| updated_at | datetime2 | Not null, Default GETUTCDATE() | Thời điểm cập nhật |


## Bảng: product
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| product_id | int | PK, Identity, Not null | Khóa chính |
| product_name | nvarchar(200) | Not null | Tên sản phẩm |
| category_id | int | FK -> categories.category_id, Not null | Nhóm sản phẩm |
| brand_name | nvarchar(100) | Not null | Thương hiệu |
| color | nvarchar(50) | Null | Màu |
| storage_capacity | int | Null, CHECK >= 0 | Dung lượng GB |
| processor | nvarchar(100) | Null | CPU |
| screen_size | decimal(5,2) | Null, CHECK >= 0 | Inch |
| battery_capacity | int | Null, CHECK >= 0 | mAh |
| image_url | nvarchar(300) | Null | URL hình chính |
| image_gallery_json | nvarchar(max) | Null | Danh sách ảnh (JSON) |
| cost_price | decimal(18,2) | Not null, CHECK >= 0 | Giá vốn |
| sell_price | decimal(18,2) | Not null, CHECK >= 0 | Giá bán |
| stock_quantity | int | Not null, Default 0, CHECK >= 0 | Tồn kho |
| description | nvarchar(max) | Null | Mô tả |
| is_draft | bit | Not null, Default 0 | Cho phép lưu nháp khi auto-save |
| created_at | datetime2 | Not null, Default GETDATE() | Ngày tạo |
| updated_at | datetime2 | Null | Ngày cập nhật |


## Bảng: user
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| user_id | int | PK, Identity, Not null | Khóa chính |
| user_full_name | nvarchar(150) | Not null | Họ tên Chính |
| user_name | varchar(50) | Not null, UNIQUE | username đăng nhập |
| password_hash | nvarchar(256) | Not null | Mật khẩu băm |
| role_id | int | FK -> roles.role_id, Not null | Vai trò |
| is_active | bit | Not null, Default 1 | Trạng thái |
| has_seen_guide | bit | Not null, Default 0 | Đã xem hướng dẫn |
| created_at | datetime2 | Not null, Default GETDATE() | Ngày tạo |
| activated_at | datetime2 | null | Null nếu user chưa mua. Có giá trị nếu user đã kích hoạt |


## Bảng: order_detail
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| order_detail_id | int | PK, Identity, Not null | Khóa chính |
| order_id | int | FK -> orders.order_id, Not null | Đơn hàng |
| product_id | int | FK -> products.product_id, Not null | Sản phẩm |
| quantity | int | Not null, CHECK >= 1 | Số lượng |
| unit_price | decimal(18,2) | Not null, CHECK >= 0 | Đơn giá tại thời điểm bán |


## Bảng: payment
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| payment_id | int | PK, Identity, Not null | Khóa chính |
| order_id | int | FK -> orders.order_id, Not null | Tham chiếu đơn hàng |
| payment_method | nvarchar(50) | Not null | Phương thức thanh toán (Cash, BankTransfer, CreditCard) |
| amount | decimal(18,2) | Not null, CHECK > 0 | Số tiền thanh toán |
| payment_date | datetime2 | Not null, Default GETDATE() | Ngày thanh toán |

## Bảng: category
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| category_id | int | PK, Identity, Not null | Khóa chính |
| category_name | nvarchar(100) | Not null, UNIQUE | Tên danh mục |
| description | nvarchar(255) | Null | Mô tả |

## Bảng: customer
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| customer_id | int | PK, Identity, Not null | Khóa chính |
| customer_name | nvarchar(150) | Not null | Tên khách |
| phone_number | nvarchar(20) | Not null | SĐT liên hệ |
| email | nvarchar(150) | Null | Email |
| address | nvarchar(255) | Null | Địa chỉ |
| type | int | Not null, Default 1 | Enum: 1=Regular,2=Student,3=VIP,4=Cancelled,5=Returned |
| total_purchased | decimal(18,2) | Not null, CHECK > 0 | Tổng tiền mà khách hàng mua |
| note | nvarchar(255) | Null | Ghi chú |
| is_active | bit | Not null, Default 1 | Trạng thái |
| created_at | datetime2 | Not null, Default GETDATE() | Ngày tạo |

## Bảng: order
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| order_id | int | PK, Identity, Not null | Khóa chính |
| customer_id | int | FK -> customers.customer_id, Null | Cho phép bán lẻ không lưu khách |
| user_id | int | FK -> users.user_id, Not null | Nhân viên tạo đơn |
| order_date | datetime2 | Not null, Default GETDATE() | Ngày đặt |
| status | int | Not null, Default 1 | Enum: 1=Pending,2=Processing,3=Completed,4=Cancelled,5=Returned |
| subtotal_amount | decimal(18,2) | Not null, Default 0 | Tổng chưa giảm/thuế |
| discount | decimal(18,2) | Not null, Default 0 | Giảm giá đơn |
| total_amount | decimal(18,2) | Not null | Thành tiền |
| notes | nvarchar(255) | Null | Ghi chú |

## Bảng: role
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| role_id | int | PK, Identity, Not null | Khóa chính |
| role_name | nvarchar(50) | Not null, UNIQUE | Tên vai trò |
| description | nvarchar(255) | Null | Mô tả |

## Bảng: commission
| Tên thuộc tính | Kiểu dữ liệu | Ràng buộc | Ghi chú |
|---|---|---|---|
| commission_id | int | PK, Identity, Not null | Khóa chính |
| user_id | int | FK -> users.user_id, Not null | Nhân viên nhận hoa hồng |
| month | int | Not null | Tháng nhận |
| year | int | Not null | Năm nhận |
| total_sales | decimal(18,2) | Not null, CHECK >= 0 | Tổng số tiền nhân viên bán được |
| commission_rate | decimal(18,2) | Not null, CHECK >= 0 | Phần trăm hoa hồng |
| commission_amount | decimal(18,2) | Not null, CHECK >= 0 | Tổng tiền hoa hồng của nhân viên |
| note | nvarchar(255) | Null | Ghi chú |
| created_at | datetime2 | Not null, Default GETDATE() | Ngày tạo |