# Khởi tạo mảng bằng `= {}`
## Dùng để làm gì?
Khởi tạo tất cả phần tử của mảng về giá trị 0 khi khai báo.
## Cách hoạt động
- `char name[5] = {};` tạo mảng 5 phần tử kiểu `char`, tất cả ban đầu bằng `0`.
- Với `char`, giá trị 0 tương ứng với ký tự null `'\0'`.
- `char name[5];` là mảng cục bộ chưa được khởi tạo giá trị; không nên đọc nội dung trước khi ghi dữ liệu hợp lệ vào.
- `= {}` không làm tăng kích thước mảng.
# `MAX_COMPUTERNAME_LENGTH`
## Là gì?
Macro do Windows API định nghĩa, cung cấp giới hạn độ dài tên máy tính theo Windows API.
## Cách sử dụng
- Cần include `windows.h` để sử dụng macro này trong code Windows thông thường.
- `MAX_COMPUTERNAME_LENGTH + 1` dành thêm một ô cho ký tự kết thúc chuỗi.
# Dùng để làm gì?
Dùng để lưu nhiều phần tử cùng kiểu dữ liệu.
# Ý nghĩa
- `char name[5];` tạo mảng có 5 phần tử kiểu `char`.
# Cách hoạt động
- Chỉ số bắt đầu từ `0`.
- Mảng 5 phần tử có chỉ số hợp lệ từ `0` đến `4`.