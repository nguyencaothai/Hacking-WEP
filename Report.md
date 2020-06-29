# CRACKING WEP
## 1. Các thức hoạt động của WEP
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


### Bước 1 : Tiến hành bật chế độ monitor cho Wireless Adapter
Monitor mode của wireless apdater cho phép một computer có thể capture tất cả traffic trong wireless channel mà không cần phải associate với access point.
Để bật chế độ monitor cho Wireless Adapter sử dụng tool **airmon-ng**

![airmon-ng](https://github.com/nguyencaothai/Hacking-WEP/blob/master/image1.png)

Trong đó **wlan0** chính là tên của Wireless Adapter trong Kali Linux. Tiến hành kiểm tra
### Bước 2 : Xác định wifi đang sử dụng loại bảo mật WEP
Ở bước này chúng ta có nhiều tool để lựa chọn. Đầu tiên ta sẽ sử dụng airodump-ng.
### Bước 3 : Tiến hành bắt tất cả gói tin của wifi đó
### Bước 4 : Tìm ra mật khẩu của wifi
