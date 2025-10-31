# **Repository Design Pattern – Mẫu Thiết Kế Kho Lưu Trữ**

## **1. Giới thiệu**

### **Mục tiêu:**

**Repository Pattern** là mẫu thiết kế thuộc nhóm **Domain/Architecture Pattern**, thường được dùng trong **Domain Layer** của ứng dụng.
Mục đích là tạo ra một **lớp trung gian giữa Business Logic và Data Source** (Database, API, Supabase, File…), giúp:

* Tách biệt **logic truy cập dữ liệu** khỏi **logic nghiệp vụ**.
* Che giấu chi tiết lưu trữ: SQL, REST API, Supabase, EF Core…
* Dễ thay đổi nguồn dữ liệu nhưng **không ảnh hưởng Domain/Frontend**.

Trong dự án, Repository giúp ViewModel/Service không phải gọi trực tiếp API, không cần biết URL, JSON format, query… → code sạch và dễ test hơn.

---

## **2. Vấn đề cần giải quyết**

* Nếu ViewModel hoặc Service gọi thẳng API:

  * Rất khó test.
  * Rất nhiều duplicate code (call API, deserialize JSON, handle errors…).
  * UI bị phụ thuộc vào cấu trúc API backend.
  * Khi đổi backend (REST → Supabase → local DB), phải sửa toàn bộ.

**Repository Pattern** giải quyết bằng cách:

* Định nghĩa **interface chung để truy vấn dữ liệu**.
* Mọi thao tác đọc/ghi dữ liệu đều đi qua repository.
* Backend/API chỉ còn là *implementation detail*.

---

## **3. Cấu trúc mẫu**

```plaintext
UI (ViewModel)
   ↓
Service (Business Logic)
   ↓
Repository (Interface)
   ↓
Repository Implementation (REST API / Supabase / Database)
   ↓
Data Source
```

**Giải thích:**

* UI chỉ gọi Service.
* Service chỉ biết Repository interface, không biết REST API.
* Repository implementation chịu trách nhiệm gọi API, chuyển đổi dữ liệu.

---

## **4. Cách thực hiện**

### **Bước 1: Định nghĩa Repository Interface**

```csharp
public interface IProductRepository
{
    Task<IEnumerable<ProductDto>> GetAllAsync();
    Task<ProductDto?> GetByIdAsync(int id);
    Task<ProductDto> CreateAsync(ProductDto dto);
    Task<bool> DeleteAsync(int id);
}
```

> Interface này nằm ở **Domain Layer**, không phụ thuộc công nghệ (REST/Supabase/SQLite).

---

### **Bước 2: Viết Repository Implementation (REST API / Supabase)**

Ví dụ với REST API:

```csharp
public class ProductRepository : IProductRepository
{
    private readonly HttpClient _http;

    public ProductRepository(HttpClient http)
    {
        _http = http;
    }

    public async Task<IEnumerable<ProductDto>> GetAllAsync()
        => await _http.GetFromJsonAsync<IEnumerable<ProductDto>>("api/products");

    public async Task<ProductDto?> GetByIdAsync(int id)
        => await _http.GetFromJsonAsync<ProductDto>($"api/products/{id}");

    public async Task<ProductDto> CreateAsync(ProductDto dto)
    {
        var res = await _http.PostAsJsonAsync("api/products", dto);
        return await res.Content.ReadFromJsonAsync<ProductDto>();
    }

    public async Task<bool> DeleteAsync(int id)
    {
        var res = await _http.DeleteAsync($"api/products/{id}");
        return res.IsSuccessStatusCode;
    }
}
```

---

### **Bước 3: Service gọi Repository**

```csharp
public class ProductService
{
    private readonly IProductRepository _repo;

    public ProductService(IProductRepository repo)
    {
        _repo = repo;
    }

    public Task<IEnumerable<ProductDto>> GetAllAsync()
        => _repo.GetAllAsync();
}
```

---

### **Bước 4: ViewModel gọi Service**

```csharp
public class ProductListViewModel : ObservableObject
{
    private readonly ProductService _service;

    public ObservableCollection<ProductViewModel> Products { get; } = new();

    public ProductListViewModel(ProductService service)
    {
        _service = service;
    }

    public async Task LoadAsync()
    {
        var dtoList = await _service.GetAllAsync();
        // dùng Adapter để chuyển DTO → ViewModel
    }
}
```

---

## **5. Lợi ích mang lại**

| Lợi ích                       | Mô tả                                                          |
| ----------------------------- | -------------------------------------------------------------- |
| **Tách biệt tầng dữ liệu**    | UI và business không phụ thuộc API/backend.                    |
| **Dễ thay đổi nguồn dữ liệu** | Chỉ cần thay repository implementation.                        |
| **Dễ unit test**              | Mock repository mà không cần gọi API thật.                     |
| **Giảm duplicate code**       | Việc call API, parse JSON, xử lý lỗi tập trung tại repository. |
| **Code sạch và rõ ràng**      | Mỗi tầng có nhiệm vụ riêng, ít rối.                            |

---

## **6. Khi nào nên dùng**

| Nên dùng khi                                  | Không nên dùng khi                             |
| --------------------------------------------- | ---------------------------------------------- |
| App có nhiều nơi truy cập dữ liệu giống nhau. | Project nhỏ, chỉ vài endpoint đơn giản.        |
| Muốn tách UI khỏi logic truy cập dữ liệu.     | Khi repository chỉ wrap 1–2 API → quá dư thừa. |
| Cần test logic nghiệp vụ bằng mock.           | Backend thay đổi liên tục và không ổn định.    |

---

## **7. Tóm tắt ngắn gọn**

> **Repository Pattern** là lớp trung gian giữa Business Logic và Data Source, giúp toàn bộ ứng dụng không phụ thuộc vào cách dữ liệu được lưu trữ (REST, DB, Supabase…).
> Khi thay đổi backend, chỉ cần thay Implementation — toàn bộ UI/Domain vẫn giữ nguyên. <br>
> Trong kiến trúc phần mềm, tui thấy Hoàng cũng đang dùng Repository pattern