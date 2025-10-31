- Tài liệu có sử dụng ChatGPt để thu thập thông tin, song song có những phần sẽ gây khó hiểu đã được người làm để lại comment để giải thích cụ thể hơn.
- Cần dành ít nhất 30 phút để đọc và hiểu nội dung về pattern bên dưới. 


# Liệu có nên dùng Mediator pattern, Factory pattern trong dự án ?

Tóm tắt câu trả lời

| Pattern      | Có nên dùng không? | Ứng dụng trong dự án của bạn                                                                        | Gợi ý ví dụ                                                 |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **Factory**  | **Rất nên**      | Tạo các đối tượng (Repository, ViewModel, hoặc Service) mà không cần biết chi tiết cách khởi tạo    | Kết hợp cùng Dependency Injection                         |  


  


# 1. Factory pattern 

## 1. Factory Pattern là gì?

###  Định nghĩa ngắn gọn:

> **Factory Pattern** là một *creational design pattern* (mẫu thiết kế thuộc nhóm khởi tạo) —
> Nó **ẩn đi việc khởi tạo object cụ thể**, và thay vào đó cung cấp **một phương thức chung (factory method)** để tạo ra object.
  

###  Nói dễ hiểu:

Thay vì:

```csharp
var repo = new SqlProductRepository();
```

→ bạn dùng:

```csharp
var repo = RepositoryFactory.CreateProductRepository();
```

**Mục tiêu:**

* Không cho code bên ngoài “biết” hay “phụ thuộc” vào class cụ thể (`SqlProductRepository`).
* Khi muốn thay đổi (VD: từ SQL sang Mock, hay sang API), ta chỉ cần sửa trong **Factory**, không đụng các nơi khác.



###  Khi nào nên dùng:

* Khi bạn có **nhiều cách khác nhau để tạo ra cùng loại object.**
* Khi việc tạo object phức tạp (nhiều tham số, nhiều ràng buộc).
* Khi bạn muốn **giảm sự phụ thuộc** (loose coupling) giữa các phần trong hệ thống.



###  Ưu điểm:

| Ưu điểm                   | Giải thích                                                 |
| ------------------------- | ---------------------------------------------------------- |
| **Giảm phụ thuộc**        | Code bên ngoài chỉ biết interface, không biết class cụ thể |
| **Dễ thay đổi, dễ test**  | Muốn đổi kiểu repository hay service → chỉ đổi ở Factory   |
| **Tăng khả năng mở rộng** | Dễ thêm loại mới mà không phá vỡ code cũ                   |


---

## 2. Ví dụ thực tế trong dự án “Shop App”

Giờ ta chia thành **hai góc nhìn**:

* Backend (Repository cho dữ liệu sản phẩm)
* Frontend (WinUI ViewModel và UI service)



## (A) Backend – Factory cho Repository

Giả sử bạn có `ShopDB` dùng SQL, nhưng trong môi trường test bạn dùng dữ liệu giả (`MockRepository`).

### Interface chung

```csharp
public interface IProductRepository
{
    IEnumerable<Product> GetAll();
    Product GetById(int id);
}
```

### Implement thật (SQL)

```csharp
public class SqlProductRepository : IProductRepository
{
    public IEnumerable<Product> GetAll()
    {
        Console.WriteLine("Load từ SQL Database");
        return new List<Product> { new Product { Id = 1, Name = "Laptop" } };
    }

    public Product GetById(int id) => new Product { Id = id, Name = "Laptop" };
}
```

### Implement giả (Mock)

```csharp
public class MockProductRepository : IProductRepository
{
    public IEnumerable<Product> GetAll()
    {
        Console.WriteLine("Load từ mock data (test mode)");
        return new List<Product> { new Product { Id = 99, Name = "Test Product" } };
    }

    public Product GetById(int id) => new Product { Id = id, Name = "Mock Product" };
}
```

### Factory

