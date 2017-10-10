# Báo cáo ICINGA2 - Phạm Văn Tuấn 
 
## I. Tổng quan về ICINGA2 
 
Icinga là một hệ thống ứng dụng mã nguồn mở có chức năng giám sát mạng, là phần mềm được viết lại từ hệ thống Nagios năm 2009 với nhiều chức năng mới hơn như giao diện web 2.0, kết nối cơ sở dữ liệu bổ sung (MySQL, Oracle, PostgreSQL) và API REST cho phép các lập trình viên tích hợp thêm các extension mà không cần sửa đổi phức tạp trong phần lõi của icinga. 
 
Icinga2 cũng tương thích ngược với Nagios, tạo điều kiện thuận lợi cho việc chuyển đổi giữa hai nền tảng giám sát . 
 
Các chức năng 
 **Monitor**
  * Giám sát các dịch vụ mạng (SMTP, POP3, HTTP, NNTP, ping, ...) 
  * Giám sát các tài nguyên máy chủ lưu trữ (tải CPU, sử dụng disk, v.v.) 
  * Giám sát các thành phần máy chủ (thiết bị chuyển mạch, bộ định tuyến, cảm biến nhiệt độ và độ ẩm ...) 
  * Plugin đơn giản cho phép người dùng dễ dàng phát triển kiểm tra dịch vụ của mình 
  * Kiểm tra dịch vụ song song 
  * Khả năng xác định phân cấp máy chủ lưu trữ cho phép phát hiện và phân biệt giữa các máy chủ bị hỏng và những máy không thể truy cập được 
  * Khả năng xác định trình xử lý sự kiện sẽ được chạy trong các sự kiện dịch vụ hoặc máy chủ để giải quyết vấn đề chủ động 
 
 **Notification**
 * Thông báo người liên lạc khi các sự cố dịch vụ hoặc máy chủ xảy ra và được giải quyết (thông qua email, SMS hoặc phương pháp do người dùng xác định) 
 * Tăng cường cảnh báo cho người dùng khác hoặc kênh truyền thông 
 
 **Visualisation & Reporting**
 * Hai giao diện người dùng tùy chọn (Icinga Classic UI và Icinga Web) để hiển thị trạng thái máy chủ và dịch vụ, bản đồ mạng, báo cáo, nhật ký ... 
 * Report Icinga dựa trên Báo cáo Jasper nguồn mở cho cả giao diện người dùng Icinga Classic và Icinga Web 
 * Report dựa trên mẫu 
 * Báo cáo kho với mức độ truy cập khác nhau và tạo báo cáo tự động và phân phối 
 * Báo cáo sử dụng năng suất 
 * Hiệu suất đồ thị thông qua tiện ích như PNP4Nagios, NagiosGrapher và InGraph 
 
Cấu trúc 
 
