## Trình Soạn Thảo VI

1. [Giới thiệu](#Gioithieu)
2. [Tạo file, mở file](#taofile,mofile)
3. [Chỉnh sửa file](#chinhsuafile)
4. [Di chuyển trong file](#Dichuyen)
5. [Xóa ký tự](#Xoa)
6. [Sao chép, dán, di chuyển](#Saochep,dan)
7. [Lưu và thoát khỏi vi](#Lưu,thoat)
8. [Các lệnh nâng cao](#Lenhnangcao)
9. [Lệnh set](#Lenhset)
<a name="Gioithieu"></a>
### 1. Giới thiệu

**Vi** có 2 chế độ:

- **Command mode**: Thực hiện các nhiệm vụ như lưu file, chạy lệnh, di chuyển con trỏ, cắt và dán các dòng hoặc từ, và tìm kiếm và đổi vị trí. Trong chế độ này, bất cứ cái gì bạn gõ vào được hệ thống hiểu như một lệnh.
- **Insert mode**: Chế độ này cho bạn có thể chèn văn bản vào trong file.

Ấn phím lệnh **i** hoặc **a** từ chế độ **command mode** để chuyển sang **insert mode**. 

Ấn **Esc** để chuyển từ **insert mode** sang **command mode** 

<a name="taofile,mofile"></a>
### 2. Tạo file, mở file

- Lệnh **vi filename**: Tạo một file mới nếu nó đã không tồn tại, nếu không thì mở một file đang tồn tại.
- Lệnh **vi -R filename**: Mở một file đang tồn tại trong chế độ chỉ đọc.
- Lệnh **view filename**: Mở một file đang tồn tại trong chế độ chỉ đọc.

<a name="chinhsuafile"></a>
### 3. Chỉnh sửa file 
Để chỉnh sửa file, bạn cần trong chế độ chèn. Có nhiều cách để vào chế độ chèn từ chế độ lệnh.

|Lệnh|Mô tả|
|----|----|
|i|Chèn văn bản trước vị trí con trỏ hiện tại.|
|I|Chèn văn bản ở phần đầu dòng hiện tại.|
|a|Chèn văn bản sau vị trí con trỏ hiện tại.|
|A|Chèn văn bản tại phần cuối của dòng hiện tại.|
|o|Tạo một dòng mới để nhập văn bản dưới vị trí con trỏ hiện tại.|
|O|Tạo một dòng mới để nhập văn bản trên vị trí con trỏ hiện tại.|

<a name="Dichuyen"></a>
### 4. Di chuyển trong một file 

|Lệnh|Miêu tả|
|----|----|
|k|Di chuyển con trỏ lên trên một dòng.|
|j|Di chuyển con trỏ xuống một dòng.|
|h|Di chuyển con trỏ sang trái một ký tự.|
|l|Di chuyển con trỏ sang phải một ký tự.|
|0 or \||Đặt vị trí con trỏ tại đầu dòng.|
|$|Đặt vị trí con trở ở cuối dòng.|
|:0|Đặt vị trí con trỏ ở dòng đầu tiên của file|
|:$|Đặt vị trí con trỏ ở dòng cuối cùng của file|
|w|	Đặt vị trí con trỏ ở từ tiếp theo.|
|b|	Đặt vị trí con trỏ ở từ trước.|
|(|	Đặt vị trí con trỏ ở đầu câu văn hiện tại.|
|)|	Đặt vị trí con trỏ ở đầu câu văn kế tiếp.|
|E|Di chuyển tới phần cuối của từ được giới hạn khoảng trắng.|
|n\||Di chuyển tới cột n trong dòng hiện tại.|
|1G|Di chuyển tới dòng đầu tiên của file.|
|G|Di chuyển tới dòng cuối cùng của file.|
|nG|Di chuyển tới dòng thứ n của file.|
|:n|Di chuyển tới dòng thứ n của file.|
|H|Di chuyển tới đầu của màn hình.|
|nH|Di chuyển tới dòng thứ n tính từ đầu của màn hình.|
|M|Di chuyển tới giữa màn hình.|
|L|Di chuyển tới cuối cùng màn hình.|
|nL|Di chuyển tới dòng thứ n tính từ cuối cùng của màn hình.|
|:x|Dấu hai chấm theo sau bởi một số sẽ đặt vị trí của con trỏ| trên dòng số x|

<a name="Xoa"></a>
### 5. Xóa ký tự

|Lệnh|Miêu tả|
|----|----|
|x|Xóa một ký tự dưới vị trí con trỏ hiện tại.|
|X|Xóa một ký tự trước vị trí con trỏ hiện tại.|
|dw|Xóa 1 từ|
|d^|Xóa từ vị trí con trỏ hiện tại tới phần bắt đầu của dòng.|
|d$|Xóa từ vị trí con trỏ hiện tại tới phần cuối của dòng.|
|D|Xóa từ vị trí con trỏ hiện tại tới phần cuối của dòng hiện tại.|
|dd|Xóa cả dòng mà con trỏ hiện tại đang đứng|
|:1,8d|Xóa từ dòng 1 cho đến dòng 8 trong file.|
|:10,$d|Xóa từ dóng 10 đến dòng cuối cùng của file|

<a name="Saochep,dan"></a>
### 6. Các lệnh sao chép, dán, di chuyển 

|Lệnh|Mô tả|
|----|----|
|yy|Sao chép dòng hiện tại.|
|Nyy|Sao chép N dòng hiện tại.|
|p|Dán bản sao sau vị trí con trỏ.|
|P|Dán bản sao trước vị trí con trỏ.|
|:8,16 co 32|Copy dòng 8 đến 16 đến điểm sau dòng 32.|
|:3,16 m 32|Di chuyển dòng 8->16 đến điểm sau dòng 32.|

<a name="Luu,thoat"></a>
### 7. Lưu và Thoát khỏi vi

|Lệnh|Mô tả|
|----|----|
|:q|Thoát khỏi vi|
|:q!|Lệnh để thoát khỏi vi mà không lưu các chỉnh sửa|
|:w|Lưu và không thoát|
|:wq hoặc ZZ|Lưu và thoát|
|:w newfile.txt|save nội dung của file hiện tại vào một file mới là newfile.txt tương tựa “save as” bên Win Word|

<a name="Lenhnangcao"></a>
### 8. Các lệnh nâng cao

|Lệnh|Miêu tả|
|----|----|
|J|Nhập dòng hiện tại với dòng kế tiếp.|
|<<|Di chuyển dòng hiện tại sang trái một độ rộng shift.|
|>>|Di chuyển dòng hiện tại sang phải một độ rộng shift.|
|~|Chuyển kiểu của ký tự dưới vị trí con trỏ (VD: chữ hoa thành chữ thường ).|
|^G|Nhấn Ctrl+G cùng một lúc để chỉ trạng thái và tên file hiện tại.|
|U|Hồi phục dòng hiện tại trở về trạng thái trước khi con trỏ vào dòng đó.|
|u|Undo các thay đổi vừa thực hiện với file. Nhập u lần nữa sẽ redo sự thay đổi.|
|:f|Hiển thị vị trí hiện tại của file trong % và tên file, tổng số file.|
|:f filename|Đặt lại tên hiện tại của file.|
|:e filename|Mở một file khác với tên của nó.|
|:cd dirname|Thay đổi thư mục làm việc hiện tại tới thư mục dirname.|
|:e #|Sử dụng để chuyển đổi giữa hai file đã mở.|
|:n|Trong trường hợp bạn mở nhiều file sử dụng vi, bạn sử dụng :n để tới file kế tiếp trong seri các file đó.|
|:p|Trong trường hợp bạn mở nhiều file sử dụng vi, bạn sử dụng :p để tới file phía trước trong seri file đó.|
|:N|Trong trường hợp bạn mở nhiều file sử dụng vi, bạn sử dụng :N để tới file phía trước trong seri file đó.|
|:r file|Đọc file và chèn nó sau dòng hiện tại.|
|:nr file|Đọc file và chèn nó sau dòng thứ n.|

<a name="Lenhset"></a>
### 9. Các lệnh set 

|Lệnh|Miêu tả|
|----|----|
|:set ic|Bỏ qua kiểu chữ (chữ hoa, thường) khi tìm kiếm.|
|:set ai|Thiết lập chế độ thụt vào đầu dòng tự động.|
|:set noai|Để không đặt chế độ thụt vào đầu dòng tự động.|
|:set nu|Đánh số dòng|
|:set nonu|Hủy đáng số dòng|
|:set ro|Thay đổi kiểu file thành "chỉ đọc (read only)"|

- **Tìm kiếm trong vi**
Lệnh **/”ký tự cần tìm”** 

Ấn n để đến từ  tiếp theo, N để tìm kiếm ngược trở lại đầu văn bản trước con trỏ.

Ví dụ: Tìm kiếm từ "Linux". Gõ lệnh **/Linux**

- **Thay thế ký tự**

Lệnh  **:%s/chữ cần thay thế/ chữ được thay thế/g**
Ví dụ: **:%s/Windows/Linux/g**
Chữ **Windows** sẽ được thay thế bằng chữ **linux**

- Sử dụng dấu **.** để lặp lại hành động vừa làm

Ví dụ người dùng gõ i để insert dòng chữ “hello world”, sau đó chuyển sang chế độ command mode bằng phím **Esc**, nhảy xuống dòng và gõ . , dòng chữ “hello world” sẽ hiện ra

- Nếu dùng vi để mở nhiều file ( vi file1 file2 file3 ), có thể sử dụng **:n** để nhảy đến file tiếp theo và **:rew** để nhảy quay ngược lại đến file đầu tiên, **:args** để hiện thị tất cả các file đang được mở.

- Sử dụng số **N** đi trước phím lệnh để lặp lại **N** lần tác dụng của lệnh. Ví dụ, 3dw sẽ xóa 3 từ tính từ vị trí con trỏ.


### Tài liệu thao thảo: https://vietjack.com/unix/trinh_soan_thao_vi_trong_unix_linux.jsp

 
