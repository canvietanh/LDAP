#Tìm hiểu LDAP

##A. Lý thuyết về LDAP

###I. Định nghĩa
- Giúp tích hợp dữ liệu người dùng trên nhiều nền tảng khác nhau, nhiều hệ thống khác nhau. 
- Là 1 giao thức truy cập nhanh các dịch vụ thư mục, tìm và truy nhập nhanh các thông tin dạng thư mục trên server, giao thức dạng client-server để truy cập dịch vụ thư mục
- Chạy trên TCP/IP hoặc dịch vụ hướng kết nối khác
- Có nhiều LDAP server khác nhau như OpenLDAP, OpenDS....

###II. Thao tác trong LDAP
LDAP có 9 thao tác (Operation) cơ bản, chia ra làm 3 nhóm thao tác chính:

- Interrogation operations: search, compare.
- Update operations: add, delete, modify, modify DN (rename)
- Authentication and control operations: bind, unbind, abandon.
 - Thao tác bind cho phép client định danh mình với thư mục thông qua việc cung cấp chứng chỉ xác thực và đồng nhất.
 - Thao tác unbind cho phép hủy bỏ phiên làm việc hiện thời
 - Thao tác abandon :Cho phép client biết rằng một số thao tác trước không còn được quan tâm tới kết quả.


###III. Tiến trình hoạt động của LDAP:

  1. Client mở kết nối TCP đến LDAP server và thực hiện 1 thao tác bind. Thao tác bind bao gồm tên của directory entry.và uỷ nhiệm thư sẽ được sử dụngtrong quá trình xác thực, uỷ nhiệm thư thông thường là pasword nhưng cũng cóthể là chứng chỉ điện tử dùng để xác thực client.
 
  2. Sau khi thư mục có được sự xác định của thao tác bind, kết quả của thao tác bind được trả về cho client. Client sẽ phát ra các yêu cầu tìm kiếm.

  3. Server thực hiện xử lý và trả về kết quả cho client

  4. Server gửi thông điệp kết thúc việc tìm kiếm.

  5. Client phát ra yêu cầu unbind với yêu cầu này thì server sẽ hiểu là client muốn hủy bỏ kết nói.

  6. Server đóng kết nối.

###IV. File Ldif.
####1. Định nghĩa file Ldif
- Ldif (ldap interchange format) được định nghĩa trong RFC 2849,là 1 chuẩn định dạng file text lưu trữ những thông tin cấu hình LDAP và nội dung như mục.
- File ldif thương được sử dụng để import dữ liệu mới vào trong directory của bạn hoặc thay đổi dữ liệu đã có. Dữ liệu trong file ldif cần phải tuân thủ theo 1 liaatj có trong schema của Ldap directory
- Schema là 1 loại dữ liệu đã được định nghĩa trước trong directory của bạn, Mọi thành phần được thêm hoặc thay đổi trong directory sẽ được kiểm tra lại trong schema
- Giải pháp Import dữ liệu lớn vào LDAP. Nếu dữ liệu được lưu trong excel khoảngvài chục ngàn mẫu tin, viết tool chuyển thành định dạng trên rồi import vào LDAP
Server
 
Những yêu cầu khi khai báo nội dung file LDIF:

- Lời chú giải trong file LDIF được gõ sau dấu # trong một dòng
- Thuộc tính được liệt kê phía bên trái của dấu (:) và giá trị được biểu diễn bên phải. Dấu đặc biệt được phân cách với giá trị bằng dấu cách trắng
- Thuộc tính dn định nghĩa duy nhất một DN xác định trong entry đó

Một entry: Là tập hợp của các thuộc tính, từng thuộc tính này mô tả một nét đặc trưng tiêu biểu của một đối tượng. Một entry bao gồm nhiều dòng :
 
- dn: distinguished name -là tên của entry thư mục, tất cả được viết trên một dòng.
- Sau đó lần lượt là các thuộc tính của entry, thuộc tính dùng để lưu giữ dữliệu. Mỗi thuộc tính trên một dòng theo định dạng là ” kiểu thuộc tính : giátrị thuộc tính”
- Thứ tự các thuộc tính không quan trọng, tuy nhiên để dễ đọc được thông tinchúng ta nên đặt các giá trị objectclass trước tiên và nên làm sao cho cácgiá trị của các thuộc tính cùng kiểu ở gần nhau

####2. Cấu trúc tập tin ldif:
Thông thường một file LDIF sẽ theo khuôn dạng sau: 

