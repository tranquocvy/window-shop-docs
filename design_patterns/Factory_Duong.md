- Tài liệu có sử dụng ChatGPt để thu thập thông tin, song song có những phần sẽ gây khó hiểu đã được người làm để lại comment để giải thích cụ thể hơn.
- Cần dành ít nhất 30 phút để đọc và hiểu nội dung về 2 pattern bên dưới. 


# Liệu có nên dùng Mediator pattern, Factory pattern trong dự án ?

Tóm tắt câu trả lời

| Pattern      | Có nên dùng không? | Ứng dụng trong dự án của bạn                                                                        | Gợi ý ví dụ                                                 |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **Mediator** | **Có nên**       | Giúp tách biệt giao tiếp giữa ViewModel và các màn hình (Pages, Dialogs), đặc biệt trong WinUI MVVM | Dùng để gửi thông điệp (event/message) giữa các ViewModel |
| **Factory**  | **Rất nên**      | Tạo các đối tượng (Repository, ViewModel, hoặc Service) mà không cần biết chi tiết cách khởi tạo    | Kết hợp cùng Dependency Injection                         |  


  



# 1. Mediator - Người trung gian (phù hợp cho FE)

-**Mục đích**:
    Trong `WinUI`, ta thường có nhiều `ViewModel` (`DashboardViewModel`, `ProductViewModel`, `OrderViewModel`,…).
    __Chúng không nên gọi trực tiếp lẫn nhau__, mà nên giao tiếp thông qua một `lớp trung gian — Mediator` -> Các đối tượng giao tiếp qua các đối tượng trung gian.

-**Ví dụ**:
    Bạn có `OrderViewModel` tạo đơn hàng mới → cần thông báo `DashboardViewModel` cập nhật lại tổng doanh thu trong ngày.
Rất hay — bạn đang làm một **dự án WinUI + C#** khá hoàn chỉnh, với nhiều chức năng thực tế (quản lý sản phẩm, đơn hàng, báo cáo, cấu hình, phân quyền…).
Với quy mô như vậy, việc áp dụng **design patterns** là *rất nên làm*, vì nó giúp code gọn gàng, dễ mở rộng và bảo trì.

Thay vì gọi trực tiếp:

```csharp
dashboardViewModel.RefreshData();
```

Ta dùng Mediator:

#### Bước 1: Tạo interface Mediator

```csharp
//Là interface chung cho mọi đối tượng Mediator
public interface IMediator 
{   /*
    Register - Đăng ký: liên kết hàm callBack với use case Key.
        Key là use case được thực hiện   
        callBack chính là hàm mà sẽ được thực hiện sau khi Key hoàn thành.
    */
    void Register(string key, Action<object?> callback);
    
    //Nếu Register là thứ liên kết giữa Key và các callback function
    //Thì Notify chính là sự thực thi các callback function đó.
    void Notify(string key, object? data = null);

    //Cụ thể hơn, xem các bước bên dưới.
}
```

#### Bước 2: Tạo một Mediator thật

```csharp
//Tạo Mediator để xử lý các ViewModel gọi là AppMediator
public class AppMediator : IMediator
{   
    //Đây chính là mapping giữa một Key - câu lệnh được thực hiện 
        //Và chuỗi các hàm cần được thực hiện ngay say khi Key hoàn thành.
    private readonly Dictionary<string, Action<object?>> _subscribers = new();
    ///Summary
    /// - Trong Dictionary <string, Action <object>> Có nghĩa là tương ứng với một Key là
    /// kiểu String, nó sẽ lưu một mảng của các con trỏ hàm đúng không ? - Khi 1 use case
    /// "string" (là key) được gọi thì các hàm được gán trong mảng của con trỏ hàm đó sẽ
    /// được thực hiện ?


    public void Register(string key, Action<object?> callback)
    {   
        //Nếu chưa tồn tại Key thì tạo Key
        //Sau đó tạo liên kết giữa Key và hàm callback
        if (!_subscribers.ContainsKey(key))
            _subscribers[key] = callback;
        else
            _subscribers[key] += callback;
    }

    public void Notify(string key, object? data = null)
    {
        if (_subscribers.ContainsKey(key))
            _subscribers[key]?.Invoke(data);
    }
}
```

#### Bước 3: Đăng ký và sử dụng

