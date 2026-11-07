# Strlen là gì
- `Strlen` là viết tắt của **String Length** (Độ dài chuỗi).
- Nó là một hàm có sẵn trong thư viện chuẩn của `cstring`.
# Tác dụng và cách dùng
**Tác dụng:** Dùng để đếm số lượng ký tự có trong chuỗi kiểu C. Cho đến khi gặp ký tự kết thúc thì sẽ dừng.
**Ví dụ:** Nếu có chuỗi `char name[] = "Hello";`, khi gọi `strlen(name)`, hàm sẽ trả về số 5 (vì hàm đếm 5 chữ `H`-`e`-`l`-`l`-`o`, **tuyệt đối không tính** ký tự `\0` ở cuối).
**Lưu ý:** Hàm `strlen` chỉ biết đếm đến khi thấy được ký tự kết thúc `\0`. Nếu một chuỗi bị mất ký tự `\0` do tràn bộ nhớ thì strlen vẫn cứ đếm tiếp ra các vùng nhớ bên ngoài và gây ra lỗi chương trình (Segmentation Fault).