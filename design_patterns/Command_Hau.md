# Command Pattern

## Mục đích
Tách biệt phần **xử lý hành động (business logic)** khỏi **giao diện (UI logic)**.  
Pattern này đặc biệt hữu ích trong **WinUI**, **WPF**, hoặc bất kỳ framework nào dùng **data binding + MVVM**.

---

## Tình huống áp dụng trong dự án 
Khi người dùng bấm:
- “Đăng nhập”
- “Thêm sản phẩm”
- “In đơn hàng”
- “Xóa đơn hàng”
- “Lọc sản phẩm”

Mỗi hành động nên là **một Command riêng biệt**, để có thể dễ dàng **bind vào Button trong XAML** mà **không cần viết event code-behind**.

---

## Ví dụ thực tế

### 🔹 Command cơ bản:
```csharp
public class RelayCommand : System.Windows.Input.ICommand
{
    private readonly Action<object?> _execute;
    private readonly Func<object?, bool>? _canExecute;

    public event EventHandler? CanExecuteChanged;

    public RelayCommand(Action<object?> execute, Func<object?, bool>? canExecute = null)
    {
        _execute = execute;
        _canExecute = canExecute;
    }

    public bool CanExecute(object? parameter) => _canExecute?.Invoke(parameter) ?? true;

    public void Execute(object? parameter) => _execute(parameter);

    public void RaiseCanExecuteChanged() => CanExecuteChanged?.Invoke(this, EventArgs.Empty);
}
```

### 🔹 Sử dụng Command trong ViewModel:
```csharp
public class LoginViewModel
{
    public string Username { get; set; }
    public string Password { get; set; }

    public RelayCommand LoginCommand { get; }

    public LoginViewModel()
    {
        LoginCommand = new RelayCommand(ExecuteLogin, CanLogin);
    }

    private void ExecuteLogin(object? obj)
    {
        // Logic đăng nhập
        if (Username == "admin" && Password == "123")
        {
            // gửi thông điệp qua Mediator chẳng hạn
        }
    }

    private bool CanLogin(object? obj)
    {
        return !string.IsNullOrEmpty(Username) && !string.IsNullOrEmpty(Password);
    }
}
```

### 🔹 Bind Command trong XAML:
```xml
<Button Content="Đăng nhập" Command="{Binding LoginCommand}" />
```

### Kết quả
Cực kỳ gọn, không cần event Click, giúp tách biệt rõ ràng giữa UI và logic.