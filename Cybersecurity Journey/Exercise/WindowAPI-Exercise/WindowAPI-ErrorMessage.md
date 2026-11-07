# `MessageLength - 1`
```Cpp
ErrorMessage[MessageLength - 1] = '\0';
```
## 1. Vì sao lại dùng `MessageLength - 1`?
### Bản chất
Trước hết, cần hiểu cách hoạt động của `FormatMessageA()`.
Khi hàm `FormatMessageA()` thành công, nó ghi thông báo vào buffer và kết thúc chuỗi bằng ký tự null (`'\0'`).
Giá trị trả về của hàm được lưu trong `MessageLength` là số ký tự đã được ghi vào buffer, **không tính ký tự null kết thúc chuỗi**.
Ví dụ, giả sử buffer nhận được nội dung:
```
Error!\r\n\0
```
Trong đó:
- `Error!` là nội dung thông báo.
- `\r\n` là cặp ký tự xuống dòng.
- `\0` là ký tự kết thúc chuỗi C.
Nếu chuỗi có 9 ký tự trước `\0`, thì `MessageLength` sẽ bằng 9.
Lưu ý: Đây là ví dụ minh họa. Nội dung và độ dài thực tế của thông báo phụ thuộc vào thông báo được trả về.
### Ý nghĩa
Thông báo lỗi do Windows cung cấp thường có thể chứa ký tự xuống dòng ở cuối, chẳng hạn `\r\n`.
Nếu in thông báo bằng `std::cout` rồi tiếp tục in một dòng mới bằng `'\n'`, đầu ra có thể xuất hiện một dòng trống không mong muốn.
Vì vậy, người lập trình có thể loại bỏ ký tự xuống dòng ở cuối chuỗi bằng cách thay nó bằng `'\0'`.
Tuy nhiên, câu lệnh:
```Cpp
ErrorMessage[MessageLength - 1] = '\0';
```
chỉ thay thế **một ký tự cuối cùng** bằng ký tự null. Nó không tự kiểm tra ký tự đó có phải `'\n'` hay không.
Nếu ký tự cuối cùng là `'\n'`, câu lệnh sẽ loại bỏ ký tự đó khỏi chuỗi.
Nếu thông báo kết thúc bằng `"\r\n"`, thao tác này chỉ loại bỏ `'\n'`, còn `'\r'` vẫn tồn tại.
Do đó, không nên mặc định rằng thay thế ký tự cuối cùng luôn loại bỏ hoàn toàn phần xuống dòng.
## 2. Nếu bỏ việc gán `'\0'`, chương trình có in sai thông báo không?
### Câu trả lời
Thông thường, chương trình vẫn có thể in thông báo bình thường.
### Lý do
Bản thân `FormatMessageA()` đã thêm ký tự null kết thúc chuỗi khi thành công. Vì vậy, không cần tự thêm một ký tự null nữa chỉ để `std::cout` có thể in chuỗi.
Ví dụ:
```Cpp
char ErrorMessage[128] = {};

DWORD MessageLength = FormatMessageA(
    FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS,
    nullptr,
    ErrorCode,
    0,
    ErrorMessage,
    sizeof(ErrorMessage),
    nullptr
);

if (MessageLength > 0)
{
    std::cout << ErrorMessage;
}
```
Nếu hàm thành công, `ErrorMessage` đã là một chuỗi C kết thúc bằng `'\0'`.
Việc không thực hiện:
```Cpp
ErrorMessage[MessageLength - 1] = '\0';
```
không làm mất khả năng kết thúc chuỗi.
Tuy nhiên, nếu thông báo có sẵn ký tự xuống dòng ở cuối, ký tự đó vẫn được giữ nguyên và có thể ảnh hưởng đến cách hiển thị.
**Phân biệt hai mục đích:**
- Ký tự null do `FormatMessageA()` ghi vào buffer dùng để kết thúc chuỗi.
- Câu lệnh gán `ErrorMessage[MessageLength - 1] = '\0';` dùng để loại bỏ ký tự cuối khỏi nội dung chuỗi, nếu được thực hiện đúng cách.
## 3. Câu lệnh `ErrorMessage[MessageLength - 1] = '\0';` có an toàn trong mọi trường hợp không?
### Câu trả lời
Không. Cần kiểm tra giá trị trả về và xác định ký tự cần loại bỏ trước khi thực hiện phép gán.
Có hai vấn đề cần phân biệt:
1. **An toàn bộ nhớ:** Có truy cập hoặc ghi ra ngoài phạm vi buffer hay không?
2. **Tính đúng đắn của logic:** Có vô tình xóa một ký tự không nên xóa hay không?
Một câu lệnh có thể không gây truy cập ngoài phạm vi buffer nhưng vẫn xử lý sai dữ liệu.
### 3.1. Kiểm tra `MessageLength`
Trước khi truy cập:
```Cpp
ErrorMessage[MessageLength - 1] = '\0';
```
cần kiểm tra:
```
MessageLength > 0
```
Lý do là `MessageLength` có kiểu `DWORD`, một kiểu số nguyên 32-bit không dấu trên Windows.
Nếu `MessageLength == 0`, phép tính:
```
MessageLength - 1
```
không tạo ra số nguyên âm như khi tính toán bằng một kiểu số nguyên có dấu.
Thay vào đó, phép trừ số nguyên không dấu sẽ quay vòng theo quy tắc modulo của kiểu dữ liệu. Với `DWORD` 32-bit, kết quả sẽ là:
```
0 - 1 = 4294967295
```
Đây là hiện tượng **unsigned integer wraparound**.
Khi đó, biểu thức:
```
ErrorMessage[MessageLength - 1] = '\0';
```
có thể cố gắng ghi vào một vị trí nằm ngoài phạm vi mảng `ErrorMessage`.
Đây là một **out-of-bounds write** và dẫn đến **undefined behavior** trong C++.
Hậu quả có thể là:
- Chương trình bị crash hoặc gặp access violation.
- Dữ liệu ở vùng nhớ khác bị thay đổi.
- Chương trình biểu hiện bất thường theo cách không thể dự đoán đáng tin cậy.
Không có gì đảm bảo rằng chương trình sẽ crash ngay lập tức.
Vì vậy, cần kiểm tra `MessageLength > 0` trước khi trừ 1.
### 3.2. Kiểm tra kích thước buffer
Xét khai báo:
```
char ErrorMessage[128] = {};
```
Mảng có 128 phần tử kiểu `char`. Trên Windows, mỗi `char` chiếm 1 byte, nên mảng có kích thước 128 byte.
Khi gọi:
```Cpp
FormatMessageA(
    FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS,
    nullptr,
    ErrorCode,
    0,
    ErrorMessage,
    sizeof(ErrorMessage),
    nullptr
);
```
tham số `sizeof(ErrorMessage)` cung cấp kích thước buffer là 128 ký tự.
Đối với `FormatMessageA()`, tham số kích thước buffer được tính theo số ký tự, không phải số bit.
Vì mỗi `char` chiếm 1 byte nên trong trường hợp này, giá trị 128 tương ứng với 128 ký tự.
Nếu `FormatMessageA()` thành công theo hợp đồng của hàm, số ký tự được trả về phải phù hợp với dung lượng buffer và chừa chỗ cho ký tự null kết thúc chuỗi.
Tuy nhiên, nếu hàm trả về 0 thì đó là dấu hiệu thất bại. Không được sử dụng giá trị trả về này để truy cập `ErrorMessage[MessageLength - 1]`.
Một điều quan trọng khác: **kiểm tra kích thước buffer không thay thế cho việc kiểm tra ký tự cần loại bỏ**. Buffer có thể hoàn toàn hợp lệ nhưng thao tác vẫn xóa nhầm dữ liệu.
### 3.3. Khắc phục
Nếu chỉ muốn đảm bảo rằng phép truy cập cuối chuỗi không được thực hiện khi `MessageLength == 0`, có thể viết:
```Cpp
if (MessageLength > 0)
{
    ErrorMessage[MessageLength - 1] = '\0';
}
```
Điều kiện này ngăn việc trừ 1 khi độ dài bằng 0.
Tuy nhiên, nó **chưa đảm bảo rằng ký tự cuối cùng thực sự là ký tự xuống dòng**.
Nếu mục tiêu là loại bỏ phần xuống dòng ở cuối thông báo, nên kiểm tra nội dung trước khi thay thế.
Ví dụ:
```Cpp
if (MessageLength > 0)
{
    if (ErrorMessage[MessageLength - 1] == '\n')
    {
        ErrorMessage[MessageLength - 1] = '\0';
        --MessageLength;
    }

    if (MessageLength > 0 && ErrorMessage[MessageLength - 1] == '\r')
    {
        ErrorMessage[MessageLength - 1] = '\0';
        --MessageLength;
    }
}
```
Cách xử lý này:
1. Kiểm tra độ dài trước khi truy cập ký tự cuối.
2. Nếu ký tự cuối là `'\n'`, thay thế nó bằng `'\0'` rồi giảm `MessageLength`.
3. Kiểm tra lại độ dài.
4. Nếu ký tự cuối mới là `'\r'`, tiếp tục loại bỏ nó.
Kết quả là thao tác có thể loại bỏ cả `"\r\n"` hoặc chỉ `'\n'`, tùy nội dung thực tế của chuỗi.
Đoạn mã trên giả định rằng `MessageLength` là độ dài hợp lệ nhận được từ một lần gọi `FormatMessageA()` thành công với buffer tương ứng.
## 4. Kết luận
Khi xử lý chuỗi được tạo bởi `FormatMessageA()`, cần ghi nhớ:
- `FormatMessageA()` đã thêm ký tự null kết thúc chuỗi khi thành công.
- Giá trị trả về `MessageLength` không tính ký tự null kết thúc chuỗi.
- `MessageLength == 0` cho biết hàm thất bại; không được thực hiện phép truy cập `MessageLength - 1`.
- Với `DWORD`, phép trừ 1 khi giá trị bằng 0 gây unsigned integer wraparound, không phải tạo ra số nguyên âm theo cách thông thường.
- Truy cập ngoài phạm vi buffer có thể gây undefined behavior.
- Thay thế ký tự cuối bằng `'\0'` không đồng nghĩa với việc chắc chắn loại bỏ ký tự xuống dòng.
- Muốn loại bỏ xuống dòng chính xác, cần kiểm tra ký tự cuối trước khi thay thế.
- Kiểm tra an toàn bộ nhớ và kiểm tra tính đúng đắn của logic là hai việc khác nhau.