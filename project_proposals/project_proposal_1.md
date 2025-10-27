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
#### a. Cấu trúc Dự án (MVVM + 3-Layer Hybrid)

Mô hình hoá Kiến trúc phần mềm chia thành các Class Library riêng biệt (Dự kiến áp dụng):

```
MyShop.sln (Solution)
│
├── 📁 1. Core
│   └── 📦 MyShop.Domain (.NET Standard / .NET 6+)
│       └── 📁 Entities
│           ├── Product.cs
│           └── Order.cs
│
├── 📁 2. Application
│   └── 📦 MyShop.Application (.NET Standard / .NET 6+)
│       ├── 📁 Interfaces
│       │   ├── IProductRepository.cs
│       │   └── IEmailService.cs
│       ├── 📁 Services (or UseCases)
│       │   └── OrderProcessingService.cs
│       └── 📁 DTOs (Data Transfer Objects)
│           └── ProductDto.cs
│
├── 📁 3. Infrastructure
│   └── 📦 MyShop.Infrastructure (.NET 6+)
│       ├── 📁 Persistence (or DataAccess)
│       │   ├── AppDbContext.cs
│       │   └── Repositories
│       │       └── ProductRepository.cs  // Implements IProductRepository
│       └── 📁 ExternalServices
│           └── EmailService.cs         // Implements IEmailService
│
└── 📁 4. Presentation
    └── 📦 MyShop.Presentation.WinUI (WinUI Project)
        ├── 📁 Views
        │   └── ProductDetailPage.xaml
        ├── 📁 ViewModels
        │   └── ProductDetailViewModel.cs
        ├── 📁 Converters
        ├── 📁 Helpers
        └── App.xaml
```

**Giải thích vai trò và quy ước của từng Project:**

1. 📦 **MyShop.Domain**
    - **Trách nhiệm:** Chứa các đối tượng nghiệp vụ cốt lõi (Entities). Các lớp này là POCO thuần túy, chỉ chứa thuộc tính và các phương thức logic nghiệp vụ gắn liền với chính nó.
    - **Quy ước:** Tên class là danh từ, đại diện cho đối tượng nghiệp vụ (`Product`, `Customer`).
    - **Không phụ thuộc** vào bất kỳ project nào khác.
2. 📦 **MyShop.Application**
    - **Trách nhiệm:** Điều phối luồng dữ liệu và logic.
    - `Interfaces`: Định nghĩa các "hợp đồng" cho lớp Infrastructure. Ví dụ: "Tôi cần một thứ gì đó có thể lấy sản phẩm theo ID", nhưng tôi không quan tâm nó lấy từ đâu.
    - `Services/UseCases`: Chứa logic nghiệp vụ của ứng dụng. Ví dụ: `OrderProcessingService` sẽ nhận yêu cầu, sử dụng `IProductRepository` để kiểm tra sản phẩm, tính toán, và lưu đơn hàng.
    - `DTOs`: Các đối tượng truyền dữ liệu giữa các lớp. ViewModel sẽ làm việc với `ProductDto` thay vì `Product` entity trực tiếp, giúp tách biệt hoàn toàn lớp UI khỏi lớp Domain.
    - **Phụ thuộc** vào `MyShop.Domain`.
3. 📦 **MyShop.Infrastructure**
    - **Trách nhiệm:** Triển khai các "hợp đồng" đã định nghĩa ở lớp Application. Đây là nơi chứa các chi tiết kỹ thuật.
    - `Persistence`: Chứa mọi thứ liên quan đến database (DbContext của EF Core, các lớp Repository cụ thể).
    - `ExternalServices`: Chứa các lớp làm việc với dịch vụ bên ngoài (gửi email, thanh toán, gọi API khác...).
    - **Phụ thuộc** vào `MyShop.Application`.
4. 📦 **MyShop.Presentation.WinUI**
    - **Trách nhiệm:** Hiển thị giao diện và xử lý tương tác người dùng. Cấu trúc thư mục bên trong project này vẫn tuân theo MVVM (`Views`, `ViewModels`...) như chúng ta đã thảo luận.
    - **Sự thay đổi quan trọng:** `ViewModel` giờ đây sẽ **không** trực tiếp truy cập database. Thay vào đó, nó sẽ được **inject** các services từ lớp Application để sử dụng.

**Dependency Rule:**