```csharp
public static class RepositoryFactory
{
    public static IProductRepository CreateProductRepository(bool isTest = false)
    {
        if (isTest)
            return new MockProductRepository();
        else
            return new SqlProductRepository();
    }
}
```

### Sử dụng trong ViewModel (hoặc service layer)

```csharp
public class ProductViewModel
{
    private readonly IProductRepository _repository;

    public ProductViewModel(bool isTest = false)
    {
        _repository = RepositoryFactory.CreateProductRepository(isTest);
    }

    public void Load()
    {
        var products = _repository.GetAll();
        foreach (var p in products)
            Console.WriteLine($"{p.Id} - {p.Name}");
    }
}
```

 Kết quả:

```csharp
new ProductViewModel(false).Load();  // Dữ liệu SQL
new ProductViewModel(true).Load();   // Dữ liệu Mock
```

=> ViewModel không cần quan tâm repository thật hay giả → **chỉ Factory biết**.


---

## (B) Frontend – Factory trong WinUI (UI Factory / ViewModel Factory)

Trong WinUI (C# Desktop App), bạn có thể dùng Factory để:

* Tạo các **ViewModel** tương ứng với từng **View**.
* Tạo **Dialog** hay **Control** phù hợp theo context (VD: admin hoặc user).


### Ví dụ: ViewModel Factory trong WinUI

Giả sử app có 2 loại user: *Admin* và *Customer*
→ giao diện dashboard khác nhau.

### Interface chung cho ViewModel

```csharp
public interface IDashboardViewModel
{
    string Title { get; }
    void LoadData();
}
```

### 2 lớp cụ thể

```csharp
public class AdminDashboardViewModel : IDashboardViewModel
{
    public string Title => "Admin Dashboard";
    public void LoadData() => Console.WriteLine("Loading all statistics...");
}

public class CustomerDashboardViewModel : IDashboardViewModel
{
    public string Title => "Customer Dashboard";
    public void LoadData() => Console.WriteLine("Loading user purchases...");
}
```

### Factory cho ViewModel

```csharp
public static class ViewModelFactory
{
    public static IDashboardViewModel CreateDashboardViewModel(string role)
    {
        return role switch
        {
            "Admin" => new AdminDashboardViewModel(),
            "Customer" => new CustomerDashboardViewModel(),
            _ => throw new ArgumentException("Unknown role")
        };
    }
}
```

### Sử dụng trong App.xaml.cs hoặc MainWindow

```csharp
string currentUserRole = "Admin";
var dashboardVM = ViewModelFactory.CreateDashboardViewModel(currentUserRole);

Console.WriteLine(dashboardVM.Title);
dashboardVM.LoadData();
```

=> Nếu sau này thêm role mới (ví dụ “Manager”), chỉ cần thêm class mới + chỉnh trong Factory.
Không cần sửa UI hay MainWindow code.

---

## So sánh nhanh

| Môi trường           | Factory tạo gì                 | Mục đích chính                                       |
| -------------------- | ------------------------------ | ---------------------------------------------------- |
| **Backend**          | Repository (Data Access Layer) | Tách biệt nguồn dữ liệu (SQL, Mock, API)             |
| **Frontend (WinUI)** | ViewModel / Dialog / Service   | Tạo UI logic phù hợp với ngữ cảnh (role, feature, …) |



## Kết luận

| Ý chính                     | Diễn giải                                                                                |
| --------------------------- | ---------------------------------------------------------------------------------------- |
| **Factory Pattern**         | Là “nhà máy” sinh ra object mà che giấu chi tiết khởi tạo                                |
| **Lợi ích**                 | Dễ thay đổi loại object, giảm phụ thuộc, dễ test                                         |
| **Trong BE (Shop project)** | Factory tạo ra `Repository` (SQL / Mock / API)                                           |
| **Trong FE (WinUI)**        | Factory tạo ra `ViewModel` hoặc `UI service` phù hợp                                     |
| **Điểm cốt lõi**            | Code bên ngoài *chỉ làm việc với interface*, Factory *chịu trách nhiệm tạo class cụ thể* |

---