![1](https://www.icinga.com/wp-content/uploads/2011/08/Architecture_1.5_800px.png )
 
 * Icinga Core: quản lý các nhiệm vụ giám sát, nhận được kết quả kiểm tra từ các plug-in khác nhau. Sau đó, kết nối các kết quả này với IDODB.
 * Icinga 2: quản lý các nhiệm vụ theo dõi, chạy kiểm tra, gửi thông báo. Các tính năng của Icinga 2 có thể được kích hoạt theo yêu cầu, có thể là các tính năng mặc định như thành phần "checker" hoặc "notification" hoặc các giao diện bên ngoài tương thích với Icinga 1.x và các giao diện người dùng 
 * Icinga's User Interfaces:
   * Icinga Classic UI (còn được gọi là Classic Web) dựa trên các mô hình CGI của Nagios và giữ lại định dạng của nó. Dự án Icinga tiếp tục bổ sung các tính năng mới cho giao diện này như pagination, đầu ra JSON, và CSV export 

   * Icinga Web 2 hiện đang được phát triển song song với Giao diện người dùng Cổ điển và Web và đã được công bố trong Hội nghị Giám sát Mã nguồn Mở vào tháng 11 năm 2013 

 * Icinga Data Out Database: (IDODB) là điểm lưu trữ dữ liệu giám sát lịch sử cho các tiện ích bổ sung hoặc giao diện Web Icinga để truy cập 
 * Icinga Reporting: Dự án Icinga cung cấp mô-đun Icinga Reporting tùy chọn dựa trên Báo cáo Jasper nguồn mở. Nó có thể được tích hợp vào cả giao diện người dùng Icinga Classic và Icinga Web. 
 * Icinga Mobile: là một giao diện người dùng cho điện thoại thông minh và trình duyệt máy tính bảng chạy trên WebKit 
 
Ưu điểm 
Thiết kế mô đun cho phép bạn chọn plugin để cài đặt. 
Dễ dàng di chuyển từ Nagios 
Một trong những giải pháp báo cáo và giám sát kỹ lưỡng nhất 
Nhược điểm: 
Cấu hình có thể là khó khăn. 
Tài liệu, mặc dù rộng rãi, nhưng không bao gồm một hướng dẫn cụ thể nào 
 
Thực hiện LAB 
Chuẩn bị 
 
icinga2-master: 
OS: ubuntu server 16.04 
IP: 10.0.0.1 (internal) 
CPU: 1 core, Disk: 8GB, RAM: 256MB 
node1: 
OS: ubuntu server 16.04 
IP: 10.0.0.2 (internal) 
CPU: 1 core, Disk: 8GB, RAM: 256MB 
node2: 
OS: ubuntu server 16.04 
IP: 10.0.0.3 (internal) 
CPU: 1 core, Disk: 8GB, RAM: 256MB 
Cài đặt và cấu hình 
Cài đặt icinga2 và icingaweb2 trên icinga2-master 
Trên icinga2-master 
 
apt-get update && apt-get upgrade –y 
 
Thêm repo & cài đặt icinga2 
add-apt-repository ppa:formorer/icinga && apt-get update 
apt-get install -y icinga2 icinga2-ido-mysql 
Cài đặt icingaweb2 
apt-get install -y mysql-server php7.0 libapache2-mod-php7.0 
 
Thêm mật khẩu cho mysql 
 
apt-get install -y icingaweb2 
Thiết lập mySQL là backend cho icinga nếu lỡ ấn No trong khi cài đặt mysql 
icinga2 feature enable ido-mysql command 
Set timezone cho php 
vim /etc/php/7.0/cli/php.ini 
thêm dòng date.timezone = Asia/Ho_Chi_Minh 
 
Restart apache 
/etc/init.d/apache2 restart 
Như vậy đã cài đặt xong, giờ chúng ta tiếp tục trên nền web để thiết lập 
Vào trình duyệt: http://<icinga2IP>/icingaweb2/setup 
 
Lấy token từ icinga2SVR và nhập vào 
icingacli setup token create 
Chọn module cần cài, ở đây chỉ tick monitoring  
Check lại các gói yêu cầu 
 
Chọn Authentication là Database 
 
Tạo database cho icingaweb 
 
Điền user và password đã tạo khi cài mysql 
 
Tạo tài khoản quản trị nền web 
 
Configure log 
 
Review 
 
Chọn IDO là backend  
Tạo database cho IDO 
 
Tiếp tục chọn các thiết lập mặc định 
 
 
Đang chèn hình ảnh... 
Hoàn tất việc cài đặt, click vào login để đăng nhập vào giao diện web 
 
 
Monitoring DISK, RAM, CPU 
Sơ lược về một số file config trong icinga: /etc/icinga2 
icinga.conf: Đây là nơi cấu hình cài đặt cho ứng dụng Icinga bao gồm hosts/services để check 
constants.conf: file cấu hình có thể được sử dụng để xác định tham số global, xác định thư mục nguồn của các plugins có sẵn và mở rộng. 
zones.conf: Tệp này có thể được sử dụng để xác định đối tượng cấu hình Zone và Endpoint yêu cầu cho giám sát phân tán. 
thư mục conf.d: Thư mục này chứa cấu hình ví dụ sẽ giúp bắt đầu theo dõi máy chủ lưu trữ cục bộ và các dịch vụ của nó. Ví dụ: 
hosts.conf 
services.conf 
users.conf 
notifications.conf 
commands.conf 
groups.conf 
templates.conf 
downtimes.conf 
timeperiods.conf 
satellite.conf 
api-users.conf 
app.conf 
icinga2 cung cấp một số service check sẵn, như load, procs, swap, users, icinga, ping4, ping6, ssh, http, optional: Icinga Web 2, disk, disk / 
Change Directory vào thư mục conf.d 
root@icinga2:~# cd /etc/icinga2/conf.d/ 
Xóa file host.conf đi, ta sẽ tự tạo ra một thư mục mới để quản lý các node bằng các file conf theo tên node cho dễ 
root@icinga2:/etc/icinga2/conf.d# rm -rf hosts.conf 
root@icinga2:/etc/icinga2/conf.d# mkdir hosts 
root@icinga2:/etc/icinga2/conf.d# cd hosts 
root@icinga2:/etc/icinga2/conf.d# vi node1.conf 
Ở đây ta sẽ sử dụng service check sẵn là hostalive (ping4), ssh và disk 
object Host "node1" { 
        import "generic-host" 
        address = "10.0.0.2" 
        check_command = "hostalive" 
} 
  
object Service "ssh" { 
        host_name = "node1" 
        check_command = "ssh" 
} 
  
  
object Service "disk" { 
        host_name = "node1" 
        check_command = "disk" 
} 
 
Config tương tự với node2.conf 
Như thế ta đã monitoring được thông tin disk trên node1 và node2, để monitoring được thông tin RAM và CPU ta sẽ cần snmp 
Cài đặt snmp trên node1 và node2 
root@node1:~# apt-get install snmpd 
root@node2:~# apt-get install snmpd 
Xóa nội dung file snmpd.conf và chỉ cần thêm thông tin rocommunity string. 
root@node1:~# > /etc/snmp/snmpd.conf  
root@node1:~# vi /etc/snmp/snmpd.conf 
Ở đây dùng rocommunity string là "vtp" 
 
Restart lại dịch vụ 
root@node1:~# service snmpd restart  
Làm tương tự với node2 
Trên icinga2 master, ta phải cài đặt thêm plugin nagios-snmp-plugins 
root@icinga2:~# apt-get install nagios-snmp-plugins  
Trong file các file conf của node1 và node2, ta phải khai báo thông tin snmp_address và snmp_community 
object Host "node1" { 
        import "generic-host" 
        address = "10.0.0.2" 
        vars.snmp_address = "10.0.0.2" 
        vars.snmp_community = "vtp" 
        check_command = "hostalive" 
} 
Theo dõi các thông tin CPU, RAM bằng các check_command của snmp 
object Service "snmp-load" { 
    host_name           = "node1" 
    vars.snmp_load_type = "netsl" 
    vars.snmp_warn      = "5,3,2" 
    vars.snmp_crit      = "6,5,3" 
    check_command       = "snmp-load" 
} 
  
object Service "snmp-memory" { 
    host_name      = "node1" 
    // Set the Memory warning for Ram and swap Respectively. 
    // Uses percents. 
    vars.snmp_warn = "50,0" 
    vars.snmp_crit = "80,0" 
    check_command  = "snmp-memory" 
} 
Reload lại service icinga2 
root@icinga2:~# service icinga2 reload 
Làm tương tự với node2, sau đó ta sẽ lên giao diện web để kiểm tra 
Những phần nào đang pending, ta có thể ấn vào check now để đẩy nhanh quá trình. Khi hoàn tất sẽ có kết quả như sau: 
 
Như vậy chúng ta đã cài đặt và monitor thành công 2 server Linux bằng icinga2 