> Chỉ được phụ thuộc “vào trong” — Presentation → Application → Domain
> Domain không phụ thuộc bất kỳ lớp nào khác.

---

#### b. Quy ước cho C#

##### 1. **Naming Rules**

| Loại                | Quy tắc                   | Ví dụ                                 |
| ------------------- | ------------------------- | ------------------------------------- |
| Namespace           | PascalCase                | `MyApp.ViewModels`                    |
| Class, Struct, Enum | PascalCase                | `ProductViewModel`, `OrderStatus`     |
| Interface           | PascalCase, tiền tố **I** | `IProductService`                     |
| Method (public)     | PascalCase, động từ       | `CalculateTotalPrice()`               |
| Method (private)    | _camelCase                | `_validateInput()`                    |
| Property            | PascalCase                | `UserName`, `TotalAmount`             |
| Field (private)     | _camelCase                | `_userRepository`                     |
| Parameter           | camelCase                 | `void AddProduct(Product newProduct)` |
| Constant            | ALL_CAPS                  | `MAX_BUFFER_SIZE`                     |

**Lưu ý (Có dặn trên lớp):**

* Không dùng Hungarian notation (ví dụ: `btnSave`, `txtUser`).
* Hậu tố `Async` cho các hàm bất đồng bộ (ví dụ: `GetUserAsync()`).

---

##### 2. **Formatting & Layout**

* **Sử dụng `var` một cách thông minh:**
    - **NÊN** dùng `var` khi kiểu dữ liệu được thể hiện rõ ràng ở phía bên phải của phép gán. Điều này giúp code ngắn gọn hơn.
        
        ```csharp
        // Good
        var products = new List<Product>();
        var user = _userService.GetUserById(1);
        ```
        
    - **KHÔNG NÊN** dùng `var` cho các kiểu dữ liệu cơ bản (`int`, `string`, `bool`, `double`) hoặc khi kiểu dữ liệu không rõ ràng.
        
        ```csharp
        // Bad
        var count = 10; // Nên dùng: int count = 10;
        var result = GetResult(); // Kiểu của result là gì?
        ```
        
* **Braces `{}`:**
    - **Luôn luôn** sử dụng dấu ngoặc `{}` cho các khối lệnh `if`, `for`, `foreach`, `while`, ngay cả khi nó chỉ chứa một dòng lệnh. Điều này tránh các lỗi logic tiềm ẩn khi thêm code sau này.
        
        ```csharp
        // Good
        if (user != null)
        {
            ProcessUser(user);
        }
        
        // Bad - Rất nguy hiểm
        if (user != null)
            ProcessUser(user); // Nếu sau này thêm 1 dòng nữa mà quên ngoặc -> lỗi logic
        
        ```
        
    - Đặt dấu `{` ở một dòng riêng, không đặt cùng dòng với câu lệnh.
* **Thứ tự trong class (optional):**

  1. Fields
  2. Constructors
  3. Properties
  4. Public Methods
  5. Private Methods

**Ví dụ chuẩn:**

```csharp
public class ProductService
{
    private readonly IProductRepository _productRepository;

    public ProductService(IProductRepository productRepository)
    {
        _productRepository = productRepository;
    }

    public async Task<ProductDto> GetProductAsync(int id)
    {
        return await _productRepository.GetByIdAsync(id);
    }

    private bool _isValidId(int id) => id > 0;
}
```

---

##### 3. **Comment & Documentation**

* Nên comment trả lời **“Why?”** thay vì “What?”.
* Sử dụng XML comment cho các class và method public → Điều này giúp IntelliSense hiển thị thông tin gợi ý và có thể dùng để tự động tạo tài liệu sau này.

```csharp
/// <summary>
/// Calculates final price after discount.
/// </summary>
public decimal CalculateFinalPrice(Product product) => product.Price * 0.9m;
```

---

##### 4. **Nguyên tắc lập trình**

* **SRP (Single Responsibility):** Mỗi class chỉ làm một việc.
* **Async/Await:** Khi một phương thức thực hiện các tác vụ I/O (gọi API, truy vấn database, đọc file), nó **phải** là `async` và trả về `Task` hoặc `Task<T>`.
  - Đặt hậu tố `Async` cho tất cả các phương thức bất đồng bộ. Ví dụ: `GetUserAsync()`, `SaveProductAsync()`.
  - **Không bao giờ** dùng `.Result` hay `.Wait()` để chặn một tác vụ `async`. Nó có thể gây ra `deadlock`. Hãy `await` "all the way up".
