---
  title: Tạo bộ cài vanilla hackintosh dễ dàng nhất
  author: Nguyen Van Hung
  tags: hackintosh installer clover usb
  aside:
    toc: true
---

## Giới thiệu

- Nói sơ qua về bộ cài cách cài thì mình dựa theo cách làm của [Olarila](https://olarila.com/forum/index.php) để làm ra file disk image của một cái usb cài vanilla hackintosh. Bộ cài đã có sẵn clover bao gồm config, driver và kext cơ bản để có thể cài đặt do mình tự chỉnh sửa và được đảm bảo sẽ tương thích với phiên bản macOS đi kèm. Bộ cài này có thể được dùng trực tiếp trên cả macOS lẫn Windows.

- Bộ cài này sẽ được kiểm soát bởi chình mình do đó các bạn cài được hay thấy lỗi hãy vào group facebook [Hackintosh - The OS X on PC World](https://www.facebook.com/groups/hackintoshPC/) và viết bài, mình sẽ hỗ trợ chính ở trong này. Đương nhiên nếu có các bản macOS mới hơn thì mình cũng sẽ làm các file bộ cài khác. __Nếu cài được theo hướng dẫn của mình thì đừng ngần ngại donate cho mình__.

## Chuẩn bị
+ 1 usb 16GB, tốt nhất lấy usb 3.0 cho nhanh
+ File bộ cài, tải về và giải nén ra sẵn (nếu dùng windows, hãy dùng winrar hoặc 7zip để giải nén):
  - [High Sierra 10.13.6 17G66](https://drive.google.com/file/d/1QAprop0CsmKaTLsbxoPOdj4i2AgYlSpE/view?usp=sharing)
  - [Mojave 10.14.5 18F203](https://drive.google.com/file/d/1yiMlBO7zCgHdLLPN1R4oukNAt7_hZOrc/view?usp=sharing)
  - [Mojave 10.14.6 18G84](https://drive.google.com/open?id=1j4is1Q46eb0uxVKoMe6hd7k1jYh5DsXZ)
+ __Kiểm tra MD5 của file để chắc chắn file không có vấn đề gì trong quá trình tải:__
  - MD5 (High Sierra 17G66.raw.bz2) = __8d0badb4286d576d9f362f8b27874428__
  - MD5 (Mojave 18F203.raw.bz2) = __a9c891d32b400ec0c86501986c646483__
  - MD5 (Mojave 18G84.raw.bz2) = __f79ac1236f3742a4e8575137b947838d__
  - Trên macOS thì chỉ cần mở Terminal lên gõ lệnh __md5__ sau đó kéo file vào
  - Trên windows bạn có thể dùng phần mềm [winmd5](http://www.winmd5.com/) hoặc các phần mềm khác tương tự
+ Đối với __macOS__:
  - [Etcher](https://www.balena.io/etcher/)
  - [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/)
+ Đối với __Windows__:
  - [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)
  - [MiniTool Partition Wizard](https://www.partitionwizard.com/free-partition-manager.html)
  - [Explorer++](https://explorerplusplus.com/download)

## Thực hiện ghi bộ cài lên USB

### Trên macOS
  - Mở Etcher lên -> chọn file bộ cài (đuôi .raw) -> chọn usb -> Flash!
  ![Etcher](/assets/images/hackintosh/etcher/etcher.png)

### Trên Windows
  1. Format USB:

![format](/assets/images/hackintosh/windows/format-usb.png)

  2. Dùng win32diskimager ghi file raw ra USB, cẩn thận chọn đúng device không là xóa nhầm ổ cứng.

![1](/assets/images/hackintosh/windows/win32diskimager.png)

![2](/assets/images/hackintosh/windows/show-all.png)

![3](/assets/images/hackintosh/windows/choose-image.png)

![4](/assets/images/hackintosh/windows/write-image.png)

## Mount EFI
### Trên macOS
+ Dùng Clover Configurator mount efi của usb ra
  ![usb efi](/assets/images/hackintosh/cc/cc-usb.png)

### Trên Windows
+ Dùng Minitool Partition Wizzard:

![change-letter](/assets/images/hackintosh/windows/change-letter.png)

![choose-letter](/assets/images/hackintosh/windows/choose-letter.png)

![apply](/assets/images/hackintosh/windows/apply.png)


## Chỉnh sửa clover

+ Tất cả các file config, kext, driver uefi đều có sẵn trong clover usb rồi.
+ __Việc cần làm là chọn thay config phù hợp, nếu bạn cần dùng thêm thì bạn có thể copy từ bên ngoài vào__.
+ Windows 8/10 sẽ không cho phép bạn dùng file explorer để truy cập vào phân vùng EFI, dùng __Explorer++ chạy quyền admin để truy cập EFI__.

![explorer++](/assets/images/hackintosh/windows/explorer++.png)


### Config
+ Tất cả config cho pc và laptop đều có sẵn ở thư mục __EFI/CLOVER/backup/config/__
+ Chọn config cần dùng copy đến thư mục __EFI/CLOVER/__ và đổi tên thành __config.plist__

![config](/assets/images/hackintosh/usb-efi/config-laptop.png)

![config](/assets/images/hackintosh/usb-efi/config-pc-dgpu.png)

![config](/assets/images/hackintosh/usb-efi/config-pc-igpu.png)

### Kexts
+ Trong thư mục __EFI/CLOVER/Kexts/Other__ đã có đủ kext để dùng cho việc cài đặt, mình cũng để sẵn thêm một số kexts khác đôi lúc cần trong thư mục __EFI/CLOVER/Kexts/Other/backup__.

![kexts](/assets/images/hackintosh/usb-efi/kexts.png)

### Driver64Uefi
+ Cơ bản thì dùng bộ Driver64Uefi sẵn thì sẽ boot được.

![driver-uefi](/assets/images/hackintosh/usb-efi/driver-uefi.png)

+ Nếu bạn boot gặp lỗi như sau hãy thay __AptioMemoryFix-64.efi__ bằng một trong các file OsxAptioFixDrv trong thư mục __EFI/CLOVER/drivers64UEFI/OsxAptioFixDrv_Backup/__

![error](/assets/images/hackintosh/boot/error-boot.png)

+ Đối với main MSI và một số main dành cho CPU Intel CoffeeLake (đời 8) bị lỗi trên hãy thay __AptioMemoryFix-64.efi__ bằng __OsxAptioFix2Drv-free2000.efi__

## Tiến hành cài đặt
+ Làm xong USB rồi setup bios rồi cài đặt, đọc bài viết cũ của mình rồi làm tiếp
  - [Setup bios cho hackintosh](https://vanhung4499.github.io/2019/04/21/setup-bios-cho-hackintosh.html)

## Tổng kết

+ Cảm ơn [Madl0n Olarila](https://www.insanelymac.com/forum/profile/557433-mald0n/), [crazybirdy](https://www.insanelymac.com/forum/profile/61100-crazybirdy/), [Daliansky](https://blog.daliansky.net/about/), __Rehabman__ và tất cả các developer đã đóp góp cho cộng đồng Hackintosh.
+ Bài viết hướng dẫn cách làm bộ cài hackintosh một cách dễ nhất, bạn không cần macOS, không cần tạo máy ảo để làm usb, rút ngắn một công đoạn dài nếu bạn không có real mac.
+ __Nhưng mà cách này cũng sẽ có nhiều rủi ro sẽ gây lỗi nếu file không được bảo toàn do đó vấn khuyễn khích các bạn làm bộ cài trên macOS__

