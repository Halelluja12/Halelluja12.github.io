---
  title: Cài đặt Clover vào ổ cứng
  author: Nguyen Van Hung
  tags: hackintosh post-install clover kext
  aside:
    toc: true
---

Phần này mình sẽ hướng dẫn cài và tinh chỉnh clover lên ổ cứng để bạn không cần usb mồi nữa. Đối với PC thì làm đến này là đã có thể dùng ổn định. Còn laptop thì vẫn còn một chặng đường dài nữa.

## Vô hiệu hóa Gatekeeper
Mở Terminal lên và chạy lệnh sau và nhập password:
```bash
sudo spctl --master-disable
```
Chạy lệnh trên để mở khóa cho phép cài đặt phần mềm bên thứ ba vào macOS. Nếu không dùng lệnh này khi muốn mở phần mềm gì đó bạn cần chuột phải và chọn __Open__ sau đó sẽ không cần làm lại.

## Cài Clover UEFI lên ổ cứng

Để bạn có thể boot và macOS mà không cần usb mồi thì cần cài clover vào phân vùng efi của ổ cứng.

### Sao chép clover usb vào efi ổ cứng

Clover usb đã boot được và có sẵn config, drivers64UEFI và một số kext cơ bản chép vào ổ cứng luôn sau chỉnh sửa cho đỡ mất công.

Download Clover Configurator mới nhất theo link sau
[https://mackie100projects.altervista.org/download-clover-configurator/](https://mackie100projects.altervista.org/download-clover-configurator/)

Mở Clover Configurator lên -> Mount EFI -> Mount Partition
![cc](/assets/images/hackintosh/post-install/cc.png)

Mount cả efi usb và efi ổ cứng ra sau đó copy thư mục /EFI/CLOVER trong USB qua vị trí tương ứng trên phân vùng EFI ổ cứng

### Chạy clover installer
Download phiên bản __clover efi__ mới nhất
[https://sourceforge.net/projects/cloverefiboot/](https://sourceforge.net/projects/cloverefiboot)

Thực hiện các bước sau

|----|----|
|![clover](/assets/images/hackintosh/post-install/1.png)|![clover](/assets/images/hackintosh/post-install/2.png)|
|![clover](/assets/images/hackintosh/post-install/3.png)|![clover](/assets/images/hackintosh/post-install/4.png)|
|![clover](/assets/images/hackintosh/post-install/5.png)|![clover](/assets/images/hackintosh/post-install/6.png)|
|![clover](/assets/images/hackintosh/post-install/7.png)|![clover](/assets/images/hackintosh/post-install/8.png)|
|![clover](/assets/images/hackintosh/post-install/9.png)||

- Đa số pc hay laptop mình chỉ cần dùng AptioMemoryFix-64 là có thể load nvram
- Nếu cài như trên mà bị lỗi shutdown hay restart không được thì hãy sử dụng OsxAptioFixDrv-64 + EmuVariableUefi-64 thay cho AptioMemoryFix-64

|----|----|
|![clover](/assets/images/hackintosh/post-install/10.png)|![clover](/assets/images/hackintosh/post-install/11.png)|

- Nếu bạn dùng main MSI thì hãy dùng OsxAptioFix2Drv-free2000.efi + EmuVariableUefi-64. Download [OsxAptioFix2Drv-free2000.efi](http://hackintosher.com/wp-content/uploads/2017/07/OsxAptioFix2Drv-free2000.efi_.zip) và thêm vào __EFI/Clover/drivers64UEFI__ và xóa AptioMemoryFix-64.efi đi. EmuVariableUefi-64 bạn có lấy từ bộ cài Clover để cài hoặc dùng Clover Configurator.
![cc-uefi](/assets/images/hackintosh/post-install/cc-uefi.png)

## Kexts
Hiện tại trong clover đã có kext cơ bản, đối với PC thì từng đó kext là đủ nhưng đối với laptop thì sẽ cần một số kext khác nữa.

+ Kext chung cho PC và laptop:
  - __FakePCIID + FakePCIID_XHCIMux__: điều hướng USB2 sang EHCI dành Broadwell trở về trước, Skylake trở về sau không cần. [Download](https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/)
  - __AirportBrcmFixup__: đi kèm với Lilu hỗ trợ wifi cho một số card wifi broadcom có thể chạy trên macOS [Download](https://github.com/acidanthera/AirportBrcmFixup)
  - __BT4LEContiunityFixup__: Đi kèm với Lilu giúp bật tính năng Handoff, Hostspot đối với một card wifi broadcom hỗ trợ macOS [Download](https://github.com/acidanthera/BT4LEContiunityFixup)
  - __AppleALC__: kext hỗ trợ audio cho macOS đi kèm với __Lilu__ cần inject layout-id phù hợp trong config.plist (sẽ có bài hướng dẫn sau) để có âm thanh [Download](https://github.com/acidanthera/AppleALC)
  - __VirtualSMC__: kext mới để thay thế cho __FakeSMC__ cần đi kèm với __Lilu__. Chỉ dùng một trong hai kext FakeSMC hoặc VirtualSMC, hiện tại cá nhân mình dùng FakeSMC vẫn ổn. Nếu dùng VirtualSMC hãy xóa SMCHelper-64.efi trong Clover/driver64Uefi và thay vào đó là VirtualSmc-64.efi [Download](https://github.com/acidanthera/VirtualSMC)

+ Dành cho laptop:
  - __ACPIBatteryManager__: kext quản lí pin dành cho laptop [Download](https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/)
  - __SmartTouchpad__: dành cho một số touchpad ELAN đời cũ, hãy thử kext này nếu dùng VoodooPS2Controller không có touchpad. Tải bản beta mới nhất ở đây [Download](https://osxlatitude.com/forums/topic/1948-elan-focaltech-and-synaptics-smart-touchpad-driver-mac-os-x/)
  - __VoodooI2C + Plugin__: dành cho laptop có touchpad I2C, yêu cầu phải patch DSDT/SSDT mới dùng được. Nếu patch được thì có thể dùng được gesture như real mac [Download](https://github.com/alexandred/VoodooI2C/releases)


Chọn được kext phù hợp rồi thì thêm hết vào __EFI/Clover/kexts/Other__

## Tổng kết

Đến đây đối với PC thì có thể hoạt động được cơ bản, còn đối với laptop thì vẫn còn một chặng đường dài. Để hoàn thiện hệ thống hackintosh còn cần patch một số thành phần nữa, mình sẽ viết guide sau, mong các bạn ủng hộ.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