```csharp
// DashboardViewModel.cs
public class DashboardViewModel
{
    public DashboardViewModel(IMediator mediator)
    {
        mediator.Register("OrderCreated", OnOrderCreated);
    }

    private void OnOrderCreated(object? data)
    {
        // ví dụ: cập nhật lại tổng doanh thu
        RefreshDashboard();
    }

    private void RefreshDashboard()
    {
        // Gọi service cập nhật số liệu
    }
}

// OrderViewModel.cs
public class OrderViewModel
{
    private readonly IMediator _mediator;
    public OrderViewModel(IMediator mediator)
    {
        _mediator = mediator;
    }

    public void CreateOrder()
    {
        // ... logic tạo đơn hàng
        _mediator.Notify("OrderCreated", new { OrderId = 101 });
    }
}
```

 **Kết quả**:
Hai ViewModel **hoàn toàn không phụ thuộc vào nhau**, giúp dễ test và dễ mở rộng.  
Thực tế sẽ có nhiều hơn 2 loại ViewModel, khi đó nhiều sự liên kết giữa các ViewModel chắc chắn sẽ xuất hiện. Vậy nên ta cần ít nhất 1 lớp để quản lý các sự liên kết đó - chính là Mediator.  
**Lưu ý:** Một lớp Mediator có thể lưu nhiều liên kết giữa nhiều cặp ViewModel. Không nhất thiết phải sử dụng một thể hiện Mediator để lưu một cặp liên kết.  
  => Sử dụng Singleton để tạo một thể hiện duy nhất của Mediator sẽ là lựa chọn phù hợp.

