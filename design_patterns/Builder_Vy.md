# **Builder Design Pattern – Mẫu Thiết Kế Trình Xây Dựng**

## 1. Giới thiệu

### Mục tiêu

**Builder Pattern** thuộc nhóm **Creational Pattern**, được sử dụng khi cần **xây dựng đối tượng phức tạp** với nhiều bước tạo lập, nhiều thuộc tính tùy chọn hoặc yêu cầu sự “lắp ráp” rõ ràng theo từng giai đoạn.

Mẫu này giúp:

* Tránh **constructor dài ngoằng** hoặc nhiều *overload constructor*.
* Giúp code **dễ đọc, dễ mở rộng**.
* Tách biệt **cách tạo object** khỏi **cách sử dụng object**.

Trong các dự án như **MyShop (WinUI + API)**, Builder đặc biệt hữu ích khi:

* Tạo **request** gửi lên API (nhiều field tùy chọn).
* Tạo **Entity phức tạp** trong domain (Order, Invoice, Product…).
* Cấu hình **UI component**, **Dialog**, **Report**, **Filter**.

---

## 2. Vấn đề cần giải quyết

Nhiều đối tượng trong hệ thống có:

* Nhiều thuộc tính,
* Một số bắt buộc, một số tùy chọn,
* Một số yêu cầu kiểm tra hoặc chuẩn hóa giá trị.

Ví dụ, khi tạo đơn hàng:

```csharp
var order = new Order(1, items, discount, customerNotes, shippingAddress, ...);
```

→ Constructor dài, khó hiểu.
→ Không biết field nào bắt buộc / tùy chọn.
→ Code dễ lỗi nếu truyền sai thứ tự.

**Giải pháp:** sử dụng Builder Pattern để tạo đối tượng theo từng bước.

---

## 3. Cấu trúc mẫu

```
Client
   ↓
Director (tùy chọn)
   ↓
Builder → tạo từng phần → trả về sản phẩm hoàn chỉnh
```

* **Product:** đối tượng cần xây dựng.
* **Builder:** khai báo các bước xây dựng object.
* **Concrete Builder:** cài đặt từng bước.
* **Director (optional):** điều phối thứ tự xây dựng.
* **Client:** gọi builder để tạo object theo nhu cầu.

---

## 4. Cách hiện thực trong MyShop

### Bước 1: Định nghĩa Product (đối tượng phức tạp)

```csharp
public class Order
{
    public int CustomerId { get; private set; }
    public List<OrderItem> Items { get; private set; }
    public decimal? Discount { get; private set; }
    public string? Note { get; private set; }

    public Order(int customerId, List<OrderItem> items, decimal? discount, string? note)
    {
        CustomerId = customerId;
        Items = items;
        Discount = discount;
        Note = note;
    }
}
```

### Bước 2: Tạo Builder

```csharp
public class OrderBuilder
{
    private int _customerId;
    private List<OrderItem> _items = new();
    private decimal? _discount;
    private string? _note;

    public OrderBuilder SetCustomer(int id)
    {
        _customerId = id;
        return this;
    }

    public OrderBuilder AddItem(OrderItem item)
    {
        _items.Add(item);
        return this;
    }

    public OrderBuilder SetDiscount(decimal discount)
    {
        _discount = discount;
        return this;
    }

    public OrderBuilder SetNote(string note)
    {
        _note = note;
        return this;
    }

    public Order Build()
    {
        if (_customerId == 0)
            throw new Exception("CustomerId is required.");

        if (!_items.Any())
            throw new Exception("Order must contain at least one item.");

        return new Order(_customerId, _items, _discount, _note);
    }
}
```

### Bước 3: Sử dụng Builder trong Use Case

```csharp
var order = new OrderBuilder()
    .SetCustomer(customerId)
    .AddItem(new OrderItem(productId, quantity))
    .SetDiscount(10)
    .SetNote("Giao nhanh trong ngày")
    .Build();
```

→ Dễ đọc, dễ mở rộng, không cần nhớ constructor phức tạp.

---

## 5. Lợi ích mang lại

| Lợi ích                   | Giải thích                                     |
| ------------------------- | ---------------------------------------------- |
| ✅ Dễ đọc, dễ hiểu         | Code tạo đối tượng giống “câu tiếng Anh” hơn.  |
| ✅ Tránh constructor dài   | Không cần tạo 10 overload cho từng trường hợp. |
| ✅ Tùy biến theo từng bước | Chỉ set những gì bạn cần.                      |
| ✅ Giảm sai sót            | Không nhầm lẫn thứ tự tham số.                 |
| ✅ Dễ test                 | Tạo object trong unit test tiện hơn.           |

---

## 6. Khi nào nên dùng

| Nên dùng khi                            | Không nên dùng khi                   |
| --------------------------------------- | ------------------------------------ |
| Object có nhiều tham số.                | Object đơn giản (2–3 field).         |
| Một số field bắt buộc, một số tùy chọn. | Chỉ cần constructor đơn giản.        |
| Có logic kiểm tra khi tạo object.       | Không có biến thể phức tạp.          |
| Tạo object theo nhiều bước khác nhau.   | Sử dụng static factory method là đủ. |

---

## 7. Tóm tắt nhanh

> **Builder Pattern** giúp tạo đối tượng phức tạp một cách linh hoạt, dễ đọc và ít lỗi.
> Trong MyShop, Builder phù hợp cho việc dựng **Order**, **Filter**, **Dialog**, **DTO gửi API**, hoặc bất kỳ object nào cần nhiều bước tạo lập.
