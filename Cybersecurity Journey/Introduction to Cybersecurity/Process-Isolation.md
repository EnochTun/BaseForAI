# Ý nghĩa Process Isolation
**Process Isolation** là cơ chế cô lập các tiến trình, giúp giới hạn khả năng của một process truy cập vào tài nguyên của một process khác.
Mỗi một process thường có không gian địa chỉ ảo (Virtual Address Space) riêng.
**Ví dụ:**

| Process A                | Process B                |
| ------------------------ | ------------------------ |
| Virtual address space A  | Virtual address space B  |
| Memory riêng             | Memory riêng             |
| Không tự do đọc memory B | Không tự do đọc memory A |

Điểm quan trọng: Hai process có thể sử dụng cùng một địa chỉ ảo, nhưng địa chỉ đó có thể ánh xạ đến những vùng RAM vật lý khác nhau.
- **Virtual Address:** Địa chỉ mà process nhìn thấy.
- **Physical Address:** Địa chỉ trong bộ nhớ vật lý.
- **Memory Mapping:** Quá trình ánh xạ địa chỉ ảo sang địa chỉ vật lý.
- **MMU (Memory Management Unit):** Thành phần phần cứng hỗ trợ dịch địa chỉ và kiểm tra quyền truy cập bộ nhớ.
## Vì sao dùng "được ánh xạ"?
Bởi vì CPU thông qua MMU (Memory Management Unit) thực hiện quá trình dịch địa chỉ ảo sang địa chỉ vật lý dựa trên các cấu trúc quản lý bộ nhớ của hệ điều hành.

## Ý nghĩa về Bảo mật
Nếu không có cơ chế cách ly phù hợp, một process có thể gây ảnh hưởng đến dữ liệu của một process khác.
Process Isolation giúp hạn chế:
- Truy cập bộ nhớ trái phép.
- Hạn chế việc process này đọc hoặc sửa trái phép bộ nhớ của process khác.
- Can hiệp trái phép vào process khác.
# Góc nhìn tấn công
Một attacker có thể tìm cách:
- Khai thác lỗ hổng trong process A.
- Thực thi các mã trái phép trong process A.
- Tìm cách lợi dụng quyền truy cập process, cơ chế IPC hoặc khai thác các quyền bảo mật.
Việc kiểm soát được process A không đồng nghĩa với attacker có thể kiểm soát được process B.
# Điều quan trọng
## Process Isolation
Tuy là cô lập các tiến trình nhưng nó không có nghĩa là hai process Isolation tuyệt đối không thể giao tiếp hay truy cập dữ liệu của nhau.
## Khai báo biến cùng tên
Nếu process A và process B khai báo biến cùng tên thì cả 2 process không đọc được giá trị của biến. Tên biến giống nhau không tạo ra quyền truy cập bộ nhớ của 2 process.