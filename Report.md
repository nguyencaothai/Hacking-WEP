# CRACKING WEP
## 1. Cách thức hoạt động của WEP
- Mỗi gói tin gửi đi sẽ được mã hóa bằng một key stream riêng biệt.
- Key stream này được tạo ra bằng cách kết hợp một IV (Initialization vector) 24 bit với password (dùng để đăng nhập vào wifi) 40 bit.
- Khi gói tin được mã hóa xong thì trước khi gửi đi, gói tin ấy sẽ được đính kèm IV ở dạng plaintext vào và cuối cùng là gửi đi đến AP.
- AP (Access point) khi nhận được gói tin có kèm IV, nó sẽ dùng IV kết hợp với password và tạo ra key stream đồng thời giải mã gói tin ấy ra. Điều này cũng tương tự ở phía client.
- Vấn đề ở đây là khi lượng traffic trong wireless tăng lên rất nhiều, tức là số lượng gói tin sẽ tăng lên nhiều dẫn tới việc sử dụng hết các IV. Hay nói cách khác sẽ có rất nhiều gói tin sử dụng chung một IV dẫn tới việc key stream trùng nhau.
## 2. Tiến hành Cracking WEP

| Type of Hadware | Name | Model/Version |
| --- | --- | --- |
| Operating System | Kali Linux | 2019.3 |
| Wireless Adapter | TP-Link| TL-WN722N/Version 1.0 |
| Access Point     | TP-Link| TD-W8901N/Version 1.0 |


### Bước 1 : Bật chế độ monitor cho Wireless Adapter
Monitor mode của wireless apdater cho phép một computer có thể capture tất cả traffic trong wireless channel mà không cần phải associate với access point.
Để bật chế độ monitor cho Wireless Adapter ta sử dụng tool **airmon-ng**

![airmon-ng](https://github.com/nguyencaothai/Hacking-WEP/blob/master/image1.png)

Trong đó **wlan0** chính là tên của Wireless Adapter trong Kali Linux. Kiểm tra chế độ monitor bằng **ifconfig wlan0mon**

![ifconfig wlan0mon](https://github.com/nguyencaothai/Hacking-WEP/blob/master/ipconfig.png)

### Bước 2 : Xác định access point có sử dụng WEP
Ở bước này, có rất nhiều tool để lựa chọn. Ta sẽ lần lượt đi qua từng tool
#### a) airoudump-ng
Để có thể sử  dụng tool này để quét các access point ta sử dụng câu lệnh sau :

![airodump-ng wlan0mon](https://github.com/nguyencaothai/Hacking-WEP/blob/master/airodump-ng-ap.png)

Ta quan sát kết quả trả về và xác định target đang sử dụng WEP

![airodump-ng wlan0mon result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/airodump-ng-ap-result.png)

#### b) kismet
Để chạy được tool kismet này, ta gõ câu lệnh **kismet** 

![kismet-ap](https://github.com/nguyencaothai/Hacking-WEP/blob/master/kismet-ap.png)

Tiếp đó, ta sử dụng một browser và truy cập vào đường link *http://localhost:2501*. Khi đó giao diện web của kismet sẽ hiện lên. Ta quan sát kết quả và xác định target

![kismet-ap-result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/kismet-ap-result.png)

#### c) CommView
CommView là một công cụ trên nền tảng Window. Ta có thể sử dụng CommView để xác định được các access point xung quanh nào đang sử dụng cơ chế bảo mật WEP.

![commview-ap-result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/commview-ap-result.png)

### Bước 3 : Bắt IV

### Bước 4 : Tìm ra mật khẩu của wifi