* **LINQ:** Ưu tiên sử dụng cú pháp phương thức (Method Syntax) hơn là cú pháp truy vấn (Query Syntax) vì tính linh hoạt và phổ biến hơn.     
  ```csharp
  // Preferred (Method Syntax)
  var expensiveProducts = products.Where(p => p.Price > 100).ToList();

  // Less Preferred (Query Syntax)
  var expensiveProductsQuery = from p in products
                                where p.Price > 100
                                select p;
  ```
* **Readonly:** Dùng `readonly` cho field không thay đổi.
* **Expression-bodied members:** Sử dụng cho các properties hoặc methods chỉ có một dòng lệnh để code trông gọn gàng hơn.
  ```csharp
  // Thay vì viết thế này:
  public string FullName
  {
      get { return $"{FirstName} {LastName}"; }
  }

  // Hãy viết thế này:
  public string FullName => $"{FirstName} {LastName}";
  ```

---

#### c. Quy ước cho XAML

##### 1. **Đặt tên Controls**

| Loại                  | Quy tắc                               | Ví dụ                                       |
| --------------------- | ------------------------------------- | ------------------------------------------- |
| Control Name (x:Name) | PascalCase, `[Function][ControlType]` | `SaveOrderButton`, `UserNameTextBox`        |
| Event Handler         | `[ControlName]_[Event]`               | `SaveOrderButton_Click`                     |
| Resource Key          | PascalCase + hậu tố mô tả             | `PrimaryButtonStyle`, `TitleTextBlockStyle` |

❌ Sai: `btnSubmit`, `txtName`
✅ Đúng: `SubmitButton`, `UserNameTextBox`

---

##### 2. **Thứ tự thuộc tính (optional)**

1. **Định danh:** `x:Name`, `x:Key`
2. **Layout & Vị trí:** `Grid.Row`, `Grid.Column`, `Grid.ColumnSpan`, `Margin`, `Padding`, `HorizontalAlignment`, `VerticalAlignment`, `Width`, `Height`
3. **Nội dung & Dữ liệu:** `Content`, `Text`, `ItemsSource`, `DataContext`
4. **Binding & Commands:** `Text="{Binding UserName}"`, `Command="{Binding SaveCommand}"`
5. **Styling & Giao diện:** `Style`, `Background`, `Foreground`, `FontSize`, `FontWeight`
6. **Trạng thái & Hành vi:** `Visibility`, `IsEnabled`
7. **Accessibility:** `AutomationProperties.Name`

**Ví dụ:**

```xml
<Button
    x:Name="SaveButton"
    Grid.Row="2"
    Margin="8"
    HorizontalAlignment="Right"
    Content="Save"
    Command="{Binding SaveCommand}"
    Style="{StaticResource PrimaryButtonStyle}" />
```

---

##### 3. **Style & Resource**

- **Không bao giờ hard-code** các giá trị như màu sắc (`Background="#FF00A2E8"`), kích thước font, hay margin trực tiếp trên control.
- **Giải pháp: Định nghĩa chúng một lần** trong `ResourceDictionary` (ví dụ: `App.xaml` hoặc một file resource riêng) và tái sử dụng thông qua `"{StaticResource ...}"` hoặc `"{ThemeResource ...}"`.
    - `StaticResource`: Hiệu năng cao hơn, dùng cho các resource không thay đổi khi ứng dụng đang chạy.
    - `ThemeResource`: Dùng cho các resource cần thay đổi theo theme của hệ thống (Light/Dark mode).

```xml
<!-- Trong App.xaml -->
<Application.Resources>
    <ResourceDictionary>
        <SolidColorBrush x:Key="AppBrandBlueBrush" Color="#0078D4"/>
        <Style x:Key="PrimaryButtonStyle" TargetType="Button">
            <Setter Property="Background" Value="{ThemeResource AppBrandBlueBrush}"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="Padding" Value="12,4"/>
        </Style>
    </ResourceDictionary>
</Application.Resources>

<!-- Trong View -->
<!-- BAD -->
<Button Content="Save" Background="#0078D4" Foreground="White" Padding="12,4"/>

<!-- GOOD -->
<Button Content="Save" Style="{StaticResource PrimaryButtonStyle}"/>

```

### 6.2 Testing



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
