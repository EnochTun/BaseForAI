# Code
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
# Ý nghĩa
`FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS,`
Cách lấy và xử lí thông báo.

`nullptr,`
Không chỉ định module nguồn

`ErrorCode,`
Mã thông báo cần tìm.

`0,`
Để Window chọn ngôn ngữ theo quy tắc mặc định.

`ErrorMessage,`
Buffer nhận thông báo.

`sizeof(ErrorMessage),`
Kích thước buffer tính theo ký tự.

`nullptr`
Không cung cấp đối số chèn.