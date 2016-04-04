# Tổng Quan DHCP
## Mục lục

[1.Khái Niệm] (#1)

[2.Vai Trò] (#2)

[3.Các loại bản tin DHCP] (#3)

- [a.DHCP Discover] (#3a)

- [b.DHCP Offer] (#3b)
  
- [c.DHCP Request] (#3c)

- [d.DHCP Ack/Nack] (#3d)

- [e.DHCP Decline] (#3e)

- [f.DHCP Release] (#3f)
  
[4.DHCP Header] (#4)

[5.Cách thức hoạt động] (#5)

[6.Tham Khảo] (#6)

<a name="1"></a>
### 1.Khái Niệm
- Một giao thức cấu hình tự động địa chỉ IP cho các máy trạm.
- DHCP có 2 version: cho IPv4 và IPv6.

<a name="2"></a>
### 2.Vai Trò
- Cấu hình động các máy.
- Cấu hình IP cho các máy một cách liền mạch
- Tập chung quản trị thông tin về cấu hình IP.
- DHCP còn cung cấp thông tin cấu hình khác, cụ thể như DNS.

<a name"3"></a>
### 3.Các loại bản tin DHCP

<a name"3a"></a>
#### a.DHCP Discover
- Khi 1 client muốn gia nhập mạng, nó sẽ broadcast 1 gói tin dhcp discover tới dhcp server để yêu cầu cấp thông tin địa chỉ ip.
- Ip nguồn trong gói là 0.0.0.0.

<a name="3b"></a>
#### b.DHCP Offer
- Xuất phát từ DHCP server sau khi nhận được gói Discover của client.
- Gói tin bao gồm thông tin IP đề nghị cấp cho client như: IP address, Subnet Mask, Gateway...
- Có thể sẽ có nhiều DHCP server cùng gửi gói tin này, Client sẽ nhận và xử lý gói Offer đến trước.

<a name="3c"></a>
#### c.DHCP Resquest
- Broadcast từ client khi nhận được gói DHCP Offer.
- Nội dung gói tin: xác nhận thông tin IP sẽ nhận từ server.

<a name="3d"></a>
#### d.DHCP Ack/Nack
*DHCP Ack*
- Broadcast bởi DHCP server đến DHCP client xác nhận thông tin từ gói DHCP Request.
- Tất cả thông tin cấu hình IP sẽ được gửi đến cho client và kết thúc quá trình cấp phát IP.

*DHCP Nack*:
Broadcast từ server, khi server từ chối gói DHCP Request.

<a name="3f"></a>
#### f.DHCP Decline
Broadcast từ client nếu client từ chối IP đã được cấp.

<a name="3e"></a>
#### e.DHCP Release
- Được gửi bởi DHCP client khi client bỏ địa chỉ IP và hủy thời gian sử dụng còn lại.
- Đây là gói tin unicast gửi trực tiếp đến DHCP server cung cấp IP đó.

<a name="4"></a>
### 4.DHCP Header
<img src="http://i.imgur.com/9DNIExw.jpg" />

##### a.Opcode
- Dung lượng: 8 bits
- Vai trò: Thể hiện loại gói tin DHCP.
- Value 1: các gói tin request.
- Value 2: các gói tin reply.

##### b.Hardware type
- Dung lượng: 8 bits
- Vai trò: Quy định cụ thể loại hardware.
- Value 1: Ethernet (10Mb)
- Value 6: IEEE 802
- Value 15: Frame Relay
- Value 16: ATM
- Value 17: HDLC
- Value 18: Fibre Chanel

##### c.Hardware length
- Dung lượng: 8 bits
- Ý nghĩa: Quy định cụ thể độ dài của địa chỉ hardware

##### d.Hop counts
- Dung lượng: 8 bits
- Dùng cho relay agents

##### e.Transaction Identifier
- Dung lượng: 32 bits
- Được tạo bởi client, dùng để liên kết giữa request và replies của client và server.

##### f.Number of seconds
- Dung lượng: 16 bits
- Quy định số giây kể từ khi client bắt đầu thuê hoặc xin cấp lại IP
- Nó cũng có thể được dùng bởi dhcp server đang bận để ưu tiên trả lời những yêu cầu nổi bật của clients.
##### g.Flags
<img src="http://i.imgur.com/on5i4m8.png" />

- Dung lượng: 16 bits
- B, broadcast: 1 bits = 1 nếu client không biết được ip trong khi đang gửi yêu cầu.

##### h.Client IP address
- Dung lượng: 32 bits
- Client sẽ đặt IP của mình trong trường này nếu và chỉ nếu nó đang có IP hay đang xin cấp lại IP, không thì mặc định = 0

##### I.Your IP address
- Dung lượng: 32 bits.
- IP được cấp bởi server để đăng kí cho client.

##### K.Server IP address
- Dung lượng: 32 bits.
##### L.Gateway IP address
- Dung lượng: 32 bits.
- Sử dụng trong relay agent.

##### L.Client hardware address
- Dung lượng: 16 bytes.
- Địa chỉ lớp 2 của client, dùng để định danh.

##### M.Server host name
- Dung lượng: 64 bytes
- Khi server gửi gói tin offer hay ack thì sẽ đặt tên của nó vào trường này, nó có thể là nickname hoặc tên miền dns.

##### N.Boot filename
- Dung lượng: 128 bytes.
- Sử dụng bời client để yêu cầu loại tập tin khởi động cụ thể trong gói tin discover.
- Sử dụng bởi server để chỉ rõ toàn bộ đường dẫn, tên file của file khởi động trong gói tin offer.

<a name="5"><a>
### 5.Cách thức hoạt động
<img src="http://i.imgur.com/Y6z1Gkp.png" />

Để nhận được IP từ DHCP server, DHCP client phải khởi tạo giao tiếp với server với 1 loạt gói tin liên tiếp nhau.
Quá trình này diễn ra qua 4 bước chính:
- Client broadcast yêu cầu thuê đia chỉ IP trên hệ thống mạng (DHCPDiscover).
- Nhiều DHCP server có thể nhận thông điệp và chuẩn bị IP cho client.Nếu máy chủ có cấu hình hợp lệ cho client, 
nó gửi thông điệp "DHCP Offer" chứa địa chỉ MAC của khách, địa chỉ IP, subnet mask, địa chỉ IP của server và thời gian cho thuê đến client.
- Khi client nhận được cái thông DHCP Offer nó sẽ chọn 1 trong các địa chỉ IP, sau đó sẽ gửi DHCP Request để yêu cầu IP tương ứng với DHCP server đó.
- Cuối cùng, DHCP Server xác nhận lại với client bằng thông điệp DHCP Acknowlegde.Ngoài ra server còn gửi kèm những thông tin bổ sung như địa chỉ gateway mặc định, địa chỉ DNS Server.

<a name="6"></a>
### 6.Tham Khảo
1. http://www.networksorcery.com/enp/protocol/dhcp.htm#Client hardware address
2. http://www.tcpipguide.com/free/t_DHCPMessageFormat.htm
3. http://vdo.vn/cong-nghe-thong-tin/cac-khai-niem-co-ban-ve-dhcp.html



