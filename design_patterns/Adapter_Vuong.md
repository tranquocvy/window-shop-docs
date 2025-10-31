# Adapter Design Pattern – Mẫu Thiết Kế Bộ Chuyển Đổi

## 1. Giới thiệu

### Mục tiêu:
**Adapter Pattern** (Bộ chuyển đổi) là mẫu thiết kế thuộc nhóm **Structural Pattern**.  
Mục đích của nó là **chuyển đổi giao diện của một lớp hoặc dữ liệu** sang dạng mà hệ thống khác có thể hiểu và sử dụng được — **mà không cần thay đổi code gốc**.

Trong frontend **MyShop (WinUI 3)**, pattern này đặc biệt hữu ích khi:
- Backend trả về dữ liệu (`DTO`) không khớp hoàn toàn với model UI (`ViewModel`).
- Cần **tách biệt giao diện người dùng khỏi cấu trúc dữ liệu backend**, giúp giảm sự phụ thuộc giữa hai bên.

---

## 2. Vấn đề cần giải quyết

Khi giao tiếp với Backend, frontend thường nhận được các `DTO` (Data Transfer Object) có cấu trúc phục vụ nghiệp vụ backend, ví dụ:

```json
{
  "id": 1,
  "name": "iPhone 15",
  "price": 25000000,
  "category": "Điện thoại",
  "createdAt": "2025-10-25T09:00:00Z"
}
```

Trong khi đó, UI (WinUI 3) cần hiển thị dữ liệu thân thiện và được định dạng lại, ví dụ:
- Hiển thị `price` dưới dạng **chuỗi tiền tệ** (“25.000.000 ₫”).
- Đổi tên thuộc tính hoặc gộp dữ liệu (`ProductName`, `DisplayPrice`, `CreatedDateString`...).
- Nếu để ViewModel tự xử lý việc chuyển đổi này thì ViewModel trở nên **nặng và khó bảo trì**.

**Giải pháp:** sử dụng **Adapter Pattern** để tách riêng phần chuyển đổi dữ liệu.

---

## 3. Cấu trúc mẫu

```plaintext
Client (UI / ViewModel)
   ↓
Adapter  ←—— adapts ——→  Adaptee (DTO / Backend Data)
```

- **Client:** Phần giao diện (ViewModel, View) sử dụng dữ liệu ở dạng riêng (ViewModel).
- **Adaptee:** Dữ liệu gốc (DTO) nhận từ API backend.
- **Adapter:** Lớp trung gian, chịu trách nhiệm chuyển đổi `Adaptee` sang định dạng `Client` cần.

---

## 4. Cách hiện thực trong MyShop (Frontend – WinUI 3)

### Bước 1: Định nghĩa DTO (backend trả về)

```csharp
public class ProductDto
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
    public DateTime CreatedAt { get; set; }
}
```

### Bước 2: Định nghĩa ViewModel (UI cần hiển thị)

```csharp
public class ProductViewModel
{
    public string ProductName { get; set; }
    public string DisplayPrice { get; set; }
    public string CategoryName { get; set; }
    public string CreatedDateString { get; set; }
}
```

### Bước 3: Tạo lớp Adapter

```csharp
public class ProductAdapter
{
    public ProductViewModel Convert(ProductDto dto)
    {
        return new ProductViewModel
        {
            ProductName = dto.Name,
            DisplayPrice = $"{dto.Price:N0} ₫",
            CategoryName = dto.Category,
            CreatedDateString = dto.CreatedAt.ToString("dd/MM/yyyy")
        };
    }

    public IEnumerable<ProductViewModel> ConvertList(IEnumerable<ProductDto> dtos)
        => dtos.Select(Convert);
}
```

### Bước 4: Sử dụng trong ViewModel

```csharp
public class ProductListViewModel : ObservableObject
{
    private readonly IAppService _appService;
    private readonly ProductAdapter _adapter = new();

    public ObservableCollection<ProductViewModel> Products { get; } = new();

    public async Task LoadProductsAsync()
    {
        var dtoList = await _appService.GetAllProductsAsync();
        var uiList = _adapter.ConvertList(dtoList);
        Products.Clear();
        foreach (var item in uiList)
            Products.Add(item);
    }
}
```

---

## 5. Lợi ích mang lại

| Lợi ích | Mô tả |
|----------|-------|
| **Tách biệt lớp dữ liệu và lớp hiển thị** | ViewModel không cần biết cấu trúc của DTO. |
| **Giảm phụ thuộc vào backend** | Nếu backend đổi tên field hoặc thêm thuộc tính, chỉ cần sửa trong Adapter. |
| **Tái sử dụng dễ dàng** | Adapter có thể được dùng lại cho nhiều ViewModel khác nhau. |
| **Dễ test, dễ mở rộng** | Có thể unit test riêng phần chuyển đổi dữ liệu. |

---

## 6. Khi nào nên sử dụng

| Nên dùng khi | Không nên dùng khi |
|---------------|--------------------|
| Backend và frontend dùng cấu trúc dữ liệu khác nhau. | Cấu trúc dữ liệu giống hệt nhau (chuyển đổi dư thừa). |
| Cần chuyển đổi/định dạng dữ liệu trước khi hiển thị. | Ứng dụng nhỏ, ít dữ liệu hiển thị. |
| Muốn giảm phụ thuộc giữa UI và backend. | Tốc độ quan trọng hơn tính trừu tượng. |

---

## 7. Tóm tắt ngắn gọn

> **Adapter Pattern** trong frontend MyShop đóng vai trò **bộ chuyển đổi dữ liệu**, giúp UI hoạt động độc lập với cấu trúc dữ liệu backend, dễ mở rộng và dễ bảo trì.  
> Nó là cầu nối “dịch ngôn ngữ” giữa **Backend (DTO)** và **Frontend (ViewModel)**.