- Mỗi một tập entry khác nhau được phân cách bởi một dòng trắng
- Sự sắp đặt “tên thuộc tính : giá trị” 
- Một tập các chỉ dẫn cú pháp để làm sao xử lý được thông tin

Một số các thuộc tính cơ bản trong file ldif:

- dn:  Distinguished Name : tên gọi phân biệt
- c: country - 2 ký tự viết tắt của tên 1 nước
- o: organization - tổ chức
- ou: organization unit - đơn vị tổ chức
- objectClass: Mỗi giá trị objectClass hoạt động như 1 khuôn mẫu cho các dữ liệu được lưu trữ trong 1 entry. Nó định nghĩa 1 bộ các thuộc tính phải trình bày trong 1 entry.
- givenName: tên
- uid: id người dùng
- cn: common name-tên thường gọi
- telephone Number: số điện thoại
- sn: surname-họ
- userPassword: mật khẩu người dùng
- mail: địa chỉ mail
- facsimileTelephoneNumber: số phách
- createTimeStamp: thời gian tạo ra entry này
- creatorsName: tên người tạo ra entry này
- pwdChangedTime: thời gian thay đổi mật khẩu
- entryUUID: id của entry

###V. Ứng dụng LDAP
Bằng cách kết hợp các thao tác LDAP đơn giản này. Thư mục client có thể thực hiện các thao tác phức tạp như các ví dụ sau đây . 

####1. Lưu trữ
Môt chương mail có thể thực hiện dùng chứng chỉ điện tử chứa trong thư mục trên
server LDAP để kí, bằng cách gởi yêu cầu tìm kiếm cho LDAP server , LDAP server gởi lại cho client chứng chỉ điện tử của nó sau đó chương trình mail dùng chứng chỉ điện tử để kí và gởi cho Message sever. Nhưng ở góc độ người dùng thì tất cả quá trình trên đều hoạt động một cách tự động và người dùng không phải quan tâm 

<img src="http://i.imgur.com/dtQd0St.png">

####2. Quản lý thư
Netscape Message server có thể sử dụng LDAP directory để thực hiện kiểm tra các mail. Khi một mail đến từ một địa chỉ, messeage server tìm kiếm địa chỉ email trong thư mục trên LDAP server lúc này Message server biết được hợp thư người sử dụng có tồn tại và nhận thư. 

<img src ="http://i.imgur.com/Iq95fPj.png">

####3. Xác thực
Dùng LDAP xác thực một user đăng nhập vào một hệ thống qua chương trình thẩm tra, chương trình thực hiện như sau đầu tiên chương trình thẩm tra tạo ra một đại diện để xác thực với LDAP thông qua (1) sau đó so sánh mật khẩu của user A với thông tin chứa trong thư mục. Nếu so sánh thành công thì user A đã xác thực thành công 

<img src ="http://i.imgur.com/pCkRimR.png">

##B. Lab LDAP
###I. Chuẩn bị
Ta chuẩn bị 2 máy cài ubuntu server 14.04: Test1, Test2 , mỗi máy chỉ có 1 user đăng nhập là user root theo mô hình như sau:

<img src ="http://i.imgur.com/NzR8Spn.png">

###II. Cài đặt
####1. Cài đặt DHCP
#####a. Test1
Sử dụng lênh sau để update
    
    apt-get update

Cài đặt DHCP:

    apt-get install isc-dhcp-server -y 

Chỉnh sửa file config /etc/dhcp/dhcpd.conf
 
Sửa dòng 16: chỉnh domain name

    option domain-name "canvietanh.com";

Sửa dòng 17: ip domain name server

    option domain-name-servers 20.20.20.140;

Sửa dòng 24:  bỏ comment

    authoritative;

Thêm vào cuối file

    subnet 20.20.20.0 netmask 255.255.255.0 {
        #option routers 10.0.0.1;
        option subnet-mask 255.255.255.0;
        range dynamic-bootp 20.20.20.10 20.20.20.100;
    }

Do card VMnet ko ra mạng được nên ta chỉ cấp ip động và dns cho máy client.

Start dịch vụ:

    initctl start isc-dhcp-server 


#####b. Test 2 
Sửa file /etc/network/interfaces như sau:

    auto eth0
    iface eth0 inet static
    address 10.145.25.217
    netmask 255.255.255.0
    gateway 10.145.25.1


    auto eth1
    iface eth1 inet dhcp

Test dhcp
 
    ifdown -a && ifup -a

Ta sẽ thấy máy 2 được cấp ip và dhs tương ứng

 <img src ="http://i.imgur.com/aHjwdJx.png">

####2. Cài đặt DNS
#####a. Test1

Cài đặt DNS:

    apt-get install bind9 bind9utils -y