* Link đọc thêm: [Một trang web giải thích cơ bản về Mediator pattern](https://viblo.asia/p/mediator-design-pattern-tro-thu-dac-luc-cua-developers-m68Z0jVj5kG)
---

### 1.1 So sánh giữa không sử dụng Mediator và sử dụng Mediator (Nếu muốn đọc thêm)

Chúng ta sẽ cùng phân tích **tình huống cụ thể**:

> “`OrderViewModel` tạo đơn hàng mới → cần báo `DashboardViewModel` cập nhật lại tổng doanh thu trong ngày.”

---

##  **Trường hợp 1: Không dùng Mediator**

###  Cấu trúc code

Giả sử trong WinUI (MVVM) có hai ViewModel:

* `OrderViewModel`: Xử lý logic khi người dùng tạo đơn hàng
* `DashboardViewModel`: Hiển thị tổng doanh thu trong ngày

Bạn muốn khi `OrderViewModel.CreateOrder()` được gọi, thì `DashboardViewModel` sẽ cập nhật lại dữ liệu.

###  Cách làm đơn giản nhất (không Mediator)

Thực hiện “nối thẳng” hai ViewModel với nhau:

```csharp
public class DashboardViewModel
{
    public decimal TotalRevenueToday { get; private set; }

    public void RefreshDashboard()
    {
        // Lấy lại dữ liệu doanh thu từ repository
        TotalRevenueToday = new OrderRepository().GetTodayRevenue();
        Console.WriteLine($"Dashboard updated: Revenue = {TotalRevenueToday}");
    }
}

public class OrderViewModel
{
    private readonly DashboardViewModel _dashboard;

    public OrderViewModel(DashboardViewModel dashboard)
    {
        _dashboard = dashboard;
    }

    public void CreateOrder()
    {
        // 1. Lưu đơn hàng mới vào DB
        new OrderRepository().AddNewOrder(...);

        // 2. Gọi dashboard cập nhật lại
        _dashboard.RefreshDashboard();
    }
}
```

---

##  **Vấn đề sẽ phát sinh**

|  Vấn đề                                  |  Giải thích                                                                                                                                                                                                                                                                |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Phụ thuộc chặt chẽ (tight coupling)** | `OrderViewModel` **phải biết** DashboardViewModel tồn tại, và biết luôn cách gọi `RefreshDashboard()`. Nếu sau này bạn đổi dashboard layout, đổi class name, đổi logic... thì OrderViewModel cũng phải sửa.                                                                  |
| **2. Khó mở rộng hoặc tái sử dụng**        | Nếu bạn có thêm `ReportViewModel`, `NotificationViewModel` cũng muốn “nghe” event “OrderCreated”, bạn phải gọi từng cái một: `_dashboard.RefreshDashboard(); _report.UpdateChart(); _notify.ShowPopup();` — rất rối, và sai nguyên tắc mở rộng/đóng (Open-Closed Principle). |
| **3. Khó test đơn vị (Unit Test)**         | Khi test `OrderViewModel`, bạn phải mock cả DashboardViewModel — tức là test bị phụ thuộc.                                                                                                                                                                                   |
| **4. Dễ tạo vòng lặp phụ thuộc**           | Nếu DashboardViewModel ngược lại cũng cần gọi sang OrderViewModel (VD: click vào biểu đồ để xem đơn hàng hôm nay), bạn sẽ sớm gặp “dependency circle”.                                                                                                                       |
| **5. Mất khả năng thay thế**               | Bạn không thể dễ dàng thay Dashboard bằng màn hình khác mà không sửa code trong `OrderViewModel`.                                                                                                                                                                            |

---

##  Tóm lại: Không Mediator = **"ViewModel biết quá nhiều"**

```plaintext
OrderViewModel ---> DashboardViewModel
```

→ Quan hệ này khiến hai module dính nhau như keo, khó thay thế, khó mở rộng.

---

##  **Trường hợp 2: Có Mediator**

###  Sử dụng Mediator làm trung gian

```csharp
public interface IMediator
{
    void Register(string message, Action<object?> callback);
    void Notify(string message, object? data = null);
}

public class AppMediator : IMediator
{
    private readonly Dictionary<string, Action<object?>> _subscribers = new();

    public void Register(string message, Action<object?> callback)
    {
        if (_subscribers.ContainsKey(message))
            _subscribers[message] += callback;
        else
            _subscribers[message] = callback;
    }

    public void Notify(string message, object? data = null)
    {
        if (_subscribers.TryGetValue(message, out var callback))
            callback.Invoke(data);
    }
}
```

###  ViewModel sử dụng Mediator

```csharp
public class DashboardViewModel
{
    public DashboardViewModel(IMediator mediator)
    {
        mediator.Register("OrderCreated", _ => RefreshDashboard());
    }

    public void RefreshDashboard()
    {
        Console.WriteLine("Dashboard updated via Mediator!");
    }
}

public class OrderViewModel
{
    private readonly IMediator _mediator;

    public OrderViewModel(IMediator mediator)
    {
        _mediator = mediator;
    }

    public void CreateOrder()
    {
        // Lưu đơn hàng
        Console.WriteLine("Order created!");

        // Báo cho ai quan tâm biết
        _mediator.Notify("OrderCreated");
    }
}
```

###  Kết quả

* `OrderViewModel` **không cần biết Dashboard tồn tại**
* `DashboardViewModel` **chỉ quan tâm khi có “OrderCreated”**
* Khi test `OrderViewModel`, bạn chỉ cần mock `IMediator`, không cần `Dashboard`

```plaintext
OrderViewModel --> Mediator --> DashboardViewModel
```

---

## **So sánh trực tiếp**

| Tiêu chí                        |  Không dùng Mediator                        |  Dùng Mediator                                       |
| ------------------------------- | -------------------------------------------- | ----------------------------------------------------- |
| **Quan hệ giữa ViewModel**      | Order biết rõ Dashboard                      | Hai ViewModel độc lập                                 |
| **Khả năng mở rộng**            | Mỗi lần thêm màn hình phải thêm code gọi tay | Chỉ cần đăng ký thêm 1 callback                       |
| **Khả năng test**               | Phải mock Dashboard                          | Chỉ mock Mediator                                     |
| **Khả năng thay đổi giao diện** | Sửa Dashboard => phải sửa Order              | Dashboard thay đổi, OrderViewModel không bị ảnh hưởng |
| **Phạm vi trách nhiệm**         | Vi phạm nguyên tắc SRP                       | Mỗi ViewModel chỉ lo việc của nó                      |
| **Khả năng tái sử dụng**        | Thấp                                         | Cao                                                   |

---

##  Tóm gọn:

> **Không Mediator** → Code chạy được, nhưng ViewModel “biết quá nhiều” → khó mở rộng, khó test.
>
> **Có Mediator** → Code dễ bảo trì, ViewModel “mù lẫn nhau” → chỉ nói chuyện qua kênh trung gian.


---  


# 2. Factory pattern 

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


