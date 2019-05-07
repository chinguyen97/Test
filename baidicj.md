### Bài dịch
## LINUX PID 1 & SYSTEMD

Để nói Systemd, bạn phải bắt đầu với việc khởi động hệ điều hành LINUX. Việc khởi động HĐH Linux bắt đầu bằng BIOS, sau đó kernel được tải bởi Boot Loader và kernel được khởi tạo. Bước cuối cùng trong khởi tạo kernel là bắt đầu tiến trình init. Tiến trình này là tiến trình đầu tiên của hệ thống, PID là 1, hay còn gọi là siêu tiến trình, hay tiến trình gốc. . Nó chịu trách nhiệm tạo ra tất cả cac stieens trình của người dùng khác. Nếu tiến trình này thoát tất cả các tiến trình khác sẽ bị hủy. (PID 0 là 1 phần của nhân chứ không phải 1 tiến trình người dùng)
#### SysV Init
PID 1 là quá trình đặc biệt, đưa toàn bộ hệ thống vào trạng thái hoạt động. Ví dụ: Khởi chạy UI-Shell để tương tác vpsi người dùng thông qua giao diện đồ họa X. Theo truyền thống, PID 1 tương tác với Unix System V truyên fthoongs, do đó SysV init còn được gọi là sysvinit triển khai lâu đời nhất. Unit System V được phát hành năm 1983.
```
1 số linux Ó sử dụng Sysem V
- Debian 6 trở về trước
- ubuntu  9.04 trở về trươvs
- CentOS 5 trở về trước
```
Trong SysV init, có 1 số phương thức hoạt động được gọi là runlevel. Ví dụ: Level 3: bắt đầu giao diện dòng lệnh với nhiề người dùng. Level 5: Bắt đầu gao diện đồ họa, 0: tắt máy, 6 khởi động lại. File cấu hình `/etc/inittab`

Gói này có `/etc/init.d` và `/etc/rc[X].d` lưu trữ các quá trình bắt đầu và ngừng các tiến trình (theo thông số kỹ thuật hỗ trợ các lệnh cần thiết start, stop), X đại diện cho các runlevel khác nhau, chẳng hạn `/etc/rc3.d` là runlevel 3. các tập tin bên trong `/etc/init.d` là tập các lệnh bắt đầu và dừng liên kết. Có 1 số quy ước đặt tên: bắt đầu với K hoặc S, tiếp theo là các con số, sau đó đến các tùy chỉnh, chẳng hạn như `S01rsyslog`, `S02ssh`. S có nghĩa là bắt đầu, K có nghĩa là dừng và các số chỉ thứ tự thực hiện.
#### UpStart 
Sysvinit hoạt động nhiều năm trên HĐH unix và Linux, đến khoảng năm 2006, Linux kernel vào thời đại 2.6, Linux cso nhiều thông tin cập nhật. Hơn nữa Linux bắt đầu xauats hiện trong trong hệ thống máy tính để bàn, và hệ thống máy chủ khác. Hệ thống máy tính để bàn thường xuyên được khởi động lại và ngườu dùng sử dụng công nghệ cắm nóng phần cứng thường xuyên. Vì vậy sysvnit có nhiều thách thức. 

Ví dụ: Máy in cần 1 quy trình dịch vụ như CUPS, nếu như người dùng không có máy in thì việc khởi động lại dịch vụ gây lãng phí, nếu không được khởi động, nếu máy in được sử dụng, nó sẽ không thể sử dụng được vì sysvint không có cơ chế phát hiện tự động. Có thể bắt đầu tất cả cac sdichj vụ cùng 1 lúc. Người ra còn 1 số vân sđề với việc gắn đĩa mạng. Trong `/etc/fstab` chứa các đĩa cứng để mount, vfa đôi kbi là 1 ổ đĩa mạng (NFS hoặc iCSI), nhưng trên máy tính để bàn, mạng không thể truy cập vào đĩa cứng, nó không cso khả năng mount. Điều này ảnh hưởng đến tốc độ khởi động. SysVinit ắp dụng netdev để giải quyết vân sđề này, có nghĩa là nhu cầu của người dùng trong /etc/fstab cấu hình ổ cứng tương ứng với netdev, vì vậy SysVinit không gắn kết nó lúc khởi động, chỉ sau khi mạng có sẵn, bởi chuyên gia ngành netfs quá trình dịch vụ để miunt. Loai quản lý này khó quản lý hơn, và gây khó khăn cho mọi người. 

