## Cấu hình network Ubuntu Server 14.04 và CentOS7

A. [UbuntuServer14](#Ubuntu)
1. [Cấu hình IP tạm thời](#iptamthoi)
2. [Cấu hình IP tĩnh](#iptinh)
3. [Cấu hình IP động](#ipdong)
4. [Một số câu lệnh khác](#caulenhkhac)
B. [CentOS7](#centos7)
1. [Cấu hình Ip tĩnh bằng cách sửa file cấu hình](#suafilecauhinh)
2. [Cấu hình IP với chương trình dịch vụ Network Manager](nmtui)
3. [Một số câu lệnh khác](#caulenhkhac1)
<a name="Ubuntu"></a>
### A. UbuntuServer 

<a name="iptamthoi"></a>
### 1. Cấu hình IP tạm thời
- Cấu hình IP tạm thời: Sẽ mất sau khi reboot máy tính

	+ Câu lệnh: 
```
ifconfig ethX IP-address netmask net-address
```
Ví dụ: `ifconfig eth0 192.168.1.2 netmask 255.255.255.0`
	
	+ Đặt default gateway
```
route add default gw ip-getway {interface-name}
```
Ví dụ: `route add default gw 192.168.1.1 eth0`

<a name="iptinh"></a>
### 2. Cấu hình Ip tĩnh 
Cấu hình vẫn giữ sau khi reboot máy tính

Thông tin về cấu hình IP lưu trong file `/etc/network/interfaces`

Ví dụ: Cấu hình IP tĩnh cho giao diện eth0
```
auto eth0
iface eth0 inet static
	address 192.168.1.2 # IP address
	netmask 255.255.255.1.0 #subnet mask
	gateway 192.168.1.1 # gateway 
	dns-nameservers 8.8.8.8
	dns-nameservers 8.8.4.4
```
- Khởi động lại dịch vụ mạng để lấy cấu hình mới

```sudo /etc/init.d/networking restart```

Hoặc : ```service networking restart```

<a name="ipdong"></a>
### 3. Cấu hình IP động
Nhận địa chỉ IP một cách tự động từ một DHCP server

Tương tự như cấu hình IP tĩnh. Thay từ khóa **static** bằng **dhcp**
```
auto eth0
iface eth0 inet dhcp
```

<a name="caulenhkhac"></a>
### 4. Các câu lệnh khác
- Tắt/mở giao diện mạng
	+ ifup < tên card mạng>
	+ ifdown < tên card mang>
- Xem có bao nhiêu giao diện Ethernet

	+ `ifconfig ­a | grep eth`

- Xem tất cả giao diện mạng

	+ `ifconfig -a`

	+ Hoặc : `ip a`

- Xem cấu hình card eth0

	+ `ifconfig eth0`

	+ Hoặc: `ip a s eth0`

- Kiểm tra thông tin DNS

	+ `Cat /etc/resolv.conf`

- Xem đường đi 

	+ `route –n` 
	+ Hoặc `ip r`

<a name="centos7"></a>
### B. Cấu hình Network CentOS7

- Liệt kê các card mạng : 
`ip link show`

Hoặc có thể sử dụng lệnh `nmcli` của chương trình dịch vụ **NetworkManager**. Card nào hiện thị trạng thái connected là card đó đã được cấu hình để quản lý bởi chương trình Network Manager.
Câu lệnh: `nmcli –p dev`

<a name="suafilecauhinh"></a>
### 1. Cấu hình Ip tĩnh bằng cách sửa file cấu hình

Giả sử card mạng có tên ens33

File cấu hình ở trong thư mục: `/etc/sysconfig/network-scripts/`

Chỉnh sửa file ifcfg-ens33 

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33 
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33g
UUID=99576993-d359-49a8-9ef6-384535442663
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.206.15
NETMASK=255.255.255.0
GATEWAY=192.168.206.2
DNS1=8.8.8.8
DNS2=8.8.4.4
PREFIX=24
```

Dưới đây là các option cần chú ý:

|Option|Mô tả|
|---|---|
|DEVICE=ens33|Tên card mạng|
|NAME|Giống DEVICE|
|ONBOOT=yes|Kích hoạt card mạng khi khởi động|
|NM_CONTROLLED=no|Nếu không sử dụng Network Manager chọn no|
|BOOTPROTO=none/static|Không sử dụng DHCP|
|IPADDR=192.168.206.15|Địa chỉ IP|
|NETMASK= 255.255.255.0|Địa chỉ Subnet|
|GATEWAY=192.168.206.2|Địa chỉ Getway|
|DNS1=8.8.8.8|Địa chỉ DNS|
|DNS2=8.8.4.4|Địa chỉ DNS|

Các dòng cấu hình ko phân biệt thứ tự ưu tiên chỉ cần có nội dung đúng. 

- Khởi động lại dịch vụ mạng Network 

	+ `service network restart`
	+ Hoặc  `systemctl restart network`

<a name="nmtui"></a>
### 2. Cấu hình IP với chương trình dịch vụ Network Manager

NetworkManager là một chương trình/dịch vụ hỗ trợ điều khiển quản lý mạng cũng như cấu hình hệ thống mạng trên CentOS 7. Mặc định thì chương trình này đã được cài đặt từ ban đầu. Nhưng cần phải cài đặt chương trình **NetworkManager Text User Interface-nmtui** nhằm cung cấp giao diện text cấu hình linh động tương tác với Network Manager ngay trên Terminal hoặc Console kết nối đến hệ thống thay vì phải dùng lệnh riêng của NetworkManager.
- Cài đặt chương trình nmtui bằng câu lệnh:
```
yum install NetworkManager-tui –y
```
- Muốn dùng Network Manager để quản lý các card mạng trước tiên phải thêm dòng **NM_CONRTROLLED=yes** vào file **/etc/sysconfig/network-scripts/ifcfg-ens33**

- Khởi động dịch vụ Network Manager 

```
systemctl start NetworkManager.service 
```
- Tiến hành cấu hình IP tĩnh chop card mạng esn33
```
nmtui edit ens33 
```
Xuất hiện giao diện như hình:

<img src="image/1.png">

- Khởi động lại dịch vụ mạng Network 

<a name="caulenhkhac1"></a>
### 3. Một số câu lệnh khác
- Kiểm tra thông tin Ip tĩnh 
	+ **ip a**
	+ Hoặc **ip a s ens33**
- Kiểm tra thông tin routing 
	+ **ip r**
- Kiểm tra thông tin DNS
	`Cat /etc/resolv.conf`


#### Tài liệu tham khảo: https://cuongquach.com/cau-hinh-ip-tinh-tren-centos-7.html
