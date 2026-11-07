```Cpp
#include <iostream>
#include <cstring>

void ProcessInput(const char* userInput) {
    char buffer[8]; // Bộ đệm cực kỳ nhỏ, chỉ 8 bytes!

    // BẪY: strcpy chép không kiểm soát
    strcpy(buffer, userInput); 
}
```
# Vọc vạch đoạn code trên
Với đoạn code trên, có một mảng chỉ chứa có 8 phần tử `char buffer[8];` và sử dụng `strcpy` một cách tự do mà không có kiểm tra nghiêm ngặt, điều này thường xuyên gây ra lỗi tràn bộ nhớ nếu người dùng hoặc kẻ tấn công nhập quá mức ký tự mà mảng có thể chứa. Ở đây, bộ đệm chỉ có thể chứa **7 ký tự**. Vì sao chỉ có thể chứa **7 ký tự** trong khi mảng có **8 phần tử**?
Vì khi dùng các `strcpy`, `strncpy` hay `snprintf` thì ở cuối các chuỗi ký tự (Em cần sự trợ giúp giải thích từ ngữ của chị Minji chỗ nàyT-T) thì `strcpy`, `strncpy` hay `snprintf` sẽ tự thêm ký tự kết thúc `\0` vào cuối câu.
# Khắc phục lỗi tràn bộ nhớ
Đối với đoạn code trên, chúng ta có nhiều hướng để khắc phục lỗi tràn bộ nhớ.
## lệnh kiểm tra if
Với hướng đầu tiên chúng ta sử dụng if để kiểm tra, đây là cách tốt nhất cho việc "Không bỏ khung code cũ"
```Cpp
#include <iostream>
#include <cstring>

void SafeProcessInput(const char* userInput) {
    char buffer[8]; // Sức chứa tối đa: 8 bytes

    // BƯỚC CHECK CHUẨN:
    // Nếu độ dài chuỗi đầu vào >= kích thước của buffer, HOẶC dùng snprintf
    // Ở đây ta check thủ công bằng if:
    if (strlen(userInput) >= sizeof(buffer)) {
        std::cout << "[!] Canh bao: Du lieu qua dai, tu choi xu ly de tranh tran bo nho!\n";
        return; // Dừng hàm ngay lập tức!
    }

    // Nếu qua được cửa kiểm tra này, dùng strcpy lúc này mới an toàn tuyệt đối
    strcpy(buffer, userInput);
    std::cout << "[+] Thanh cong! Buffer dang chua: " << buffer << "\n";
}
```
### Lưu ý
Đây là cách tốt nhất cho **Giữ nguyên khung code** chứ không phải hướng tốt nhất.
### Ghi chú
Định nghĩa strcpy -[[Strcpy]].
Định nghĩa strlen - [[Strlen]].

