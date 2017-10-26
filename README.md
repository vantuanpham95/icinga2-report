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
 
**Cấu trúc**
 
![1](https://www.icinga.com/wp-content/uploads/2011/08/Architecture_1.5_800px.png )
 
 * Icinga Core: quản lý các nhiệm vụ giám sát, nhận được kết quả kiểm tra từ các plug-in khác nhau. Sau đó, kết nối các kết quả này với IDODB.
 * Icinga 2: quản lý các nhiệm vụ theo dõi, chạy kiểm tra, gửi thông báo. Các tính năng của Icinga 2 có thể được kích hoạt theo yêu cầu, có thể là các tính năng mặc định như thành phần "checker" hoặc "notification" hoặc các giao diện bên ngoài tương thích với Icinga 1.x và các giao diện người dùng 
 * Icinga's User Interfaces:
   * Icinga Classic UI (còn được gọi là Classic Web) dựa trên các mô hình CGI của Nagios và giữ lại định dạng của nó. Dự án Icinga tiếp tục bổ sung các tính năng mới cho giao diện này như pagination, đầu ra JSON, và CSV export 

   * Icinga Web 2 hiện đang được phát triển song song với Giao diện người dùng Cổ điển và Web và đã được công bố trong Hội nghị Giám sát Mã nguồn Mở vào tháng 11 năm 2013 

 * Icinga Data Out Database: (IDODB) là điểm lưu trữ dữ liệu giám sát lịch sử cho các tiện ích bổ sung hoặc giao diện Web Icinga để truy cập 
 * Icinga Reporting: Dự án Icinga cung cấp mô-đun Icinga Reporting tùy chọn dựa trên Báo cáo Jasper nguồn mở. Nó có thể được tích hợp vào cả giao diện người dùng Icinga Classic và Icinga Web. 
 * Icinga Mobile: là một giao diện người dùng cho điện thoại thông minh và trình duyệt máy tính bảng chạy trên WebKit 
 
**Ưu điểm:**
 * Thiết kế mô đun cho phép bạn chọn plugin để cài đặt. 
 * Dễ dàng di chuyển từ Nagios 
 * Một trong những giải pháp báo cáo và giám sát kỹ lưỡng nhất 
 
**Nhược điểm:** 
 * Cấu hình có thể là khó khăn. 
 * Tài liệu, mặc dù rộng rãi, nhưng không bao gồm một hướng dẫn cụ thể nào 
 
## II. Thực hiện LAB 
### 1. Chuẩn bị 
 ![topo](https://github.com/vantuanpham95/icinga2-report/blob/master/images/icinga2labtopo.png)
 
**icinga2-master:** 
 * OS: ubuntu server 16.04 
 * IP: 10.0.0.1 (internal) 
 * CPU: 1 core, Disk: 8GB, RAM: 256MB
 
**node1:**
 * OS: ubuntu server 16.04 
 * IP: 10.0.0.2 (internal) 
 * CPU: 1 core, Disk: 8GB, RAM: 256MB
 
**node2:**
 * OS: ubuntu server 16.04 
 * IP: 10.0.0.3 (internal) 
 * CPU: 1 core, Disk: 8GB, RAM: 256MB 
### 2. Cài đặt và cấu hình 
#### a. Cài đặt icinga2 và icingaweb2 trên icinga2-master 
**Trên icinga2-master**
 
    apt-get update && apt-get upgrade –y 
 
Thêm repo & cài đặt icinga2 
```
add-apt-repository ppa:formorer/icinga && apt-get update 
apt-get install -y icinga2 icinga2-ido-mysql 
```
Cài đặt icingaweb2 

    apt-get install -y mysql-server php7.0 libapache2-mod-php7.0 
 
 ![1](https://github.com/vantuanpham95/icinga2-report/blob/master/images/1)
 
Thêm mật khẩu cho mysql 
 
    apt-get install -y icingaweb2 

![2](https://github.com/vantuanpham95/icinga2-report/blob/master/images/2.png)

Thiết lập mySQL là backend cho icinga nếu lỡ ấn No trong khi cài đặt mysql 

    icinga2 feature enable ido-mysql command 

Set timezone cho php 
    
    vim /etc/php/7.0/cli/php.ini 

thêm dòng 

    date.timezone = Asia/Ho_Chi_Minh 
 ![3](https://github.com/vantuanpham95/icinga2-report/blob/master/images/3.png)
 
Restart apache 

    /etc/init.d/apache2 restart 

Như vậy đã cài đặt xong, giờ chúng ta tiếp tục trên nền web để thiết lập 

Vào trình duyệt: http://<icinga2IP>/icingaweb2/setup 
 
 ![4](https://github.com/vantuanpham95/icinga2-report/blob/master/images/4.png)
 
Lấy token từ icinga2SVR và nhập vào 

    icingacli setup token create 

Chọn module cần cài, ở đây chỉ tick monitoring  

Check lại các gói yêu cầu

![5](https://github.com/vantuanpham95/icinga2-report/blob/master/images/5.png)
 
Chọn Authentication là Database 
 
 ![6](https://github.com/vantuanpham95/icinga2-report/blob/master/images/6.png)
 
Tạo database cho icingaweb 
 
 ![7](https://github.com/vantuanpham95/icinga2-report/blob/master/images/7.png)
 
Điền user và password đã tạo khi cài mysql 
 
 ![8](https://github.com/vantuanpham95/icinga2-report/blob/master/images/8.png)
 
Tạo tài khoản quản trị nền web 
 
 ![9](https://github.com/vantuanpham95/icinga2-report/blob/master/images/9.png)
 
Configure log 
 
 ![10](https://github.com/vantuanpham95/icinga2-report/blob/master/images/10.png)
 
Review 
 
 ![11](https://github.com/vantuanpham95/icinga2-report/blob/master/images/11.png)
 
Chọn IDO là backend

Tạo database cho IDO 

![12](https://github.com/vantuanpham95/icinga2-report/blob/master/images/12.png)
 
Tiếp tục chọn các thiết lập mặc định 
 
![13](https://github.com/vantuanpham95/icinga2-report/blob/master/images/13.png)
 
![14](https://github.com/vantuanpham95/icinga2-report/blob/master/images/14.png)

Hoàn tất việc cài đặt, click vào login để đăng nhập vào giao diện web 

![15](https://github.com/vantuanpham95/icinga2-report/blob/master/images/15.png)
 
#### b. Monitoring DISK, RAM, CPU 
Sơ lược về một số file config trong icinga: /etc/icinga2 

 * icinga.conf: Đây là nơi cấu hình cài đặt cho ứng dụng Icinga bao gồm hosts/services để check 
 * constants.conf: file cấu hình có thể được sử dụng để xác định tham số global, xác định thư mục nguồn của các plugins có sẵn và mở rộng. 
 * zones.conf: Tệp này có thể được sử dụng để xác định đối tượng cấu hình Zone và Endpoint yêu cầu cho giám sát phân tán.
 * Thư mục conf.d: Thư mục này chứa cấu hình ví dụ sẽ giúp bắt đầu theo dõi máy chủ lưu trữ cục bộ và các dịch vụ của nó. Ví dụ: 
   * hosts.conf 
   * services.conf 
   * users.conf 
   * notifications.conf 
   * commands.conf 
   * groups.conf 
   * templates.conf 
   * downtimes.conf 
   * timeperiods.conf 
   * satellite.conf 
   * api-users.conf 
   * app.conf 

 * templates.conf: tệp này tạo ra một số file cấu hình sẵn để dành cho các host hay service có chung thuộc tính kế thừa templates này. Các object host hay service có thể kế thừa nhiều templates miễn là có thuộc tính phù hợp. Các thuộc tính khi được các object host hay service kế thừa rồi vẫn có thể chỉnh sửa lại trong code bằng cách ghi đè, hoặc thêm thuộc tính bằng cách sử dụng toán tử += 

 * users.conf: tệp này định nghĩa ra các users chứa thông tin mail hay số điện thoại để thực hiện thông báo. Tệp này cũng định nghĩa ra các groups là một nhóm các users khác nhau có chung một mục đích, để khi gửi thông báo thì không cần khai báo đầy đủ users ra. Users.conf chỉ phục vụ cho mục đích notifications. 

 * notifications.conf: tệp này sẽ áp các notifications được định nghĩa từ trước theo host hay service 
command.conf: tệp này định nghĩa các lệnh để thực hiện: mail-host-notification, mail-service-notification hay cũng có thể tự định nghĩa ra các lệnh đặc biệt để sử dụng tùy theo nhu cầu 

 * hosts.conf: tệp này khai báo ra các host cần monitor, trong đó cần khai báo các biến cần sử dụng để monitor host đó như: address, snmp address, cũng có thể khai báo ra các service check cho host đó 

Khi có nhiều host cần monitor và các service để monitor khác nhau, cũng như vấn gửi cảnh báo là khác nhau, thì việc khai báo thủ công là mất thời gian. Khi đó chúng ta có thể sử dụng các thuộc tính chung, tìm ra các điểm chung, điểm riêng của host hay service, sau đó assign hay ignore để áp dụng rule cho chuẩn xác. 

Ví dụ 
```
apply Notification "disk-notification-via-email" to Service { 
  import "mail-disk-notification" 
  assign where match("*disk*", service.name) 
} 
```
icinga2 cung cấp một số service check sẵn, như load, procs, swap, users, icinga, ping4, ping6, ssh, http, optional: Icinga Web 2, disk, disk / 

Change Directory vào thư mục conf.d 

    root@icinga2:~# cd /etc/icinga2/conf.d/ 

Xóa file host.conf đi, ta sẽ tự tạo ra một thư mục mới để quản lý các node bằng các file conf theo tên node cho dễ 
```
root@icinga2:/etc/icinga2/conf.d# rm -rf hosts.conf 
root@icinga2:/etc/icinga2/conf.d# mkdir hosts 
root@icinga2:/etc/icinga2/conf.d# cd hosts 
root@icinga2:/etc/icinga2/conf.d# vi node1.conf 
```

Ở đây ta sẽ sử dụng service check sẵn là hostalive (ping4), ssh và disk 
```
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
``` 

![16](https://github.com/vantuanpham95/icinga2-report/blob/master/images/16.png)

Ta có thể định nghĩa lại giá trị cảnh báo disk bằng cách sử dụng biến như sau: 
```
object Service "disk" { 
        host_name = "node1" 
        check_command = "disk" 
        vars.disk_wfree = "50%" 
        vars.disk_cfree = "20%" 
  
} 
```

Config tương tự với node2.conf 

Như thế ta đã monitoring được thông tin disk trên node1 và node2, để monitoring được thông tin RAM và CPU ta sẽ cần snmp 

Cài đặt snmp trên node1 và node2 

```
root@node1:~# apt-get install snmpd 
root@node2:~# apt-get install snmpd
```
Xóa nội dung file snmpd.conf và chỉ cần thêm thông tin rocommunity string. 
```
root@node1:~# > /etc/snmp/snmpd.conf  
root@node1:~# vi /etc/snmp/snmpd.conf 
```
Ở đây dùng rocommunity string là "vtp" 
 
 ![17](https://github.com/vantuanpham95/icinga2-report/blob/master/images/17.png)
 
Restart lại dịch vụ 

    root@node1:~# service snmpd restart  

Làm tương tự với node2 

Trên icinga2 master, ta phải cài đặt thêm plugin nagios-snmp-plugins 

    root@icinga2:~# apt-get install nagios-snmp-plugins  

Trong file các file conf của node1 và node2, ta phải khai báo thông tin snmp_address và snmp_community 
```
object Host "node1" { 
        import "generic-host" 
        address = "10.0.0.2" 
        vars.snmp_address = "10.0.0.2" 
        vars.snmp_community = "vtp" 
        check_command = "hostalive" 
} 
```
Theo dõi các thông tin CPU, RAM bằng các check_command của snmp 
```
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
```
Reload lại service icinga2 

    root@icinga2:~# service icinga2 reload 

Làm tương tự với node2, sau đó ta sẽ lên giao diện web để kiểm tra 

Những phần nào đang pending, ta có thể ấn vào check now để đẩy nhanh quá trình. Khi hoàn tất sẽ có kết quả như sau:

![18](https://github.com/vantuanpham95/icinga2-report/blob/master/images/18.png)
 
Như vậy chúng ta đã cài đặt và monitor thành công 2 server Linux bằng icinga2 
#### c. Cài đặt cảnh báo qua email 
Cài đặt các service ssmtp và mailutils cho icinga2 master để có thể gửi email:

    root@icinga2:~# apt-get install -y ssmtp mailutils 

Cấu hình thông số email để icinga2 có thể gửi email đi trong /etc/ssmtp/ssmtp.conf, ở đây thiết lập bằng tài khoản gmail: 
```
mailhub=smtp.gmail.com:465  
UseTLS=yes  
FromLineOverride=yes  
AuthUser=vantuanpham95@gmail.com 
AuthPass=P@ssword 
```

Kiểm tra xem icinga2 có gửi được email đi không 

    root@icinga2:~# echo "hello, world" | mail -s "Test Icinga2" svtn-tuanphamvan@vccorp.vn 

Kết quả là icinga2 đã gửi được email thành công. 

![19](https://github.com/vantuanpham95/icinga2-report/blob/master/images/19.png)

Bây giờ, ta sẽ tự tạo ra các vấn đề của các node để nhận cảnh báo bằng cách sử dụng stress: 

    root@node1:~# apt-get install stress –y 
 
Tự tạo ra vấn đề CPU: 

    root@node1:~# stress --cpu 6 

Chờ một lúc, ta sẽ thấy trên icingaweb2 nhận được các thông tin cảnh báo CPU vượt ngưỡng 

(Ngưỡng cảnh báo của icinga2 có các thông số mặc định, nhưng chúng ta cũng có thể tự định nghĩa thông qua các tham số vars.snmp_warn và vars.snmp.crit trong các file config của các node) 

![20](https://github.com/vantuanpham95/icinga2-report/blob/master/images/20.png)

![21](https://github.com/vantuanpham95/icinga2-report/blob/master/images/21.png)

Kiểm tra lại hòm mail sẽ thấy email cảnh báo được gửi bởi icinga2 thông qua tài khoản email đã được thiết lập: 

![22](https://github.com/vantuanpham95/icinga2-report/blob/master/images/22.png)

Như vậy chúng ta đã cấu hình thành công email cảnh báo trên icinga2. 
#### d. Cài đặt notifications theo service check đến từng user groups
Bài toán đặt ra là gửi cảnh báo theo RAM, DISK và CPU đến từng nhóm users có liên quan. Ở đây ta cần định nghĩa các user và user groups trong file **/etc/icinga2/conf.d/users.conf** 
```
object User "tuanpv" { 
  import "generic-user" 
  
  display_name = "Pham Van Tuan" 
  groups = [ "CPU" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  email = "tuanpauldesign01@gmail.com" 
} 
object User "tungnt" { 
  import "generic-user" 
  
  display_name = "Nguyen Thanh Tung" 
  groups = [ "CPU" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  
  email = "tuanpauldesign02@gmail.com" 
} 
  
object User "thaobtp" { 
  import "generic-user" 
  
  display_name = "Bui Thi Phuong Thao" 
  groups = [ "RAM" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  
  email = "tuanpauldesign03@gmail.com" 
} 
  
object User "tuyendt" { 
  import "generic-user" 
  
  display_name = "Dinh Thanh Tuyen" 
  groups = [ "RAM" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  
  email = "tuanpauldesign04@gmail.com" 
} 
  
object User "binhnd" { 
  import "generic-user" 
  
  display_name = "Nguyen Duy Binh" 
  groups = [ "DISK" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  
  email = "tuanpauldesign05@gmail.com" 
} 
  
object User "quynhnt" { 
  import "generic-user" 
  
  display_name = "Nguyen Thi Quynh" 
  groups = [ "CPU" ] 
  enable_notifications = true 
  states = [ OK, Warning, Critical ] 
  types = [ Problem, Recovery ] 
  
  email = "tuanpauldesign06@gmail.com" 
} 
  
  
object UserGroup "RAM" { 
  display_name = "RAM - adminsgroup" 
} 
  
object UserGroup "DISK" { 
  display_name = "DISK - adminsgroup" 
} 
  
object UserGroup "CPU" { 
  display_name = "CPU - adminsgroup" 
} 
```

Ta tạo ra 3 templates notifications theo từng dịch vụ yêu cầu trong file /etc/icinga2/templates.conf 
```
template Notification "mail-disk-notification" { 
  user_groups = [ "DISK"] 
  command = "mail-service-notification" 
  states = [ OK, Warning, Critical, Unknown ] 
  types = [ Problem, Acknowledgement, Recovery, Custom, 
            FlappingStart, FlappingEnd, 
            DowntimeStart, DowntimeEnd, DowntimeRemoved ] 
  
  vars += { 
    // notification_icingaweb2url = "https://www.example.com/icingaweb2" 
    // notification_from = "Icinga 2 Service Monitoring <icinga@example.com>" 
    notification_logtosyslog = false 
  } 
  
  period = "24x7" 
  
} 
  
template Notification "mail-ram-notification" { 
  user_groups = [ "RAM"] 
  command = "mail-service-notification" 
  states = [ OK, Warning, Critical, Unknown ] 
  types = [ Problem, Acknowledgement, Recovery, Custom, 
            FlappingStart, FlappingEnd, 
            DowntimeStart, DowntimeEnd, DowntimeRemoved ] 
  
  vars += { 
    // notification_icingaweb2url = "https://www.example.com/icingaweb2" 
    // notification_from = "Icinga 2 Service Monitoring <icinga@example.com>" 
    notification_logtosyslog = false 
  } 
  
  period = "24x7" 
  
} 
  
template Notification "mail-cpu-notification" { 
  user_groups = [ "CPU"] 
  command = "mail-service-notification" 
  states = [ OK, Warning, Critical, Unknown ] 
  types = [ Problem, Acknowledgement, Recovery, Custom, 
            FlappingStart, FlappingEnd, 
            DowntimeStart, DowntimeEnd, DowntimeRemoved ] 
  
  vars += { 
    // notification_icingaweb2url = "https://www.example.com/icingaweb2" 
    // notification_from = "Icinga 2 Service Monitoring <icinga@example.com>" 
    notification_logtosyslog = false 
  } 
  
  period = "24x7" 
  
} 
template Notification "mail-default-notification" { 
  users = [ "tuanpv"] 
  command = "mail-service-notification" 
  states = [ OK, Warning, Critical, Unknown ] 
  types = [ Problem, Acknowledgement, Recovery, Custom, 
            FlappingStart, FlappingEnd, 
            DowntimeStart, DowntimeEnd, DowntimeRemoved ] 
  
  vars += { 
    // notification_icingaweb2url = "https://www.example.com/icingaweb2" 
    // notification_from = "Icinga 2 Service Monitoring <icinga@example.com>" 
    notification_logtosyslog = false 
  } 
  
  period = "24x7" 
  
} 
``` 
Và cuối cùng là apply notification templates này tới các dịch vụ yêu cầu trong file /etc/icinga2/conf.d/notifications.conf 
```
apply Notification "disk-notification-via-email" to Service { 
  import "mail-disk-notification" 
  assign where match("*disk*", service.name) 
} 
apply Notification "ram-notification-via-email" to Service { 
  import "mail-ram-notification" 
  assign where match("*memory*", service.name) 
} 
apply Notification "cpu-notification-via-email" to Service { 
  import "mail-cpu-notification" 
  assign where match("*load*", service.name) 
} 
apply Notification "default-notification-via-email" to Service { 
import "mail-default-notification" 
  assign where host.name == "node1" 
  ignore where match("*disk*", service.name) 
  ignore where match("*disk*", service.name) 
  ignore where match("*load*", service.name) 
} 
```
Reload lại service icinga2, chúng ta lên web để kiểm tra kết quả: 

Về disk:

 ![23](https://github.com/vantuanpham95/icinga2-report/blob/master/images/23.png)
 
Về CPU: 

![24](https://github.com/vantuanpham95/icinga2-report/blob/master/images/24.png)

Về RAM: 

![25](https://github.com/vantuanpham95/icinga2-report/blob/master/images/25.png)

Như vậy chúng ta đã gửi cảnh báo email theo từng service tới các user groups thành công! 