Vì vậy, các nhà phát triển Ubuntu đã quyết định thiết kế lại hệ thống sau khi đánh giá một hệ thống init tùy chọn tại thời điểm đó, vì vậy Upstart xuất hiện. Upstart dơaj trên cp chế hướng sự kiện, dịch vụ khởi động đồng bộ hoàn toàn nối tiếp trước đó đã được thay đổi bằng phương pháp không dồng bộ theo hướng sự kiện. Ví dụ: Nếu ổ flash USB được lắp vào, udev sẽ nhận thông báo upstart  và chương trình dịch vụ tương ứng đưc[j kích hoạt sau khi sự kiện được cảm nhận, cahwngr hạn như gán tệp hệ thóng. Do sử dụng dịch vụ hướng sự kiện, khi khởi động hệ điều hành, nhiều dịch vụ không cần thiết có thể được khởi động mà không phải chờ đợi. Và lợi ích của hướng sự kiện là các dịch vụ có thể được bắt đầu song song và các phụ thuộc giữa chúng được hoàn thành bằng thông báo sự kiện tương ứng.

Upstart có một thiết kế rất tốt, hai trong số đó quan trọng nhất là Công việc và Sự kiện.

Công việc có một công việc chung, một công việc dịch vụ và upstart quản lý vòng đời của toàn bộ công việc, như: Chờ đợi, Bắt đầu, Trước khi bắt đầu, Sinh sản, Sau khi bắt đầu, Chạy, Dừng trước, Dừng lại, Bị giết, Sau khi dừng Đợi và duy trì bộ máy trạng thái của vòng đời này.

Sự kiện được chia thành ba loại signal, method và hooks. signal Đó là một thông điệp,  method đồng bộ bị chặn đồng bộ. hooks Nó cũng đồng bộ, nhưng giữa hai lần đầu tiên, quá trình phát hành sự kiện hook phải đợi cho đến khi sự kiện hoàn thành, nhưng không kiểm tra thành công.

Tuy nhiên, upstart các sự kiện rất phức tạp và rất hỗn loạn, và các sự kiện khác nhau (các sự kiện không được phân loại) gây ra một chút nhầm lẫn. Tuy nhiên, do trước khi thiết kế toàn bộ sự kiện- driven của nó tốt hơn sysvinit nhiều do đó, cũng giành được hoan nghênh.
```
Một số distro hỗ trợ Upstart (đa phần là Ubuntu vì Upstart vốn được phát triển cho riêng Ubuntu)

Từ Ubuntu 9.10 tới Ubuntu 14.04, 14.10
CentOS 6
```
#### Systemd
Năm 2010, Lennart Poettering  và  Kay Sievers giới thiệu 1 init mới: systemd. Đây là 1 dự án lớn, tham vọng đã thay đổi hầu hết mọi thứ, systemd không chỉ thay đổi hệ thống hiện tại mà nó còn làm được nhiều hơn thế nữa. 

Lennart đồng ý rằng upstart làm tốt, chất lượng mã rất tốt, thiết kế dựa trên sự kiện cũng rất tốt. Tuy nhiên ông cảm tupstart có 1 số vấn đề, vấn đề lớn nhất là không đủ nhanh, mặc dù upstart đã đạt đến trình độ xử lý song song, nhưng bản chất các sự kiện nayfvaanx bắt đầu các quá trình nối tiếp nhau. Chẳng hạn như: NetworkManager chờ đợi sự kiện D-Bus bắt đầu, trong khi D-Buschờ đợi sự kiện syslog bắt đầu.

Lennart tin rằng, về mặt triển khai, upstart thực sự đang quản lý cây phụ thuộc dịch vụ logic, nhưng cây phụ thuộc dịch vụ này tương đối đơn giản trong biểu thức. Tuy nhiên, Lennart nói rằng sự đơn giản hóa này thực sự gây bất lợi. Ông tin rằng: 

- Từ quan điểm quản lý hệ thống, ông sẽ thiết lập cây phụ thuộc dịch vụ mà toàn bộ hệ thống bắt đầu lại từ đầu, nhưng quản trị viên hệ thống muốn dịch dịch vụ sạch ban đầu sang máy tính. Biểu mẫu Sự kiện / Hành động và cấu hình Sự kiện / Hành động là thời gian chạy, vì vậy bạn cần chạy nó để biết nó là gì.
- Sự kiện logic ở khắp mọi nơi từ đầu đến chân. Sự kiện này mở rộng sự phức tạp của vận hành và bảo trì, không còn tốt như trước sysvint. Đó là, khi bạn cấu hình "Start D-Bus Hãy bắt đầu NetworkManager", điều này upstart có thể khô, nhưng ngược lại, nếu một người dùng bắt đầu NetworkManager, chúng ta nên đi để bắt đầu trước sự phụ thuộc của mình D-Bus, nhưng bạn phải cấu hình các sự kiện ngược lại. Ban đầu, tôi chỉ cần cấu hình một phụ thuộc và bây giờ tôi phải cấu hình nhiều sự kiện trong nhiều trường hợp.
Cuối cùng, upstartSự kiện không chuẩn, rất khó hiểu và không có định nghĩa tốt. Ví dụ: hiện có, quá trình bắt đầu, chạy, dừng sự kiện, chèn thiết bị USB, có thể sử dụng, các sự kiện chưa được cắm, cũng như thiết bị hệ thống tệp được gắn, gắn và gắn các sự kiện, cũng như ngắt kết nối và ngắt kết nối dây nguồn AC Sự kiện. Bạn sẽ thấy rằng quá trình bắt đầu, dừng, USB, hệ thống tệp và đường dây điện trông rất giống nhau, nhưng nó không được chuẩn hóa, bởi vì hầu hết các sự kiện là ba lần: Bắt đầu, điều kiện, dừng lại. mô hình thiết kế khái niệm này và không có trong upstartxuất hiện trong. Do upstartđược thiết kế để trở thành một sự kiện duy nhất, trong khi bỏ qua các phụ thuộc logic.

Tất nhiên, nếu systemd chỉ để giải quyết upstart vấn đề này, ông sẽ chuyển upstart đủ, nhưng Lennart tham vọng không chỉ muốn làm một điều như vậy, ông muốn làm nhiều hơn nữa.

Trước hết, tôi nhận ra rằng mục tiêu chính của systemd trong quá quá trình init là cho phép người dùng nhanh chóng xâm nhập vào môi trường nơi hệ điều hành có thể được vận hành. Do đó, tốc độ phải nhanh, càng nhanh càng tốt, vì vậy systemd khái niệm thiết kế là hai:
+ Để bắt đầu ít hơn
+ Để bắt đầu vào sâu hơn trong Parallel
Nghĩa là, bắt đầu theo yêu cầu, không thể bắt đầu mà không bắt đầu, nếu bạn muốn bắt đầu, bạn có thể bắt đầu song song và bắt đầu song song, bao gồm cả sự phụ thuộc giữa bạn, tôi cũng bắt đầu song song. Tốt hơn là nên hiểu về khởi động theo yêu cầu, sau đó, khởi động song song với các phụ thuộc, làm thế nào để làm điều đó? Ở đây, systemddựa trên các hệ điều hành MacOS Launchdtrò chơi được chơi (có được một chia sẻ trên Youtube - launchd: Một để Rule Them All Program , trong trang web mã nguồn mở của Apple cũng liên quan hồ sơ thiết kế - các Daemons Về and Services )

Để giải quyết các phụ thuộc này, systemd cần giải quyết ba phụ thuộc cơ bản - Ổ cắm, D-Bus và hệ thống tệp.

Ổ cắm phụ thuộc . Nếu dịch vụ C phụ thuộc vào ổ cắm của dịch vụ S, thì hãy khởi động S trước, sau đó khởi động C, vì nếu C không thể tìm thấy ổ cắm S khi nó khởi động, C sẽ thất bại. systemdCó thể giúp bạn tạo một ổ cắm khi S chưa khởi động, để nhận tất cả các yêu cầu và dữ liệu C và lưu trữ bộ đệm đó, khi S được khởi động đầy đủ, hãy thay thế dữ liệu được lưu trong bộ nhớ cache và bộ mô tả ổ cắm bằng systemd Thay thế quá khứ.
 

D-Bus phụ thuộc . D-BusTên đầy đủ của Desktop Bus là một dịch vụ được sử dụng để liên lạc giữa các quy trình. Ngoài quy trình giao tiếp giữa quy trình người dùng và chế độ lõi, nó cũng được sử dụng trước quy trình chế độ người dùng. Bây giờ, rất nhiều các quy trình dịch vụ hiện tại được sử dụng D-Busđể giao tiếp thay vì Socket. Ví dụ: NetworkManagernó là thông qua D-Bus, có nghĩa là, nếu một quá trình cần phải biết tình trạng của quá trình mạng và các phương tiện khác phục vụ, sau đó ông sẽ cần phải D-Busgiao tiếp. D-BusHỗ trợ cho tính năng "Kích hoạt xe buýt". Nói cách khác, từ A đến D-Busdịch vụ và thông tin liên lạc B, nhưng B không khởi động, bạn D-Buscó thể đặt B lên trong quá trình khởi động của B D-Busgiúp bạn lưu trữ dữ liệu. systemdBạn có thể sử dụng tính năng này để bắt đầu song song A và B.
 

Hệ thống tập tin phụ thuộc . Các hoạt động liên quan đến hệ thống tệp tốn nhiều thời gian nhất trong quá trình khởi động hệ thống, chẳng hạn như gắn hệ thống tệp, thực hiện kiểm tra đĩa (fsck) trên hệ thống tệp và kiểm tra hạn ngạch đĩa là các hoạt động tốn thời gian. Trong khi chờ đợi những công việc này hoàn thành, hệ thống không hoạt động. Các dịch vụ muốn sử dụng hệ thống tệp dường như phải đợi hệ thống tệp được khởi tạo trước khi chúng có thể bắt đầu. systemdTham khảo autofsý tưởng thiết kế dựa trên hệ thống tập tin và dịch vụ hệ thống tập tin khởi tạo chính nó có thể trở nên phức tạp cả công việc. autofsCó thể giám sát các hệ thống tập tin gắn kết trỏ đến một sự tiếp cận khi nó được kích hoạt để gắn kết các hoạt động, điều này được thực hiện bởi các hạt nhân automounterhỗ trợ và thực hiện mô-đun. Ví dụ, một open()thời gian vai trò hệ thống gọi trên một hệ thống tập tin, và gắn kết hệ thống tập tin đã không được thực hiện, lần này open()gọi hạt nhân treo cứng đợi cho đến khi sau khi gắn xong, trở về điều khiển open()các cuộc gọi hệ thống, và mở file bình thường. Và quá trình này autofslà tương tự.
 

Hình dưới đây là một trang của PPT từ bài thuyết trình của Lennart cho thấy sự ra mắt của các hệ thống init khác nhau.



Ngoài ra, systemd quản lý một số điều sau đây khi khởi động.

Thay thế khởi động theo kịch bản truyền thống bằng C. Như chúng ta đã đề cập ở trên, sysvintvới sự /etc/rcX.dra mắt của một loạt các kịch bản. Tuy nhiên, nhu cầu sử dụng các kịch bản awk, sed, grep, find, xargsvà do đó, những hệ điều hành lệnh rằng cần phải tạo ra một quá trình, quá trình này tạo ra rất nhiều chi phí, chìa khóa được tạo ra sau khi hoàn thành các quá trình này, quá trình này sẽ làm một cái gì đó để chỉ rắm lớn Nghỉ hưu. Nói cách khác, hệ điều hành của tôi đã làm rất nhiều thứ để bạn mở ra một quy trình, và kết quả là, bạn sẽ biến một chuỗi thành chữ thường và rút lui, hệ điều hành của tôi là gì?

Trong một bình thường của sysvinitkịch bản, có thể có hàng trăm của một trật tự như vậy. Vì vậy, chậm chết. Do đó, nó đã được systemdthay thế hoàn toàn bằng ngôn ngữ C. Nói chung, sysvinitthấp hơn, sau khi hệ điều hành khởi động xong, echo $$bạn có thể thấy, pid được giao nhiệm vụ trông giống như hàng ngàn, nhưng systemdhệ thống chỉ là hàng trăm.

Ngoài ra, systemd thực sự là một cách để kiểm soát quy trình dịch vụ - bạn có thể theo dõi tất cả các quy trình mà fork / exec ra khỏi quy trình dịch vụ.

Chúng tôi biết rằng các thực tiễn tốt nhất của quy trình dịch vụ Unix / Linux Daemon truyền thống về cơ bản là như thế này (đối với quy trình cụ thể, hãy xem bài viết " SysV Daemon ") -
Khi quá trình bắt đầu, hãy đóng tất cả các bộ mô tả tệp đang mở (ngoại trừ bộ mô tả chuẩn 0, 1, 2) và sau đó đặt lại tất cả xử lý tín hiệu.
Gọi để fork()tạo ra một quá trình đứa trẻ, ở trẻ em setsid(), sau đó quá trình cha mẹ thoát (cho nền)
Trong quy trình con, gọi lại fork(), tạo quy trình cháu và xác định rằng không có thiết bị đầu cuối tương tác. Sau đó quá trình con thoát ra.
Trong quá trình cháu, đầu vào tiêu chuẩn và đầu ra tiêu chuẩn để sai số chuẩn được kết nối /dev/nulltrên, mà còn để tạo ra các file pid, file log, tín hiệu quá trình liên quan đến ......
Điều cuối cùng là thực sự bắt đầu cung cấp dịch vụ.
 

Trong quá trình trên, quá trình dịch vụ, thêm vào hai forkbên ngoài sẽ được forkrất nhiều tiến trình con (ví dụ, một số quá trình dịch vụ Web sẽ dựa trên yêu cầu liên kết của người dùng forksub-process), cây quá trình là khá khó khăn để quản lý, bởi vì Khi quy trình cha mẹ thoát ra, quy trình con sẽ được nối với PID 1. Vì vậy, về cơ bản, bạn không thể tìm thấy tất cả các quy trình liên quan bằng tệp pid riêng của quy trình dịch vụ (đây là yêu cầu của nhà phát triển). Vì vậy, quá cao, theo cách truyền thống, sử dụng tập lệnh để bắt đầu và dừng dịch vụ hoàn toàn có thể so sánh với Buggy, vì không thể giám sát con cháu sinh ra trong tất cả các dịch vụ.
 

Để giải quyết vấn đề này, upstartqua biến thái straceđể theo dõi quá trình fork()và exec()hoặc exit()các cuộc gọi và các hệ thống liên quan khác. Phương pháp này khá vụng về. systemdSử dụng một lối chơi rất thú vị để theo dõi tất cả các quy trình mà quy trình dịch vụ sinh ra , đó là cgroup(tôi đã từng nói về điều này trong "bài viết nhóm" công nghệ cơ bản của Docker ). cgroup được sử dụng chủ yếu để quản lý các nhóm quá trình hạn ngạch nguồn điều, vì vậy, bất kể như thế nào dịch vụ bắt đầu tiến trình con mới, tất cả trong số đó sẽ thuộc về các quá trình có liên quan cùng cgroup, vì vậy, systemdchỉ cần vào traverse tương ứng về cgrouphệ thống tập tin ảo Các tệp trong thư mục sẽ có thể tìm thấy tất cả các quy trình có liên quan một cách chính xác và dừng từng cái một.
 

Ngoài ra, nó systemdđơn giản hóa quá trình phát triển toàn bộ daemon:

Bạn không cần phải làm điều đó hai lần fork(), bạn chỉ cần thực hiện logic chính của chính dịch vụ.
Không setsid(), nó systemdsẽ giúp bạn
Không cần bảo trì pid文件, nó systemdsẽ giúp.
Không cần quản lý tệp nhật ký hoặc sử dụng sysloghoặc xử lý HUPtín hiệu tải lại nhật ký. Các hit đăng nhập stderrvào, systemdgiúp bạn quản lý.
Xử lý SIGTERMtín hiệu, mà là quyền để thoát khỏi dịch vụ hiện tại, đừng làm những việc khác.
......
Ngoài ra, systemd vẫn có thể -

Không có sự phụ thuộc vòng tròn giữa các dịch vụ tự động phát hiện khởi động.
Tích hợp quản lý tự động tự động.
Đăng nhập dịch vụ. systemd Chuyển đổi vấn đề nhật ký hệ thống truyền thống, lưu nhật ký ở định dạng nhị phân, chỉ mục nhật ký nhanh hơn.
Ảnh chụp và phục hồi. Chụp nhanh bộ dịch vụ hiện tại đang chạy trên hệ thống và khôi phục nó.
......
Có rất nhiều thứ, anh ấy đã tiếp quản rất nhiều thứ, rất nhiều người buồn bã vì anh ấy đang làm rất nhiều thứ không phải là một phần của PID 1.

Lập luận và tin đồn hệ thống
Vì vậy, systemdđiều này đã trở thành một cuộc chiến tranh của những lời bao giờ có thể là một trong những phần mềm mã nguồn mở lớn nhất. systemdTranh cãi lớn nhất với tất cả các loại tranh cãi là ông phá hoại triết lý thiết kế Unix (triết lý liên quan có thể đọc " Nghệ thuật lập trình Unix "), làm một việc lớn và đầy đủ và khá phức tạp. Tất nhiên, Lennart không đồng ý với lập luận cho rằng, sau này ông đã viết một blog " tại The Biggest thần thoại " để giải thích systemdkhông phải là trường hợp, chúng ta có thể đi đọc.

Tranh cãi như thế nào? 2014, Debian Linux vì chúng tôi muốn chuẩn bị để sử dụng systemdnhư một daemon init tiêu chuẩn để thay thế sysvinit. Những tranh cãi xung quanh vấn đề này đã đạt đến một mức độ nhiệt tình chưa từng thấy. Cuộc tranh cãi đầy thù hận. systemdNhững người ủng hộ và những người chống đối đã xúc phạm lẫn nhau, dẫn đến sự khởi đầu của trại Debian. Những người khác đã gửi email đe dọa đến Lennart, thuê một kẻ giết người bằng bitcoin, đe dọa lấy mạng anh ta, tải lên một bài hát xúc phạm anh ta trên Youbute, và gửi anh ta xuống và lăng mạ IRC và các kênh xã hội khác nhau. Tin nhắn. Điều này không còn gây tranh cãi, mà là một loại thù hận!



Vì vậy, Lennart đã đăng một bài đăng trên Google Plus , chỉ trích toàn bộ cộng đồng nguồn mở Linux và chính Linus. Ông nói với bức tranh lớn,

Cộng đồng này quá bệnh hoạn, tất cả đều là lỗ đít. Bạn không ngừng sử dụng nhiều cách khác nhau để lăng mạ và chửi rủa ở những nơi và ngôn ngữ khác nhau. Tôi vẫn là một chàng trai trẻ, tôi chưa bao giờ trải qua một cảnh như vậy, nhưng hôm nay tôi đã quen thuộc với cảnh này. Đôi khi tôi có thể không nói được, nhưng tôi sẽ không nói như anh ấy. Tôi không bị ảnh hưởng bởi những điều này, vì khuôn mặt của tôi đủ dày, vậy tại sao tôi có thể systemdthành công trong việc chống đối lớn như thế nào?  Tuy nhiên, cộng đồng Linux của bạn rất tệ. Có quá nhiều người mắc bệnh tâm thần trong bạn. Ngoài ra, đối với Linus Torvalds, bạn là Mô hình Vai trò của cộng đồng này, nhưng thật không may, bạn là Mô hình Vai trò Xấu, những lời nói và hành động xúc phạm trong cộng đồng, ở một mức độ nhất định, khuyến khích người khác giống như bạn, tất nhiên, Đó không chỉ là vấn đề của riêng bạn, mà là một nhóm người như bạn đang tụ tập quanh bạn. Gửi cho bạn một từ - Một con cá thối từ đầu xuống! Một con cá đang thối rữa từ mặt đất xuống ...

Văn bản này rất dài và những sinh viên thích buôn chuyện có thể đọc bài đầu tiên. Cảm thấy tâm lý của Lennart tại thời điểm đó (tôi nghĩ rằng nó có thể được coi là rất trơn tru).

Linus cũng là một phương tiện truyền thông hỏi systemdđiều này đi (xem " Torvalds (Pic) của Ông có ý kiến mạnh mẽ New NO ON systemd "), Linus nói trong một cuộc phỏng vấn,

Tôi có systemdkhông có ý kiến mạnh mẽ và bài viết Lennart. Mặc dù triết lý thiết kế Unix truyền thống - "Làm một việc và làm tốt", rất tốt và hầu hết chúng ta đã thực hành trong nhiều năm, nhưng điều này không đại diện cho tất cả thế giới thực. Trong lịch sử, nó không chỉ systemdđược thực hiện theo cách này. Tuy nhiên, tôi vẫn là một người lỗi thời, ít nhất tôi thích nhật ký dựa trên văn bản, không phải nhật ký nhị phân. Nhưng systemdbạn không nhất thiết phải có hương vị như vậy. Ồ, tôi đã nói chi tiết ...

Ngày nay, nó systemdchiếm cấu hình mặc định của hầu hết tất cả các bản phân phối Linux chính, bao gồm: Arch Linux, CentOS, CoreOS, Debian, Fedora, Megeia, OpenSUSE, RHEL, SUSE Enterprise và Ubuntu. Hơn nữa, đối với CentOS, CoreOS, Fedora, RHEL, SUSE, những phân phối này, không thể có được systemd. (Ubuntu cũng có một wiki đẹp - Systemd for Upstart Users  giải thích cách chuyển đổi giữa hai)

 

Khác
Tôi vẫn còn nhớ trong bài viết "Quy trình cập nhật bộ đệm ", tôi đã nói rằng nếu bạn muốn làm kiến ​​trúc, trước tiên bạn phải hiểu kỹ kiến ​​trúc máy tính và công nghệ cơ bản của nhiều cổ vật cũ . Bởi vì có rất nhiều thứ có thể được mượn và truyền đạt. Vì vậy, bạn đã thấy một cái gì đó tương tự như một kiến ​​trúc phân tán từ bài viết này?

Ví dụ: từ sysvinitđến upstartvà sau đó systemd, rất nhiều giống như một quản trị dịch vụ? Các quy trình dịch vụ này trong các hệ thống Linux có giống như các dịch vụ siêu nhỏ trong kiến ​​trúc phân tán không? Và D-Bus, nó có giống với ESB trong SOA không? Và hệ thống init có giống như một hệ thống điều khiển không? Thậm chí giống như một hệ thống dàn nhạc dịch vụ?

Cũng có nhiều phụ thuộc giữa các dịch vụ trong một hệ thống phân tán. Vì vậy, khi bắt đầu một kiến ​​trúc, nếu chúng ta có thể làm điều đó song song như systemd, nó có giống như một dịch vụ siêu nhỏ không?

Chà, bạn sẽ thấy rằng nhiều thứ kỹ thuật được kết nối và nhau có bóng của nhau, nên trên thực tế, không có nhiều công nghệ. Điều quan trọng là liệu chúng ta đã học được bề mặt hay nhìn thấy bản chất.