Thêm zone vào file /etc/bind/name.config.local như sau:


    zone "canvietanh.com" {
        type master;
        file "/etc/bind/db.canvietanh.com";
    };
    zone "20.20.20.in-addr.arpa" {
        type master;
        file "/etc/bind/db.20.20.20";
    };

Tạo file /etc/bind/db.canvietanh.com

    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     test1.canvietanh.com. root.canvietanh.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      test1.canvietanh.com.
    @       IN      A       20.20.20.140
    test1   IN      A       20.20.20.140

Tạo file /etc/bind/db.20.20.20

    ;
    ; BIND reverse data file for local loopback interface
    ;
    $TTL    604800
    @     IN      SOA     test1.canvietanh.com. root.canvietanh.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      test1.canvietanh.com.
    @       IN      A       255.255.255.0
    @       IN      PTR     canvietanh.com.
    140     IN      PTR     test1.canvietanh.com.

Sửa file /etc/bind/name.conf.option

     forwarders {
                8.8.8.8;
                8.8.4.4;
        };

Cụ thể về DNS các bạn có thể tham khảo bải sau: https://github.com/canvietanh/Tim-hieu-DNS

#####b. Test 2 
Sử dụng lệnh nslookup và ping google.com để kiểm tra:

<img src ="http://i.imgur.com/cmcG1K2.png">

####3. Cài đặt LDAP
#####a. Test1
Sửa file /etc/hosts

    127.0.1.1       test1.canvietanh.com    test1

Thực hiện cài đặt

    apt-get install slapd ldap-utils -y

Trong quá trình cài đặt bạn nhập pass cho LDAP admin
ở đây của mình là "canvietanh"

Tạo file base.ldif như sau

    dn: ou=people,dc=canvietanh,dc=com
    objectClass: organizationalUnit
    ou: people

    dn: ou=groups,dc=canvietanh,dc=com
    objectClass: organizationalUnit
    ou: groups 

Chạy lệnh:
    
    ldapadd -x -D cn=admin,dc=canvietanh,dc=com -W -f base.ldif 

Tạo file /etc/ldap/ldapuser.ldif chứa thông tin user ubuntu khởi tạo như sau:

    dn: uid=ubuntu,ou=people,dc=canvietanh,dc=com
    objectClass: inetOrgPerson
    objectClass: posixAccount
    objectClass: shadowAccount
    cn: ubuntu
    sn: ubuntu
    userPassword: {SSHA}lXSBQwX548zSWtcTWgFsWmUuYXxMssOF
    loginShell: /bin/bash
    uidNumber: 1000
    gidNumber: 1000
    homeDirectory: /home/ubuntu

    dn: cn=ubuntu,ou=groups,dc=canvietanh,dc=com
    objectClass: posixGroup
    cn: ubuntu
    gidNumber: 1000
    memberUid: ubuntu

- Với trường userpassword là password đã được mã hóa với câu lệnh **slappasswd** bạn nhập vào 1 password và nó sẽ trả ra pass được mã hóa

Thực hiện lệnh sau để add user và LDAP

     ldapadd -x -D cn=admin,dc=canvietanh,dc=com-W -f ldapuser.ldif 


#####b. Test 2 
Cài đặt LDAP client

    apt-get install libnss-ldap libpam-ldap ldap-utils -y

Sau đó thực hiện lần lượt như sau:

<img src ="http://i.imgur.com/lFaIe4J.png">

<img src ="http://i.imgur.com/OYrlzps.png">

<img src ="http://i.imgur.com/YLHNUSp.png">

<img src ="http://i.imgur.com/LXCk3CL.png">

<img src ="http://i.imgur.com/lhmyfwv.png">

<img src ="http://i.imgur.com/cQVmjTC.png">

<img src ="http://i.imgur.com/LOCw3yk.png">

Chỉnh sửa file sau:


- file /etc/nsswitch.conf

dòng 7: thêm:

    passwd:     compat ldap
    group:     compat ldap
    shadow:     compat ldap

-file /etc/pam.d/common-password

dòng 26: bỏ 'use_authtok' )

    password     [success=1 user_unknown=ignore default=die]     pam_ldap.so try_first_pass

- file /etc/pam.d/common-session

thêm vào cuối

    session optional        pam_mkhomedir.so skel=/etc/skel umask=077

Khởi động lại

    /etc/init.d/libnss-ldap restart 

sau đó thực hiện đặng nhập vào máy 2 sử dụng user ubuntu vs password là canvietanh

<img src ="http://i.imgur.com/AYtfyYt.png">
