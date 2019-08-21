---
  title: Kích hoạt âm thanh trong hackintosh với AppleALC
  author: Nguyen Van Hung
  tags: hackintosh audio AppleALC AppleHDA
  aside:
    toc: true
---

## Chuẩn bị

+ __Lilu__: [https://github.com/acidanthera/Lilu/releases](https://github.com/acidanthera/Lilu/releases)
+ __AppleALC__: [https://github.com/acidanthera/AppleALC/releases](https://github.com/acidanthera/AppleALC/releases)
+ __Clover Configurator__: [https://mackie100projects.altervista.org/download-clover-configurator/](https://mackie100projects.altervista.org/download-clover-configurator/)
+ Copy hai kext trên đến thư mục EFI/CLOVER/kexts/Other
+ Đảm bảo chưa thay đổi, động chạm gì đến kext AppleHDA gốc

## Cơ chế hoạt động

AppleALC là một kho database lớn gồm các layout, platform hỗ trợ rất nhiều loại codec. AppleALC là mã nguồn mở cho phép mọi người có thể đóng góp. Hiện tại AppleALC đã support đa số codec-id có trên thị trường và nhờ sự đóng góp của cộng thì nó lại càng lớn mạnh.

Nhờ có Lilu hỗ trợ việc patch on-the-fly, nên AppleALC sẽ patch, thay đổi layout và platform trong kext AppleHDA gốc lúc clover load. Do đó yêu cầu bạn chưa thay đổi gì kext AppleHDA gốc. Vì thế bạn sẽ không phải lo lắng việc update phải patch lại AppleHDA như trước kia.

## Xác định audio codec

Có nhiều cách để xác định, làm theo một trong số những cách sau:

+ Dùng Aida64 Extreme tạo file report để xem (Nên dùng cách này, lỡ quên thì mở ra xem hoặc có thể gửi cho người khác để giúp đỡ)

![codec](/assets/images/hackintosh/audio/aida.png)

+ Xem thông tin main trên trang chủ các hãng sản xuất (dành cho PC, laptop thường không ghi rõ)

![codec](/assets/images/hackintosh/audio/web.png)

+ Dùng DCPIManager. [Download](https://sourceforge.net/projects/dpcimanager/)

![codec](/assets/images/hackintosh/audio/dcpi.png)

+ Dùng Hackintool, bạn có thể xem luôn những layout-id có thể dùng. [Download](http://headsoft.com.au/download/mac/Hackintool.zip)

![codec](/assets/images/hackintosh/audio/hackintool.png)

## Xác định layout-id

+ Bạn có thể xem danh sách các layout-id có sẵn cho audio codec ngay trên github [https://github.com/acidanthera/AppleALC/wiki/Supported-codecs](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)

![codec](/assets/images/hackintosh/audio/layout-web.png)

+ Như lúc nãy mình nói có thể dùng Hackintool và lựa chọn

![codec](/assets/images/hackintosh/audio/layout-hack.png)

+ Để xem chi tiết hơn về các layout-id:
  - Vào [AppleALC Resources](https://github.com/acidanthera/AppleALC/tree/master/Resources) -> chọn codec -> Info.plist
  - Link tương ứng: __https://github.com/acidanthera/AppleALC/blob/master/Resources/\<codec\>/Info.plist__.
  - Xem thông tin của các layout, có thể bạn sẽ thấy được tên laptop của mình trong đó. Rồi chọn layout có thể gần gần với máy bạn, có thể là cùng hãng sản xuất chả hạn.

  - Ví dụ laptop của mình thinkpad T470, codec ALC298 thì vào [https://github.com/acidanthera/AppleALC/blob/master/Resources/ALC298/Info.plist](https://github.com/acidanthera/AppleALC/blob/master/Resources/ALC298/Info.plist). Trong này mình thấy có 2 layout-id có vẻ gần với máy là 29 (thinkpad X270) và 47 (thinkpad T470P).

  ![layout](/assets/images/hackintosh/audio/layout-alc.png)

## Chỉnh sửa layout-id trong config.plist

1. Dùng Clover Configurator mount EFI ra
2. Mở config.plist bằng Clover Configurator
3. Chọn tab Devices
4. Phần Audio Inject điền layout-id mà bạn đã xác định vào
5. Lưu config.plist lại và restart

![layout](/assets/images/hackintosh/audio/cc-layout.png)

Nếu không kích hoạt được âm thanh hãy thử các layout-id khác.

Nếu thử hết layout-id mà máy bạn không có âm thanh thì xin chia buồn bạn sẽ cần phải tự patch AppleHDA (rất khó)

## Kinh nghiệm chọn layout-id của mình
  1. Các layout-id từ 1 đến 10 sẽ do Mirone/Toleda làm có chứa các patch cơ bản và chung cho nhiều máy, hãy thử những layout này trước. Các layout-id từ 11 đến 99 sẽ do sự đóng góp từ cộng đồng.
  2. Nếu có layout-id với tên máy của bạn thì hãy chọn nó.
  3. Ưu tiên thử các layout cùng hãng sản xuất trước
  4. Đối với PC mình thường chọn layout-id 1, gần như các codec PC trong AppleALC đều có layout này
  5. Đối với laptop thì hãy thử layout-id 3 trước tiên
  6. Một số codec hay xảy ra lỗi nhất ALC255, ALC256, ALC292, nếu bạn dùng codec này thì cầu mong nó không lỗi vặt đi.

## Sửa lỗi mất âm thanh sau khi sleep

__Nếu bị lỗi trên thì hãy làm theo cách dưới đây, còn không hãy bỏ qua bước này.__

+ Download:
  - __CodecCommander.kext__: [https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/](https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/)
  - __Kext Utility__: [http://cvad-mac.narod.ru/index/0-4](http://cvad-mac.narod.ru/index/0-4)

+ Cài đặt kext trên vào __/System/Library/Extensions__ (viết tắt __SLE__) hoặc __/Library/Extensions__ (viết tắt __LE__)
  - __SLE__: Chạy Kext Utility và kéo thả kext vào, nhập mật khẩu và chờ đợi.
  - __LE__: Dùng command line để cài, mở terminal lên và chạy lệnh sau

  ```bash
    # Copy kext tới /Library/Extensions
    sudo cp -R <kéo thả kext vào terminal> /Library/Extensions

    # rebuild kext
    sudo kextcache -i /
  ```
+ Khởi động và test

## Tổng kết

Trên đây mình đã hướng dẫn chi tiết cách đơn giản nhất để kích hoạt được âm thanh trong hackintosh.

Cảm ơn [Acidanthera team](https://github.com/acidanthera), [Rehabman](https://github.com/RehabMan), Mirone, Toleda, ... và tất cả những người đã đóng góp cho cộng đồng hackintosh.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
