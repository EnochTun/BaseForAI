# Arrays And Buffer
## 1. Mảng `char` là gì?
Mảng là một tập hợp các phần tử cùng kiểu dữ liệu được lưu liên tiếp trong bộ nhớ.
Ví dụ:

```
char ErrorMessage[512] = {};
```

- `char`: kiểu dữ liệu của mỗi phần tử.
    
- `ErrorMessage`: tên mảng.
    
- `[512]`: mảng có 512 phần tử, được đánh chỉ số từ `0` đến `511`.
    
- `= {}`: khởi tạo tất cả phần tử về giá trị `0`.
    

Với `char`, giá trị `0` tương ứng với ký tự null `'\0'`.

## 2. Tại sao cần bộ đệm?

`ErrorMessage` được dùng làm vùng nhớ để chứa thông báo lỗi dạng chuỗi ký tự.

Một thông báo có thể gồm nhiều ký tự, nên cần một vùng nhớ đủ lớn để chứa nội dung đó.

Trong ví dụ này, 512 phần tử `char` thường chiếm tổng cộng 512 byte.

Kích thước 512 byte không đảm bảo mọi thông báo đều vừa với bộ đệm. Chương trình vẫn cần kiểm tra kết quả của API ghi dữ liệu vào vùng nhớ này.

## 3. Ý nghĩa của `= {}`

```
char ErrorMessage[512] = {};
```

Cách khởi tạo này đưa tất cả phần tử của mảng về giá trị `0`.

Đối với mảng `char`, điều đó có nghĩa là mọi phần tử ban đầu đều chứa `'\0'`.

## 4. Nếu bỏ `= {}` thì sao?

```
char ErrorMessage[512];
```

Nếu đây là biến cục bộ thông thường trong một hàm, các phần tử không được tự động khởi tạo về `0`. Giá trị của chúng là không xác định.

Không nên coi các giá trị đó là dữ liệu ngẫu nhiên có thể sử dụng được.

## 5. Ký tự kết thúc chuỗi

Trong chuỗi C kiểu `char`, `'\0'` đánh dấu điểm kết thúc chuỗi.

Khi toàn bộ mảng được khởi tạo về `0`, phần tử đầu tiên đã là `'\0'`, nên nếu đọc nó như chuỗi C thì chuỗi ban đầu là chuỗi rỗng.

Sau khi `FormatMessageA()` ghi thông báo vào bộ đệm, nội dung được ghi sẽ thay thế các phần tử đầu mảng và API tự thêm ký tự null kết thúc khi thành công theo cách sử dụng hiện tại.