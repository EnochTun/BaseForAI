# Null Character (`'\0'`)
## Dùng để làm gì?
Dùng để đánh dấu điểm kết thúc của một chuỗi C.
## Bản chất
- `'\0'` là ký tự có giá trị bằng `0`.
- Nó được gọi là null character.
- Nó không giống ký tự `'0'`, vốn là ký tự số 0 hiển thị được.
## Cách hoạt động
- `'\0'` là ký tự có giá trị số bằng `0`.
- Chuỗi C lưu các ký tự liên tiếp trong mảng `char`.
- Khi đọc chuỗi theo quy ước C, chương trình dừng tại ký tự `'\0'`.
- Một chuỗi có `n` ký tự thông thường cần ít nhất `n + 1` ô nhớ nếu lưu cả ký tự kết thúc.
## Ví dụ
```
char name[7] = "LENOVO";
```
Mảng có 7 ô: 6 ô chứa các chữ cái của `LENOVO` và 1 ô chứa `'\0'` do trình biên dịch tự thêm khi khởi tạo bằng chuỗi literal này.