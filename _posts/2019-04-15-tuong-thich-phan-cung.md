---
title: Phần cứng và Hackintosh
tags: hackintosh hardware
author: Nguyen Van Hung
aside:
  toc: true
---
# Mở đầu
Bài viết này sẽ giúp bạn trả lời cho một số câu hỏi như sau, các bạn cần có những kiến thức cơ bản về phần cứng máy tính để có thể hiểu bài này:
- PC hoặc laptop cấu hình ABCXYZ thế này có cài được hackintosh không? Cài bản nào tối ưu nhất?
- Card wifi nào support hackintosh tốt?
- Mua usb wifi/bluetooth nào support Hackintosh?
- Laptop có dùng được card rời không?

# Độ tương thích phần cứng
Hackintosh có thể cài đặt trên phần lớn các PC laptop hiện có trên thị trường. Mình sẽ đi chi tiết vào các linh kiện phần cứng cho cả PC và laptop:

## CPU
  Nếu bạn chưa biết nhiều về CPU đọc thêm trước:
    [Các đời CPU Intel](https://www.phucanh.vn/nhung-dieu-ban-can-biet-ve-cac-dong-cpu-intel-tren-may-tinh.html) và
    [Phân loại CPU Intel](https://laptop88.vn/cac-doi-cpu-cua-intel-va-giai-ma-cach-dat-ten-cpu/a63.html)

  Phần lớn các CPU của Intel từ đời 2 trở đi đều có thể chạy tốt. CPU AMD Ryzen cũng có thể cài được hackintosh nhưng mình chưa cài nhiều nên sẽ không nói nhiều về nó. macOS Mojave đã ngưng support cpu đời 2 (Sandy Bridge) nên nó chỉ dừng lại ở High Sierra.
  - __PC:__ Các CPU Intel core I, Xeon, Pentium, Celeron đều có thể cài. Đối với dòng Pentium và Celeron sẽ cần phải fakecpuid thành core i. Trừ dòng core I ra thì những dòng cpu khác đều không thể sử dụng gpu onborad đi kèm, Xeon ko có card onboard đi kèm nên xác định dùng chip trên thì phải có card rời mới cài được.
  - __Laptop:__ Các CPU Intel core I hoặc core M đời 2 trở đi đều có thể cài. CPU Intel Celeron và Pentium sẽ không thể cài vì card HD graphics onboard của mấy dòng này sẽ không hoạt động.

## GPU
  Card onboard Intel HD graphics đi kèm CPU core I đời 4 trở đi, card rời Nvida và AMD hiện có trên thị trường sẽ hoạt động với hackintosh.
  - __Onboard Graphics:__ CPU Intel đời 3 trở lên mới dùng được. Phụ thuộc vào main nếu main chỉ có cổng vga thì sẽ không thể xuất màn hình. Cài được Mojave mới nhất.
  - __Laptop:__ Hầu hết chỉ sử dụng HD graphics đi kèm CPU rất ít máy có thể sử dụng được card rời, chỉ một số máy có thể tắt được card onboard là có thể, Ví dụ điển hình: Dell M4700, Dell M4800, Dell Alianware 17R4/17R5, Thinkpad X1 Extreme, ... macOS không hỗ trợ chuyển đổi card qua lại như windows, đối với card nvidia chế độ này gọi là nvidia optimus.
  - __Nvida:__ Card rời Nvida với kiến trúc Kelper sẽ native support hackintosh (GT710, GT730, GT740, ...), cài là tự nhận và có thể cài Mojave. Đối với những card Maxwell (GTX 7XX, GTX 8XX, GTX 9XX) và Pascal (GTX 10XX) sẽ cần đến Nvidia Web Driver để có thể chạy và chỉ cài được High Sierra trở về trước. Còn dòng RTX hiện tại chưa có Web Driver sẽ không dùng được. Card kiến trúc Femi trở về trước quá cũ rồi mình không đề cập đến nữa.
  - __AMD:__ Phần lớn card AMD sẽ chạy native với macOS vì nó là card được dùng trên real mac. Card AMD sẽ chạy với native kext của Apple. Các dòng AMD RX 4XX, RX 5XX, Vega56, Vega64 đều support tốt. Còn khá nhiều dòng cũ nữa tham khảo link sau [AMD GPU HACKINTOSH](https://www.tonymacx86.com/threads/radeon-compatibility-guide-ati-amd-graphics-cards.171291/)

## Main PC
  Mình sẽ chỉ nói về main cho cpu intel. Gần như tất cả các main dành cho cpu Intel đều có thể cài. Main dòng H, B, Z, X đều có thể cài hackintosh. Vấn đề lớn nhất là nhiều main chỉ có cổng VGA và macOS thì không support cổng VGA chỉ support DVI, HDMI và Displayort, do đó nếu gặp main kiểu này bạn sẽ phải mua thêm card rời. Main Gigabyte và ASRock sẽ support tốt nhất (quan điểm cá nhân) và main MSI sẽ khó cài nhất.

## Laptop
  Laptop muốn cài hackintosh thì phải dùng CPU Intel Core I đời 2 trở lên hoặc Core M để có thể sử dụng HD Graphics. Đa số laptop trên thị trường hiện giờ đều thỏa mãn yếu tố trên.

## Ethernet
  Các kext ethernet đều đã support phần lớn các loại ethernet có tên thị trường, chỉ việc tìm kext hỗ trợ phần cứng của bạn là được.

## Wifi/Bluetooth
  - Về wifi đã có bài viết rõ ràng của anh [Nguyễn Minh Sơn](https://www.facebook.com/son01490517) ở đây [card wifi hackintosh](https://caidatmacos.com/tu-van-phan-cung/card-wifi-hackintosh/)
  - Xin chú ý nếu dùng usb wifi hay usb bluetooth thì nó sẽ gây ảnh hưởng đến việc sleep máy.
  - Usb wifi sẽ là một giải pháp tạm thời giá rẻ để sử dụng wifi trên hackintosh, bạn chỉ cần check trên trang chủ của mẫu usb wifi đó có driver cho mac hay không.
  - Một số mẫu usb wifi ổn: TP-Link TL-WN725N, TL-WN823N, Comfast WU810N, Comfast CF-811AC mấy mẫu này khá nhỏ gọn.

## Ổ cứng
  - Nên cài hackintosh trên ổ SSD để có trải nghiệm tốt nhất. Từ High Siera trở đi SSD NVME sẽ nhận native mà không cần patch gì cả.
  - Phần lớn các loại hdd, ssd trên thị trường đều có thể cài hackintosh ngoại trừ một số mẫu sau:
    + SSD Samsung PM981 sẽ không write được trong macOS
    + SSD WDC Green sẽ không format được APFS bắt buộc dùng HFS
    + SSD Samsung 970 EVO PLUS cần được update firmware mới nhất mói dùng được
  - Cá nhân mình khuyến nghị sử dụng ổ cứng của Samsung.

# Tổng kết
  Nếu máy của bạn không quá cũ thì gần như chắc chắn máy bạn có thể cài được hackintosh. Đối với PC thì với việc card trâu cày AMD bán xả giá rất rẻ bạn có thể mua một cái về cài rất dễ, cài card on sẽ khó khăn hơn. Đối với laptop thì gần như sẽ không dùng được card rời cần phải disable nó đi, wifi cũng khả năng tạch là rất cao.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
