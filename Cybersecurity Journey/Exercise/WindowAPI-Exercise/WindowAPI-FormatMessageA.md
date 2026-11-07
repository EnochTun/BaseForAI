# FormatMessageA()
## Code
```Cpp
DWORD MessageLength = FormatMessageA(
	FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS,
	nullptr,
	ErrorCode,
	0,
	ErrorMessage,
	sizeof(ErrorMessage),
	nullptr
);
```
## Nếu bỏ `FORMAT_MESSAGE_IGNORE_INSERTS` thì điều gì xảy ra?
### Cơ chế của `FORMAT_MESSAGE_IGNORE_INSERTS`
Trong Windows API, một chuỗi thông báo lỗi hệ thống có thể tồn tại ở 2 dạng:
1. **Chuỗi tĩnh (Static string):** Chỉ chứa các câu chữ cố định.
   Ví dụ: "The system cannot find the file specified."
2. **Chuỗi động (Dynamic/Format string):** Chứa các "chỗ trống" (gọi là **Insert sequences** hoặc **Placeholders**, có dạng `%1`, `%2`, `%3`...). Các chỗ trống này được thiết kế để nhét dữ liệu động vào (như tên file, tên thư mục, mã số tiến trình...).
   Ví dụ: "The file %1 cannot be opened because %2."
**Lưu ý:** Đây là cách phân loại chuỗi để dễ hiểu `Insert sequences`, không khẳng định Window API chính thức định nghĩa mọi thông báo theo 2 dạng trên.
#### Cách `FORMAT_MESSAGE_IGNORE_INSERTS` xử lý khi có và không có
- **Khi có:** Chúng ta ra lệnh Windows giữ nguyên các Insert sequences (`%1`, `%2`, ...) mà không cố gắng lấy từ tham số cuối cùng (`Arguments`). Kiểu như: "Tôi chỉ muốn lấy chuỗi nguyên bản. Nếu trong câu có %1, %2, ... thì kệ nó, đừng tìm dữ liệu để điền, cứ giữ nguyên nó."
- **Khi không có(Cơ chế mặc định):** Window sẽ sử dụng dữ liệu được chương trình cung cấp (nó không đồng nghĩa với chương trình luôn cung cấp đủ dữ liệu), điền dữ liệu vào các Insert sequences (`%1`, `%2`, ...) được lấy từ tham số cuối cùng (`Arguments`). Kiểu như: "Cậu muốn tôi **điền dữ liệu thực tế** vào các chỗ trống `%1`, `%2`? Vậy tôi sẽ lấy dữ liệu từ tham số cuối cùng (`Arguments`) ra để chép vào đó nhé!"
- **Điều kiện quan trọng:** `FormatMessageA()` xử lý các Insert sequences bằng dữ liệu được cung cấp thông qua `Arguments`. Nếu không có dữ liệu đối số hợp lệ mà message lại yêu cầu chúng, việc xử lý có thể dẫn đến truy cập bộ nhớ không hợp lệ.