---
  title: Cài Web Driver cho card rời NVIDIA
  author: Nguyen Van Hung
  tags: hackintosh whatevergreen nvidia lilu
  aside:
    toc: true
---
## Giới thiệu

+ Đối với những máy dùng card rời NVIDIA dòng Maxwell và Pascal thì lúc mới cài máy sẽ chưa nhận card và sẽ rất lag, cần phải cài web driver vào thì máy mới nhận card rời.
+ Cụ thể thì là những card sau
  - NVIDIA Pascal: GeForce GTX 1030, 1050, 1050 Ti, 1060, 1070, 1070 Ti, 1080, 1080 Ti, TITAN Pascal và TITAN Xp
  - NVIDIA Maxwell: GeForce GTX 750, 750 Ti, 950, 960, 970, 980, 980 Ti, and TITAN X
+ Dòng card NVIDIA RTX mới ra hiện chưa có web driver nên chưa thể dùng cho hackintosh.
+ Chưa có Web Driver cho Mojave nên chỉ chỉ dừng lại ở High Sierra 10.13.6 không được update lên.
+ Dòng card NVIDIA Kelper sẽ chạy native với kext của Apple nên không cần cài web driver và có thể cài Mojave.

## Yêu cầu
- Cần có __Lilu + Whatevergreen__ trong Clover/kexts/Other
- Nếu làm theo hướng dẫn của mình thì đã có sẵn

## Cài NVIDIA Web Driver
__Mình sẽ giới thiệu hai cách cài cho các bạn tham khảo__

### Dùng bộ cài từ NVIDIA
- Kiểm tra build version của macOS đang dùng. Mở Terminal lên và chạy lệnh sau:

```bash
sw_vers
```

![build](/assets/images/hackintosh/webdrv/build-version.png)

+ Download bộ cài Web Driver đặt phù hợp

+ Danh sách các phiên bản web driver đã được tổng hợp. Bạn có thể download từ các nguồn sau:
  - __tonymacx86__: [https://www.tonymacx86.com/nvidia-drivers/](https://www.tonymacx86.com/nvidia-drivers/)
  - __InsanelyMac__: [https://www.insanelymac.com/forum/topic/324195-nvidia-web-driver-updates-for-macos-high-sierra-update-apr-02-2019/](https://www.insanelymac.com/forum/topic/324195-nvidia-web-driver-updates-for-macos-high-sierra-update-apr-02-2019/)
+ Download phiên bản web driver phù hợp với build version macOS của bạn
+ Chạy bộ cài web drvier (ảnh chỉ mang tính chất minh họa)

![webdrv](/assets/images/hackintosh/webdrv/webdrv.png)

+ Web Driver sẽ báo restart thì bạn đừng restart vội, còn cần chỉnh config.plist nữa.
+ Nếu không thấy bản web driver nào giống build version macOS thì hãy update macOS lên phiên bản phù hợp nhưng không được update lên Mojave hoặc thử cách phía dưới.

### Dùng nvidia-update script của Benjamin-Dobell

+ Không cần làm gì nhiều chỉ mở Terminal lên và chạy lệnh sau
```bash
bash <(curl -s https://raw.githubusercontent.com/Benjamin-Dobell/nvidia-update/master/nvidia-update.sh)
```

+ Link github của repository để ủng hộ tác giả: [https://github.com/Benjamin-Dobell/nvidia-update](https://github.com/Benjamin-Dobell/nvidia-update)

+ Cách này cực dễ làm nhưng đôi lúc có thể lỗi, vẫn khuyến khích dùng cách bên trên. Nếu bạn dùng bộ cài từ olarila thì hãy dùng cách này.

## Chỉnh sửa config.plist

+ Đừng vội restart máy, hãy mount EFI ra và mở config.plist lên bằng Clover Configurator
+ Chuyển qua tab __Boot__ và thêm boot argument sau: __nvda-drv=1__

![nvda-drv](/assets/images/hackintosh/webdrv/nvda-drv.png)

+ Chuyển qua tab __System Parameters__ và tích __NvidiaWeb__ lên

![nvidiaweb](/assets/images/hackintosh/webdrv/nvidiaweb.png)

+ Lưu config.plist lại và restart

## Tổng kết
Mình đã hướng dẫn cơ bản cách cài web driver. Đến đây gần như là PC đã có thể hoạt động ổn định.
Để chơi hackintosh lâu dài thì bạn vẫn nên qua với team đỏ AMD.

Cảm ơn NVIDIA, team Acidanthera, Benjamin Dobell và tất cả các lập trình viên đã viết kext và tool cho Hackintosh.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
