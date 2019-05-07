#### SERVICE
SErviec là một ứng dụng hoặc bộ các ứng dụng chạy trong nền đang chờ sử dụng hoặc thực hiện các tác vụ thiết yếu. 
? Biết các dịch vụ đang chạy
? Thiết lập dịch vụ của riêng mình.

Runlevel
|Level|Generic|Fedora / Core| Slackware|Debian|
|---|---|---|---|---|
|0|Halt|Halt|Halt|Halt|
|1|Single-user mode|Single-user mode|Single-user mode|Single-user mode|
|2|Basic multi-user mode (without networking)|User definable (Unused)
|User definable - configured the same as runlevel 3|Multi-user mode|
|3|Full (text based) multi-user mode|Multi-user mode|Multi-user mode - default Slackware runlevel|...|
|4|Not used|Not used|X11 with KDM/GDM/XDM (session managers)|Multi-user mode|
|5|Full (GUI based) multi-user mode|Full multi-user mode (with an X-based login screen) - default runlevel|User definable - configured the same as runlevel 3| Multi-user mode|
|6|Reboot|Reboot|Reboot|Reboot|

### /etc/services
Hệ điều hành LInux lưu trữ các tệp dịch vụ tại /etc/services. Nó lưu trữ thông tin vầ nhiều dịch vụ mà các ứng dụng khác có teher sử dụng trên máy tính. Trong tệp bao gồm: Tên dịch vụ, số cổng dịch vụ, giao thức mà nó sử dụng.

bạn có thể cấu hình lại số cổng bằng cách chỉnh sửa file /etc/services bằng các trình soạn thảo.


Systemctl đre giám sát và ddeieuf khiển systemd. 
- Liệt kê các unit đã được load `systemctl list-units`
- Hiển thị trạng thái 1 hệ thống `systemctl status`
- Kiểm tra trạng thái service 
`systemctl status service_name`
or
`service service_name status`
- bắt đầu 1 dịch vụ: 
`systemctl start service_name`
- Ngừng chạy 1 dịch vụ:
`systemctl stop service_name`
- Khởi động lại dịch vụ
`systemctl restart service_name`
- Bật service khởi động cùng máy
`systemctl enable service_name`
- Tắt service khởi động cùng máy
`systemctl disable service_name`

https://mangmaytinh.net/threads/understanding-systemd-units-and-unit-files.243/



Unit fiel là cac staapj tin cấu hình xác định cac snguonf tài nguyên.

Systemd unit file được tìm thấy ở đâu. 
Các tập tin xác định cách mà systemd sẽ xử lý một đơn vị có thể được tìm thấy ở nhiều nơi, trong đó có những ưu tiên và mnh]ngx tác động khác nhau.
Bản sao các đơn vị tập tin heeh thống được lưu trữ trong thư mục: /lib/systemd/system. Khi 1 phần mềm được cài đặt trên hệ thống, ccs tập tin sẽ được lưu trữ mặc định tại đây.
Các unit file ở đây có thể được bắt đầu vad dừng theo yêu cầu trong 1 phiên.

Các loại unit
Các unit của Systemd dựa trên laoij tài nguyên mà chúng mô tả. 
+ .service: Đơn vị dochj vụ mô tả làm thế nào đẻ qune ký các dịch vụ và ứng dụng trên máy chủ
+ .socket: Một tập tin đơn vị socket mô tả một mạng hoặc socket IPC, hoặc một bộ đệm FIFO rằng systemd sử dụng để kích hoạt socket. Lúc nào cũng có một file .service liên quan sẽ được bắt đầu khi hoạt động được nhìn thấy trên các socket mà đơn vị này xác định.
+ .device: Đơn vị này xác định điểm gắn kết trên hệ thống được quản lý bởi systemd.
+ .automount: Một đơn vị .automount cấu hình điểm gắn kết sẽ được tự động gắn kết.
+ .target: Một đơn vị target được sử dụng để cung cấp các điểm đồng bộ cho các đơn vị khác khi khởi động hoặc thay đổi trạng thái.
+ .path: Đơn vị này xác định một đường dẫn mà có thể được sử dụng cho path-based activation. Theo mặc định, một đơn vị .service của tên cơ sở tương tự sẽ được bắt đầu khi con đường đạt đến trạng thái nhất định. Điều này sẽ sử dụng inotify giám sát các đường dẫn cho sự thay đổi.
+ .timer: Một đơn vị .timer định nghĩa một bộ đếm thời gian sẽ được quản lý bởi systemd, tương tự như một công việc định kỳ để kích hoạt chậm hoặc theo lịch trình. Một đơn vị trùng khớp sẽ được bắt đầu khi bộ đếm thời gian đạt đủ điều kiện.
+ .slice: Một đơn vị .slice được liên kết với Linux Control Group nodes, cho phép nguồn tài nguyên có thể bị hạn chế hoặc chỉ định cho bất kỳ quá trình liên kết với các slice. Tên đơn vị này phản ánh vị trí thứ bậc của nó trong cây cgroup. Các đơn vị được đặt trong slice nhất định theo mặc định tùy thuộc vào kiểu của chúng.
+ 

Init process là một tiến trình được khởi động lên đầu tiên trong hệ thống Linux. Tức là sau khi bạn chọn hệ điều hành trong menu của Boot Loader. Hệ điều hành bắt đầu được khởi động và tiến trình đầu tiên khởi động lên là init. Nhiệm vụ của init là start và stop các process, services… cần thiết khác.

Vì init là tiến trình được khởi động đầu tiên của hệ thống Linux nên:

Init process luôn có PID (Process ID) là 1.
Tiến trình init là tiến trình cha (hoặc ông nội =)) ) của các tiến trình khác.
Có ba kiểu triển khai init system chính trong hệ thống Linux là:

System V (đọc là System Five – V là số 5 la mã đó): là phiên bản truyền thống của init system trên nhiều hệ thống Linux.
Upstart: Được phát triển bởi Canonical vào khoảng năm 2009 và sử dụng trong các phiên bản Ubuntu cũ hơn bản 15.04.
Systemd: Là một init system được phát triển khoảng năm 2010 và được nhiều Linux distributions sử dụng để thay thế các init system cũ. Ubuntu từ phiên bản 15.04 và Centos từ phiên bản 7 đã sử dụng systemd làm init system mặc định.

Systemd không chỉ dừng lại ở việc start hoặc stop các services nó còn có thể mount filesystems, quản lý network sockets… Và để thực hiện được những công việc đó nó phân chia ra các đơn vị units:

Service units (.service) – để start và stop các service.
Mount units (.mount) – Để quản lý các mount point.
Target units (.target) – Để điều khiển các “runlevels” (khái niệm runlevels chỉ sử dụng trong SysV init).

Làm sao biết được hệ thống của bạn đang dùng systemd?
Để kiểm tra xem hệ thống của bạn có đang sử dụng systemd hay không thì:

Hãy kiểm tra xem bạn có thư mục /usr/lib/systemd không.
Gõ lệnh pstree -Ap | more bạn sẽ nhìn thấy systemd có process ID là 1 và nó là cha (hoặc ông nội :)) ) của các process khác.(ubuntu)