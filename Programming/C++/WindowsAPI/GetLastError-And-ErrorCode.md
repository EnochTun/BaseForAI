# GetLastError And ErrorCode

## 1. `GetLastError()` là gì?

`GetLastError()` là hàm thuộc Windows API, dùng để lấy mã lỗi được lưu cho luồng hiện tại (_thread_).

Ví dụ:

```
DWORD ErrorCode = GetLastError();
```

- `DWORD`: kiểu số nguyên không dấu 32-bit của Windows.
- `ErrorCode`: tên biến dùng để lưu mã lỗi.
- `GetLastError()`: hàm trả về mã lỗi được lưu.

## 2. Tại sao gọi trong nhánh `else`?

```
if (GetComputerNameA(computerName, &size))
{
    std::cout << "Computer Name: " << computerName << '\n';
}
else
{
    DWORD ErrorCode = GetLastError();
}
```

`GetComputerNameA()` trả về kết quả cho biết thao tác thành công hay thất bại.
- Thành công: thực hiện nhánh `if`.
- Thất bại: thực hiện nhánh `else` và lấy mã lỗi để chẩn đoán.

Không nên lấy mã lỗi một cách tùy tiện khi API thành công, vì giá trị trả về từ `GetLastError()` có thể là mã lỗi còn lưu từ một thao tác trước đó.

## 3. Tại sao phải lưu mã lỗi ngay?

```
DWORD ErrorCode = GetLastError();
```

Khi API thất bại, nên lấy mã lỗi ngay lập tức nếu tài liệu của API quy định sử dụng `GetLastError()`.

Một số lời gọi API có thể làm thay đổi trạng thái mã lỗi được lưu. Vì vậy, nếu trì hoãn việc lấy mã lỗi, chương trình có thể chẩn đoán nhầm nguyên nhân.

Khi đã lưu vào `ErrorCode`, giá trị đó vẫn giữ nguyên cho đến khi chương trình chủ động thay đổi biến.

## 4. Nguyên tắc cần nhớ

**API thất bại → lấy mã lỗi ngay → lưu mã lỗi → xử lý và báo cáo.**

> Lưu ý: Không phải mọi Windows API đều dùng `GetLastError()` để báo lỗi. Cần kiểm tra tài liệu của từng API để biết cách lấy lỗi chính xác.