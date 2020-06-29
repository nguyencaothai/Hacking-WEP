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
Trước khi thực hiện bắt các IV, ta cần đề phòng trong trường hợp lượng traffic trong mạng wireless này quá ít, tức là ta không thể thu thập đủ số lượng IV để crack được WEP. Để giải quyết vấn đề này ta sẽ sử dụng tool **aireplay** để replay các arp request. Bất cứ khi nào **aireplay** phát hiện một gói tin arp trong mạng wireless này, nó sẽ lập tức capture gói tin arp này và gửi đi gửi lại hàng loạt các gói tin arp tương tự đến cho access point. Access point một khi nhận được nó phải response một packet với một new IV. Từ đó làm gia tăng lượng traffic trong mạng cũng như số lượng IV. Tuy nhiên ta lưu ý vì lúc này ta vẫn chưa kết nối được vào access point, tức là access point sẽ "ignore" các gói tin từ wireless adapter gửi tới. Do đó bước đầu tiên ta phải thực hiện associate với access point để mà access point sẽ nhận được các packet từ wireless adapter của ta gửi tới.

![aireplay-ng](https://github.com/nguyencaothai/Hacking-WEP/blob/master/aireplay.png)

Trong đó :
- -1 là fake authentication
- -a là Mac address của access point
- -h là Mac address của wireless adapter
- wlan0mon là tên của wireless adapter ở monitor mode

Tiếp theo ta sẽ chạy **aireplay** để mà thực hiện replay các arp request

![aireplay-arp](https://github.com/nguyencaothai/Hacking-WEP/blob/master/aireplay-arp.png)

Trong đó :
- -3 là arp request replay
- -b là Mac address của access point
- -h là Mac address của wireless adapter
- wlan0mon là tên của wireless adapter ở monitor mode

Sau khi đã xong hai bước trên, ta tiến hành bắt IV bằng các tool airodump, kismet và CommView
#### a) airodump
Để tiến hành bắt IV với airodump, ta chạy câu lệnh sau đây 

![airodump-ng-IV](https://github.com/nguyencaothai/Hacking-WEP/blob/master/airodump-ng_IV.png)

Trong đó :
- --bssid là Mac address của acess point
- --channel là channel mà access point đang hoạt động
- --write là file để lưu các gói tin IV
- wlan0mon là tên của wireless adapter ở monitor mode

![airodump-ng-IV-result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/airodump-ng-IV-result.png)

Ta thấy airodump đã xuất ra một file log có định dạng là **.cap**

#### b) kismet
Khi ta chạy kismet ở **bước 2** để quan sát các access point xung quanh thì ngay lúc này kismet cũng đã bắt đầu thu thập toàn bộ các packet của tất cả các access point xung quanh nó bất kể access point đó đang dùng cơ chế bảo mật gì. Tuy nhiên file log của kismet để lưu trữ các packet này có định dạng là **.kismet** và định dạng này không thể làm việc được với các tool khác. Do đó ta cần cấu hình cho kismet sẽ lưu file log dưới dạng **.pcapng**. Ta tiến hành cấu hình trong **/etc/kismet/kismet_logging.conf**

![kismet-logging-config](https://github.com/nguyencaothai/Hacking-WEP/blob/master/kismet-logging-config.png)

Lưu cấu hình và chạy lại kismet như ở bước 2. Khi đó xuất hiện hai file log để lưu tất cả các gói tin mà kismet bắt được với 2 định dạng lần lượt là **.kismet** và **.pcapng**.
Tiếp theo ta sẽ sử dụng Wireshark đọc file **.pcapng** này và export ra một file **.pcap**

![kismet-IV-result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/kismet-IV-result.png)

![kismet-convert-pcap](https://github.com/nguyencaothai/Hacking-WEP/blob/master/kismet-convert-pcap.png)

#### c) CommView
Khi bật CommView lên thì có giao diện như sau:

![commview](https://github.com/nguyencaothai/Hacking-WEP/blob/master/commview.png)

- Bước 1: Nhấp chuột chọn “Capture Data Packet”
- Bước 2: Trong thẻ **Rules** ta tiến hành thiết lập để commview chỉ bắt các gói tin của một access point duy nhất. Ở đây là access point đang sử dụng WEP

![commview-rules-config](https://github.com/nguyencaothai/Hacking-WEP/blob/master/commview-rules-config.png)

- Bước 3: Set up logging của Commview

![commview-logging-config](https://github.com/nguyencaothai/Hacking-WEP/blob/master/commview-logging-config.png)

Trong đó :
- Maximum directory size : Tổng dung lượng của các file log chứa trong folder LOGS mặc định của CommView. Nếu vượt quá dung lượng này thì CommView sẽ tiến hành xóa các file log cũ.
- Average log file size : Kích thước xấp xỉ của một file log.
Ta nên chọn kích thước file log tối đa là 100MB để hạn chế việc có quá nhiều file log và sẽ gây vất vả trong việc chuyển đổi file .cap sau này.
- Bước 4 : Thiết lập channel phù hợp với access point target.
- Bước 5 : Click vào "Start Caputre"

![commview-IV-result](https://github.com/nguyencaothai/Hacking-WEP/blob/master/commview-IV-result.png)

Sau khi thu thập đủ IV, tron giao diện **Log Viewer**, ta load file log của commview lên. Sau đó ta tiến hành xuất các files log đang ở định **.ncf** ra thành file **.cap**

### Bước 4 : Tìm ra mật khẩu của wifi
