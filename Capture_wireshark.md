#Capture dhcp with wireshark
## Mục lục
[1.Khởi động wireshark] (#1)

[2.Giải phóng và cấp mới IP] (#2)

[3.Lọc các gói tin DHCP] (#3)

[4.Phân tích các gói tin DHCP] (#4)

<a name="1"></a>
### 1.Khởi động wireshark

<a name="2"></a>
### 2.Giải phóng và cấp mới IP

<a name="3"></a>
### 3.Lọc các gói tin DHCP
- Gõ "bootp" vào ô filter.

- Hoặc bạn có thể vào trực tiếp 1 gói tin dhcp bất kì để lọc theo boostrap protocol

<a name="4"></a>
### 4.Phân tích các gói tin DHCP
#### a.DHCP discovery
<img src="http://i.imgur.com/LXY7EyH.png" />
Đây là gói tin broadcast gửi từ client đến các servers.
- 1.source mac(client) des mac(servers).
- 2.source ip (client) = 0.0.0.0 do lúc này client chưa có ip, des ip servers =255.255.255.255 do đây là bản tin broadcast.
- 3.source port=68(client) và des port=67(server)

`DHCP header`

- 4.Loại gói tin:DHCP discovery
- 5.IP được client yêu cầu cấp phát
- 6.Hostname của client

#### b.DHCP offer
<img src="http://i.imgur.com/kSf1YlC.png" />
Đây là gói tin unicast gửi từ server đến client
- 1.source mac(server) des mac(client).
- 2.source ip (server) và des ip(client).
- 3.source port=67(server) des port=68(client).

`DHCP header`

- 4.ip client trong gói tin header
- 5.Loại gói tin:DHCP offer
- 6.Định danh dhcp server:chính là ip của server
- 7.Thời gian cho client thuê
- 8.Subnet mask cấp cho client
- 9.default gateway cấp cho client
- 10.DNS cấp cho client

#### c.DHCP Request
<img src="http://i.imgur.com/eDlTpxi.png" />
Đây cũng là gói tin broadcast gửi từ client đến các servers.Nó có thêm trường Client Fully Qualified Domain Name trong dhcp header


#### d.DHCP Ack
<img src="http://i.imgur.com/y9Jztbq.png" />
Đây cũng là gói tin unicast gửi từ server đến client để xác nhận lại các thông tin đã cấp cho client
