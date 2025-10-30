# Strategy Pattern

## Mục đích
Tách biệt **thuật toán hoặc cách xử lý** ra khỏi **class chính**, cho phép **thay đổi linh hoạt hành vi** của đối tượng **mà không cần sửa code bên trong**.  
Pattern này rất hữu ích khi bạn có **nhiều cách thực hiện một hành động**, ví dụ như tính **giá giảm**, **phí vận chuyển**, hoặc **báo cáo doanh thu** theo các tiêu chí khác nhau.

---

## Tình huống áp dụng trong dự án
Trong ứng dụng **Shop bán hàng**, Strategy Pattern có thể áp dụng cho:
- Tính **giá khuyến mãi** theo từng loại khách hàng (VIP, thường, nhân viên,…)
- Tính **phí vận chuyển** theo phương thức giao hàng (nhanh, tiết kiệm, nội thành,…)
- Tạo **báo cáo doanh thu** với nhiều chiến lược lọc khác nhau (theo ngày, tháng, năm,…)

Thay vì viết `if/else` rườm rà trong ViewModel, bạn chỉ cần **thay đổi “chiến lược”** tại runtime.

---

## Ví dụ thực tế

### 🔹 Định nghĩa Strategy interface:
```csharp
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal price);
}
```
### 🔹 Các chiến lược cụ thể:
```csharp
public class NoDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price) => price;
}

public class PercentageDiscountStrategy : IDiscountStrategy
{
    private readonly decimal _percent;
    public PercentageDiscountStrategy(decimal percent)
    {
        _percent = percent;
    }

    public decimal ApplyDiscount(decimal price)
        => price - (price * _percent);
}

public class FixedAmountDiscountStrategy : IDiscountStrategy
{
    private readonly decimal _amount;
    public FixedAmountDiscountStrategy(decimal amount)
    {
        _amount = amount;
    }

    public decimal ApplyDiscount(decimal price)
        => Math.Max(0, price - _amount);
}
```

### 🔹 Áp dụng trong class Product / Order:
```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }

    private IDiscountStrategy _discountStrategy;

    public Product(string name, decimal price, IDiscountStrategy strategy)
    {
        Name = name;
        Price = price;
        _discountStrategy = strategy;
    }

    public void SetDiscountStrategy(IDiscountStrategy strategy)
    {
        _discountStrategy = strategy;
    }

    public decimal GetFinalPrice()
    {
        return _discountStrategy.ApplyDiscount(Price);
    }
}
```

### 🔹 Sử dụng trong ViewModel:
```csharp
public class OrderViewModel
{
    public void ApplyPromotion()
    {
        var product = new Product("Laptop MSI", 25000000m, new NoDiscountStrategy());

        // Áp dụng giảm giá phần trăm
        product.SetDiscountStrategy(new PercentageDiscountStrategy(0.1m)); // Giảm 10%

        Console.WriteLine($"Giá sau khi giảm: {product.GetFinalPrice():N0} VNĐ");
    }
}
```

### Kết quả:
Bạn có thể thay đổi chiến lược giảm giá linh hoạt mà không cần sửa code logic của Product hay Order.
Điều này giúp ứng dụng dễ mở rộng (thêm loại giảm giá mới) và bảo trì hơn rất nhiều.