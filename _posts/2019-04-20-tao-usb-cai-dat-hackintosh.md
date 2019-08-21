---
  title: Tạo usb cài đặt hackintosh
  author: Nguyen Van Hung
  tags: hackintosh usb
  aside:
    toc: true
---
Bạn sẽ cần macOS để thực hiện tạo usb vanilla hackintosh. Ngoài ra vẫn có cách khác để tạo usb vanilla mà không cần đến macOS nhưng mà để hiểu và ít lỗi xảy ra hãy làm theo cách này. Những cách khác mình sẽ viết sau.

## Chuẩn bị bộ cài, USB
- Bộ cài macOS gốc tải từ App Store:
  + [High Sierra 10.13.6](https://itunes.apple.com/ca/app/macos-high-sierra/id1246284741?mt=12)
  + [Mojave mới nhất](https://itunes.apple.com/us/app/macos-mojave/id1398502828?mt=12)
- Thường thì tải từ App Store rất chậm ở Việt Nam, bạn có thể tham khảo thêm một số nguồn bộ cài bên ngoài nhưng có thể gây lỗi. Mình sẽ đưa ra một nguồn mà mình hay sử dụng và cảm thấy ổn là từ [Olarila](https://olarila.com), tải về giải nén và cho vào thư mục Applications:
  + [High Sierra 10.13.6 17G2208](https://drive.google.com/file/d/1h5npjpj630TTZzrRl-g69JlWtLmFxQA8/view) (Bản này nhận native cpu coffeelake)
  + [High Sierra 10.13.6 17G65](https://drive.google.com/file/d/1dNDz9sGe1wuvNMKzjklwBUKO1Iq4AI63/view)
  + [Mojave 10.14.5 18F132](https://drive.google.com/file/d/1HN_4cVHo1Is9JuB2l8pkfiCGjWgtt_vT/view)
- Trong blog này mình sẽ chỉ hướng dẫn về cài các bản 10.13.6 hoặc 10.14.x thôi do một số kext quan trọng ngưng support các bản cũ hơn. Hãy chắc chắn rằng bạn đã có bộ cài trong thư mục Applications để thực hiện các bước tiếp theo.

## Format USB
Có 2 cách format usb bạn thích cách nào thì lam cách đó. Cả 2 cách sẽ tự động tạo ra phân vùng EFI 200MB.
### Dùng Disk Utility
  1. Cắm USB vào. Nếu bạn dùng máy ảo VMWare thì nếu usb không kết nối thì làm như sau: chọn VM ở trên thanh công cụ -> Removable Device -> chọn tên USB của bạn -> Connect
  2. Mở app Disk Utility
  3. Chọn View -> Show all devices
  ![show all devices](/assets/images/hackintosh/disk-utility/show.png)

  4. Chọn USB của bạn ở cột bên trái rồi chọn erase
  ![show all devices](/assets/images/hackintosh/disk-utility/usb.png)

  5. Sửa các lựa chọn như sau

    - Name: install_osx
    - Format: Mac OS Extended (Journaled)
    - Scheme: GUID Partition Map

  ![show all devices](/assets/images/hackintosh/disk-utility/erase.png)

  6. Chọn __Erase__

### Dùng Command line
  1. Mở Terminal lên
  2. Dùng lệnh sau:
  ```bash
  diskutil list
  ```
  3. Nhìn vào Terminal rồi xác định đường dẫn của usb, trong trường hợp của mình là __/dev/disk2__
  ![terminal](/assets/images/hackintosh/terminal/usb.png)
  4. Chạy lệnh sau để xóa usb, hãy thay thế /dev/disk2 bằng dẫn đến usb của bạn.
  ```bash
  diskutil partitionDisk /dev/disk2 1 GPT HFS+J "install_osx" R
  ```

## Tích hợp bộ cài vào USB
  Có 2 cách để copy bộ cài vào usb:
  - __Createinstallmedia:__ đề nghị sử dụng cách này vì nó khá đơn giản và cũng rất ít khi xảy ra lỗi. Đây cũng là cách tạo usb cài macOS cho real mac.
  - __BaseBinaries:__ cách này sẽ tạo ra usb dạng recovery và nó khá phức tạp, chỉ nên sử dụng khi cách bên trên không dùng được. Cách này không sử dụng được đối với High Sierra, Mojave và các bản cao hơn nên mình sẽ không đề cập đến. Nhưng mình cũng không hiểu sao Olarila images lại dùng cách này được.

### Createinstallmedia
  Mở Terminal lên và chạy các lệnh sau:
  - Đối với Mojave:
  ```bash
  # copy installer image
  sudo "/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction
  ```
  Nhập password và đợi nó chạy xong khoảng 20-30 phút tùy vào tốc độ usb của bạn. Lưu ý trong terminal nhập password nó không hiện gì cả bạn cứ nhập xong rồi ấn enter thôi. Sau đó đổi tên USB cho dễ sử dụng:
  ```bash
  # rename
  sudo diskutil rename "Install macOS Mojave" install_osx
  ```
  - Tương tự đối với High Sierra:
  ```bash
  # copy installer image
  sudo "/Applications/Install macOS High Sierra.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction
  # rename
  sudo diskutil rename "Install macOS High Sierra" install_osx
  ```

## Cài đặt và chỉnh sửa Clover vào USB
### Clover installer
  Mình sẽ chỉ hướng dẫn Clover UEFI, Clover Legacy sẽ không được support ở trong blog này.

  Download Clover installer, nên dùng bản mới nhất:
  [https://sourceforge.net/projects/cloverefiboot/](https://sourceforge.net/projects/cloverefiboot/)

  Chạy Clover installer và làm theo các bước sau.
  1. Chọn Continue -> Continue -> Continue để qua mấy cái bước thủ tục
  2. Chọn "Change Install Location" -> chọn đích đến là cái usb, "install_osx" (lúc nãy mình đã sửa tên) -> Continue
  3. Chọn Customize và tích các mục sau:
    - Tích "Install for UEFI booting only", "Install Clover in the ESP" sẽ tự động được tích lên.
    - Tích "Black Green Moody" trong mục Themes (đây là theme mình hay dùng, bạn có thể dùng theme khác và chỉnh sửa trong config.plist)
    - Mục UEFI Drivers tích như ảnh sau:
    ![clover installer](/assets/images/hackintosh/clover/uefi-driver.png)
  4. Chọn Intsall.

  Sau khi clover installer chạy xong nó sẽ mount efi của usb ra luôn. Bạn sẽ thấy xuất hiện ổ EFI ở trong finder và desktop. Cuối cùng cần thêm một efi driver không có sẵn trong clover installer là HFSPlus.efi, nếu không có nó thì khi boot vào clover sẽ không thấy bộ cài đâu cả:
  - Download: [HFSPlus.efi](https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi)
  - Copy nó đến /EFI/Clover/drivers64UEFI

  __Lưu ý:__
  - Trong số ít trường hợp HFSPlus.efi gây lỗi không boot được lúc này bạn có thể dùng VBoxHfs-64.efi thay thế có sẵn trong clover installer. Không được để cả 2 một chỗ nếu không sẽ gây xung đột.
  - Trong nhiều trường hợp boot vào bộ cài bị đứng ở chỗ những dòng lệnh đầu thì có thể đó là lỗi của AptioMemoryFix-64.efi, hãy thay thế nó bằng OsxAptioFixDrv-64.efi hoặc OsxAptioFix3Drv-64.efi có sẵn trong clover installer.
  - Đối với PC main MSI thì hã sử dụng OsxAptioFix2drv-free2000.efi thay cho AptioMemoryFix-64.efi. [Dowload](http://hackintosher.com/wp-content/uploads/2017/07/OsxAptioFix2Drv-free2000.efi_.zip)

### Kexts
  Hãy xóa tất cả các thư mục trong EFI/CLOVER/kexts/ chỉ để lại thư mục Other. Để mình giải thích kext trong Other sẽ được load ở tất cả các bản mac và recovery, còn các thư mục 10.x thì nó sẽ chỉ load kext trong đó nếu bạn boot các phiên bản macOS tương ứng.

  __Các kext cần thiết cho usb bộ cài hackintosh__
  Hãy tải kext phiên bản mới nhất
  - __FakeSMC.kext__: Đây là kext quan trọng nhất đối với hackintosh, nó giúp giả lập các key SMC, giúp đọc thông tin từ các cảm biến trong máy. [Download](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/)
  - __VoodooPS2Controller.kext__: Kext dành cho bàn phím, chuột và trackpad PS2. [Download](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)
  - __Lilu__: là kext hỗ trợ patch on-the-fly kext hệ thống của macOS mà không chỉnh sửa trực tiếp. Đây là kext đã làm cho việc hackintosh hiện tại dễ hơn rất nhiều so với trước kia. [Download](https://github.com/acidanthera/Lilu/releases)
  - __Whatevergreen__: Là plugin của Lilu, kext all in one để giải quyết vấn đề về graphics, card rời AMD, NVIDIA hay card onboard Intel HD Graphics chỉ cần nó là đủ. [Download](https://github.com/acidanthera/WhateverGreen/releases)
  - __Ethernet Kexts__:
    + __IntelMausiEthernet.kext__ hoạt động với đa số Intel LAN. [Download](https://bitbucket.org/RehabMan/os-x-intel-network/downloads/)
    + __AtherosE2200Ethernet.kext__ hoạt động với đa số Atheros hoặc Killer Ethernet. Đa số laptop MSI dùng kext này. [Download](https://github.com/Mieze/AtherosE2200Ethernet/releases)
    + __RealtekRTL8111.kext__ hoạt động với đa số Realtek LAN. [Download](https://bitbucket.org/RehabMan/os-x-realtek-network/downloads/)
    + __RealtekRTL8100.kext__ dành cho dòng Realtek RTL810x Fast Ethernet [Download](https://www.insanelymac.com/forum/files/file/259-realtekrtl8100-binary/)
  Mình thường dùng cả bốn kext này trong clover vì mình cài nhiều máy nên để sẵn trong đó cả đỡ mất công thay.
  - __USBInjectAll.kext__: kext fix cổng USB 3.0, đa số main pc và laptop đều sử dụng kext này. [Download](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)
  - __Kext ít khi phải dùng__: [Download](https://github.com/RehabMan/hack-tools/archive/master.zip)
    + __XHCI-unsupported.kext__: đi kèm với USBInjectAll dành cho một số ít main đặc biệt như X79/X99/X299/X399, ... mà USBInjectAll.kext không hỗ trợ.
    + __SATA-unsupported.kext__: dành cho pc hay laptop sử dụng cpu skylake trở đi mà sata controller/chipset Apple không hỗ trợ. Nói chung không boot bật Disk Utility mà không thấy ổ cứng đâu thì là thiếu kext này.

  __Cho tất cả các kext cần thiết vào thư mục /EFI/CLOVER/Kexts/Other__. Đây là kext trong clover usb mình sử dụng:
  ![clover kexts](/assets/images/hackintosh/clover/kexts.png)

### Config
  Cần phải chỉnh sửa config.plist để có thể boot vào macOS. Mình sẽ không giải thích quá nhiều về các lựa chọn trong config.plist vì nó quá nhiều bạn nào muốn biết rõ mấy cái này hãy vào và đọc ở [Clover Wiki](https://clover-wiki.zetam.org/).

  Bạn cần có phần mềm để chỉnh sửa config.plist, có thể sử dụng [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/), [Xcode](https://itunes.apple.com/vn/app/xcode/id497799835?mt=12), [PlistEdit Pro](https://www.fatcatsoftware.com/plisteditpro/)

  Mình sẽ chia ra 2 phần chính dành cho PC và laptop:
  - __Đối với Laptop__

    Đa số laptop đều sẽ không nhận card rời nên sẽ chủ yếu dùng card onboard. Mình khuyến nghị sử dụng config của __RehabMan__ làm sẵn ở đây: [https://github.com/RehabMan/OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config).

    Hãy download toàn bộ repos bằng link sau: [https://github.com/RehabMan/OS-X-Clover-Laptop-Config/archive/master.zip](https://github.com/RehabMan/OS-X-Clover-Laptop-Config/archive/master.zip)

    ![Rehabman Config](/assets/images/hackintosh/finder/rehabman-config.png)

    Trong repos có nhiều file config tương ứng với các dòng HD Graphics, bạn chọn đúng config với HD Graphics của máy. Sau đó copy nó đến /EFI/CLOVER và thay thế file config hiện có. Hãy chắc rằng bạn đã sửa tên nó thành "config.plist".

  - __Đối với PC__

    PC có thể dùng được các loại card rời và cả card onboard. Vấn đề là PC không có kho config chuẩn giống rehabman làm cho laptop nên mình đã làm và chỉnh sửa lại cho PC để đảm bảo có thể boot và dùng ổn định. Config do mình làm được tham khảo từ [Olarila](https://olarila.com/forum/), [Hackintosh Vanilla](https://hackintosh.gitbook.io/) và [
Intel Framebuffer patching using WhateverGreen](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/). Config này mình cài dịch vụ kha khá máy đều ổn, có thể nó chưa chuẩn các bạn có thể góp ý qua group facebook hoặc github. Mình sẽ chia ra làm hai loại:

    + PC dùng card rời: sẽ không cần chỉnh sửa config về phần graphic, tất cả sẽ do WhateverGreen lo. Download và lựa chọn config tương ứng ở đây [kirito4499 config dgpu](https://drive.google.com/file/d/1qf-LRMqjc-GlHpOoHISgehwCoqEJvSID/view?usp=sharing).

    + PC dùng Intel HD Graphics, sẽ khó làm hơn và nên chỉnh sửa phần config để nhận được card onboard. Mình đã chỉnh sửa sẵn bạn tải và lựa chọn ở đây [kirito4499 config igpu](https://drive.google.com/file/d/1MoTYVXzAlBykJZrFRiGK7H_L6cInZLfj/view?usp=sharing).

    Chọn được file rồi thì bạn copy nó đến /EFI/CLOVER/ và sửa tên thành "config.plist".

    __Lưu ý__: Nếu bạn không boot được với config của mình hãy vào [group facebook](https://www.facebook.com/groups/hackintoshPC/) để hỏi, mình sẽ support ở kênh này, các bạn đừng gọi điện hay nhắn tin.

## Tổng kết
  Trên đây mình đã hướng dẫn các bạn tạo usb hackintosh sạch. Ngoài ra có một số cách tạo usb nhanh hơn không cần macOS mình sẽ viết bài sau. Tiếp theo là setup bios và tiến hành cài đặt.

  Bài viết được tham khảo từ các nguồn sau:
  - [https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/)
  - [https://hackintosh.gitbook.io](https://hackintosh.gitbook.io)

  Chân thành cảm ơn đến Rehabman, Corpnewt, Olarila team, Acidanthera team và nhiều người khác đã viết kext, guide và chia sẻ hackintosh.

  __Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
