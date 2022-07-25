# OS COMMAND INJECTION

# I****. OS Command Injection là gì?****

## ****1.1. Khái Niệm****

****OS Command Injection**** (còn được gọi là shell injection) là một lỗ hổng bảo mật web cho phép kẻ tấn công thực thi các lệnh của hệ điều hành (OS) tùy ý trên máy chủ đang chạy một ứng dụng và thường xâm phạm hoàn toàn ứng dụng và tất cả dữ liệu của nó.

## 1.2. ****OS Command Injection có thể xuất hiện ở đâu?****

Hầu như là tất cả các function mà có thực hiện 1 hàm execute shell ở dưới back-end nếu không được lọc đầu vào kĩ càng, kẻ tấn công sẽ có thể dễ dàng bypass qua và thực hiện những ý đồ đen tối.

## ****1.3 Nguyên nhân gây ra OS Command Injection****

Có thể là vô tình hoặc cố ý. Lập trình viên không lường trước được đầu vào mà người dùng sẽ nhập vào mà không lọc ra các chuỗi an toàn bằng whitelist mà cứ thế thực thi bất cứ những gì người dùng nhập vào.

# II****. Cách Inject OS command****

- Các dấu phân tách lệnh hoạt động trên cả hệ thống Windows và Unix:
    - &
    - &&
    - |
    - ||
- Các dấu phân tách lệnh sau chỉ hoạt động trên các hệ thống dựa trên Unix:
    - ;
    - Newline (0x0a or \n)
    - `
    - $

# III. Demo

- Form có các element là input (dùng để nhập tên idol) và 1 element là button (ấn vào để submit và search).

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled.png)

- Ví dụ mình cần load tất cả ảnh của idol theo tên khi đã nhập vào input và ấn search sẽ hiện ra tất cả hình ảnh.
- Cây thư mục như sau:

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%201.png)

- Function load ảnh bị dính lỗi OS Command Injection khi sử dụng hàm thực thi bằng shell hệ thống.

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%202.png)

- Đoạn code trên sẽ thực hiện việc search thông qua tên folder bên trong imgs, nếu trùng tên sẽ thực hiện lệnh ls và tất cả giá trị trả về sẽ xem như là 1 mảng và echo ra từng ảnh.

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%203.png)

- Nhưng mà nếu chúng ta thực hiện 1 đoạn lệnh pipeline được nối vào thì sẽ dính OS Command Injection ngay, có thể thực hiện được bất cứ đoạn lệnh nào thông qua đây.

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%204.png)

# IV, Cách phòng chống

- **escapepeshellarg()** thêm các dấu ngoặc kép xung quanh một chuỗi và dấu ngoặc kép / thoát khỏi bất kỳ dấu ngoặc kép hiện có nào cho phép bạn truyền trực tiếp một chuỗi đến một hàm shell và coi nó như một đối số an toàn duy nhất. Hàm này nên được sử dụng để thoát các đối số riêng lẻ đến các hàm shell đến từ đầu vào của người dùng. Các hàm shell bao gồm execute (), system () và toán tử backtick.
- Trên Windows, **escapepeshellarg()** thay thế các dấu phần trăm, dấu chấm than (thay thế biến bị trì hoãn) và dấu ngoặc kép bằng dấu cách và thêm dấu ngoặc kép xung quanh chuỗi. Hơn nữa, mỗi chuỗi dấu gạch chéo ngược liên tiếp (\) được thoát bằng một dấu gạch chéo ngược bổ sung.
- [***https://www.php.net/manual/en/function.escapeshellarg.php***](https://www.php.net/manual/en/function.escapeshellarg.php)

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%205.png)

- Kết quả sau khi sử dụng hàm **escapeshellarg()**.

![Untitled](OS%20COMMAND%20INJECTION%2000a02500335f45ed8795162ddcee8626/Untitled%206.png)