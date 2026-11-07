# Strcpy là gì
- `strcpy` là từ viết tắt của **string copy** (Sao chép chuỗi).
- Là một hàm có sẵn trong thư viện chuẩn của C và C++ (`<cstring>`).
# Dùng để làm gì và dùng thế nào
Công việc chính của `Strcpy` là lấy nguyên một chuỗi ký tự từ nguồn này (**source**) chép sang một vùng nhớ đích (**destination**).
## Ví dụ
Chúng ta có một mảng `char buffer[10];` và một chuỗi `"Xinchao"`.
**Cách dùng:**
```Cpp
strcpy(buffer, "XinChao");
```
`strcpy` sẽ lần lượt copy từng ký tự `X`, `i`, `n`, `C`, `h`, `a`, `o` và tự gắn thêm ký tự kết thúc `\0` vào cuối buffer.
# Điểm nguy hiểm của strcpy
- `strcpy` hoàn toàn không biết về kích thước.
- `strcpy` chỉ biết copy từ chuỗi nguồn cho đến khi gặp `\0` thì mới dừng, mà `strcpy` hoàn toàn không quan tâm đến mảng đích có đủ chỗ chứa hay không.
- Nếu chuỗi nguồn dài tận 50 kí tự, mà `buffer` chỉ chứa được 8 ký tự, `strcpy` vẫn in 50 ký tự đó vào, gây ra tràn bộ nhớ **(Buffer Overflow)**.
# Vài kiến thức nhỏ
Vì lý do trên nên trong lập trình an toàn (Secure Coding), người ta thường không ưa chuộng sử dụng `strcpy` (Nhưng cũng không đồng nghĩa **không dùng**) mà thay bằng các hàm có giới hạn kích thước rõ ràng (như `strncpy` hay `snprintf`).
**Ví dụ:** `char buffer[10];` tuy mảng có 10 phần tử nhưng thật ra chỉ có thể chứa 9 ký tự nhập từ nguồn. Lý do vì ký tự cuối cùng thường sẽ là ký tự kết thúc chuỗi `\0`.