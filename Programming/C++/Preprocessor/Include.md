# `#include "engine.hpp"`
## Mục đích
Đưa nội dung của file header vào file đang biên dịch để sử dụng những khai báo mà header cung cấp.
## Khi nào dùng?
Khi file `.cpp` cần biết khai báo hàm, lớp, kiểu dữ liệu hoặc hằng số được cung cấp trong header.
## Phân biệt
- `.hpp`: thường chứa khai báo.
- `.cpp`: thường chứa phần định nghĩa và xử lý thực tế.
Đây là quy ước phổ biến, không phải luật bắt buộc.
## Ví dụ
Trong `engine.hpp` có khai báo:
```C++
void CollectSystemInfo();
```
Khi `engine.cpp` có dòng:
```C++
#include "engine.hpp"
```
compiler sẽ thấy khai báo của `CollectSystemInfo()`, biết hàm có kiểu trả về `void` và không nhận tham số.
Lưu ý: `#include` không trực tiếp chạy hàm và cũng không tự tạo ra phần định nghĩa của hàm. Nó đưa nội dung header vào đơn vị biên dịch.