---
title: Sử dụng Clover bootloader để quản lý Multi OS
tags: hackintosh clover
author: Nguyen Van Hung
aside:
  toc: true
---

# Lợi ích của Clover:

-   Có thể boot vào Mac OSX 10,4-10,9, Windows và Linux EFI
-   Có thể boot vào LegacyOS (Windows XP, Linux, hệ điều hành DOS)
-   Tự động phát hiện phần cứng và thiết lập . Nhưng có thể thay đổi bằng cách edit lại config.plist
-   Với Clover bạn có thể khởi động vào hệ điều hành khác từ Startup Disk prefpane của Mac OS
-   Full độ phân giải trong giao diện đồ họa boot của các loại card màn hình
-   Điều khiển bằng chuột ngay trong màn hình boot (giống Mac OS)
-   Tự động cấu hình bằng OEM tên nhà sản xuất.
-   Autopatch OemDSDT để hoạt động được trên OSX
-   SMBIOS sẽ được tự động lấy cho phù hợp cấu hình máy.
-   ACPI sẽ được patch theo tiêu chuẩn 4.0. DSDT đã được patch sẽ được nạp từ phân vùng khởi động hoặc từ thư mục EFI
-   Tự động cấu hính ACPI (SSDT-xx, APIC, BOOT, SLIC, khe, SRAT, UEFI …)
-   Thiết lập một cách chính xác PowerProfile cho máy tính xách tay (notebook), máy tính để bàn, máy trạm
-   RestartFix
-   Sleep / Wake hệ thống
-   Có thể thay đổi giá trị PCIRootUID (0,1) cho các card màn hình.
-   Hỗ trợ active card màn hình ATI, NVIDIA và Intel. Cho phep edit một số thành phần quan trọng trong đó.
-   Có thể thêm vào thông tin EDID của màn hình để fix các lỗi về graphics
-   Fix các lỗi về USB
-   Patch lại AppleHDA và HDMI.
-   Ethernetbuilin
-   Enable CPU Turbo
-   Enable P và C state ngay ngoài màn hình boot
-   hỗ trợ cho CPU ​​Atom và Ivy Bridge
-   Fix lại kext trong kernelcache cho phần cứng không được hỗ trợ
-   Thêm được kext và inject trực tiếp từ CLover\Kext Folder
-   Chế độ bảo mật cho FireWire
-   Thay đổi được thời gian chờ lúc boot
-   Hỗ trợ thay đổi Theme: hỗ trợ chủ đề, biểu tượng riêng, phông chữ, hình nền, chuột.
-   Thay đổi dc ngôn ngữ.
-   Lưu lại các thành phần gốc của ACPI bằng cách nhấn F4
-   Kiểm tra DSDT patch với F5
-   Lưu rom card màn hình vào EFI / misc bằng cách nhấn F6
-   Lưu ảnh chụp màn hình từ giao diện bằng cách nhấn F10
-   Nhấn F12 để điều khiển DVD ngay ở giao diện boot

## Nguyên tắc hoạt động:

 ### Ở chế độ UEFI boot:
UEFI BIOS->BOOTX64.efi->Apple’s boot.efi->mach_kernel

-   Đối với UEFI boot, nó cần load driver để nhận diện được UEFI của từng nhà sản xuất:  
    Các driver cơ bản để nhận diện được kê ra như sau:
-   HFSPlus.efi, OsxFatBinaryDrv-64.efi, OsxAptioFixDrv-64.efi, EmuVariableRuntimeDxe.efi HFSPlus.efi, OsxFatBinaryDrv-64.efi,  ngoài ra một số driver mới được dev phát triển hiện này là AptioMemoryfix.efi 

1.  Driver này hoạt động trên main Gigabyte EFI. Đây là lựa chọn tốt nhất cho UEFI khởi động, tức không cần phải thêm bất kỳ driver nào nữa (các bạn chú ý hai driver này nhận diện ổ EFI định dạng fat 32 và ổ Mac định dạng HFS)
2.  HFSPlus.efi, OsxFatBinaryDrv-64.efi, OsxLowMemFixDrv-64.efi
3.  Driver này hoạt động trên Insyde H2O UEFI. Một số vấn đề bộ nhớ nhỏ sẽ được fix bằng LowMemFix, và sau đó tất cả mọi thứ giống với trường hợp 1
4.  HFSPlus.efi, OsxFatBinaryDrv-64.efi, OsxAptioFixDrv-64.efi  
    Driver này hoạt động tốt trên tất cả các main hỗ trợ AMI Aptio EFI . Đây không phải là giải pháp tốt vì nó phụ thuộc vào hoạt động hiện tại của boot.efi và cấu trúc hiện tại được thông qua giữa boot.efi(boot loader) và kernel (cái boot.efi này nằm trong system=>Library=>Coreservice của ổ Mac đấy) đối với AMI. Ví dụ về cái OsxAptioFixDrv-64.efi: Chameleon trước đây ko thể boot được 10.7 hoặc 10.8 lúc Apple mới đưa ra hệ điều hành, điều này do Apple đã thay đổ cấu trúc của file boot.efi trong phân vùng của Mac OS (extension.mkext chuyển thành kernelcache đấy các bạn), sau đó Chameleon mới dc fix lại để tương thích với boot.efi. Clover cũng giống vậy, việc thay đổ cấu trúc boot.efi của Apple làm Clover ko thể khởi động được vào các main AMI và giải pháp đưa ra là OsxAptioFixDrv-64.efi để fix sự thay đổi này (cái fix này sẽ phải thay đổi khi Aple thay đổi cấu trúc của boot.efi và kernel).
5.  HFSPlus.efi, OsxFatBinaryDrv-64.efi, OsxAptioFixDrv-64.efi, EmuVariableRuntimeDxe.efi

Driver này hoạt động trên Dell Vostro, một số của ThinkPad – một số máy tính xách tay với Phoenix UEFI. Tất cả được đề cập trong trường hợp 3 đều có thể áp dụng ở đây.

Ngoài ra còn có một số Driver khác hỗ trợ, tuy nhiên đây là những driver cơ bản nhất để UEFI có thể boot dc CLover.  

**=> Vậy các bạn đã hiểu là một khi muốn sử dụng UEFI để boot thì phải cài đặt Driver cho main, tùy từng trường hợp cụ thể mà chọn Driver thích hợp nếu không sẽ không boot được**


### Ở chế độ BIOS boot
BIOS->boot0->boot1->BOOT->CLOVERIA32.efi->Apple’s boot.efi->mach_kernel2 bit)  
BIOS->boot0->boot1->BOOT->CLOVERX64.efi->Apple’s boot.efi->mach_kernel4 bit)

-   Ở chế độ BIOS boot thì Clover hoạt động gần chư Chameleon nhưng khác ở chỗ Chameleon load Kernelcache và mach_kernel trực tiếp, còn Clover thông qua boot giống chameleon và load Clover boot=>> Apple boot.efi. Vậy với những máy ko có UEFI vẫn có thể cài được Clover (nhưng theo tác giả thì có thể dc hoặc ko), hai hôm nay mình test Clover trên các máy ko hỗ trợ UEFI vẫn hoạt động tốt
-   Một chú ý quan trọng là ở chế độ bios thì Clover vẫn load các driver ch UEFI nên khi cài đặt các bạn cũng phải chọn Driver trong mmuc5 UEFI nhé.
-   Yêu cầu của Clover rên Bios cũng giống trên Chameleon là phải active phân vùng chứa boot và vẫ phải fix boot1h với các ổ 4k sector. Mình sẽ hướng dẫn các bạn cách Config trên Legacy Bios.

### Vấn đề cần giải quyết

-   Thường thì sau khi cài hackintosh, chọn Clover làm trình khởi động thì menu của Windows, MacOS, và một số Linux đã mặc định hiện trên Menu của clover. Nguyên do là Clover đọc file *.efi từ phân vùng EFI để tự động thêm vào menu. (auto scan mode)

-   **Vấn đề 1**: nếu bạn chọn cài những Linux Distro phổ biến thì menu sẽ tự động thêm cho bạn nhưng nếu những distro bạn dùng không phổ biến hoặc mới xuất hiện vài năm gần đây thì menu sẽ không được tự động cho bạn (ví dụ: ParrotSec OS)

-   **Vấn đề 2**: Boot qua file *.efi cho Linux cũng chỉ là một bước trung gian để khởi động vào Grub2. Vậy nếu để mặc định thì lại thành thừa thãi. Nguyên do là bạn phải dùng 2 thao tác là chọn menu qua clover rồi lại chọn qua Grub2 mới vào hệ điều hành mà bạn muốn >>> Mệt người @_@

## Mục đích của guide này?

Thêm đầy đủ các hệ điều hành từ Windows, MacOS đến Linux lên menu của Clover và chỉ cần enter vào menu đã chọn là boot thẳng vào hệ điều hành bạn muốn. Ngoài ra còn hướng dẫn cho các bạn cách khắc phục một số lỗi trong quá trình sử dụng

Linux khởi động từ Clover như thế nào?

Từ Kernel 3.3 trở lên Linux đã hỗ trợ khởi động bằng EFISTUB (EFI BOOT STUB). Đây là phương pháp có thể khởi động trực tiếp kernel từ EFI mode. Grub2 cũng khởi động thông qua nó, khi sử dụng Clover bootloader bạn có thể boot trực tiếp qua kernel này, xong ở một số trường hợp Clover chưa hỗ trợ cho một số distro linux đặc biệt ta vẫn cần load thông qua trợ giúp Grub2 bootloader

Mặc định Clover bootloader không hỗ trợ driver cho linux để load trực tiếp kernel, để sử dụng tính năng hữu ích này ta cần lấy driver từ rEFInd boot Manager

### Có bao nhiêu cách để thêm Linux vào menu Clover!?

**Cách 1**: Chỉ cần chép driver của rEFInd vào thư mục drivers64UEFI là, trong thẻ GUI ở khung scan tích vào cả 2 ô kernel và linux là menu của Linux tự động add vào menu clover

-   Đơn giản, không phải thao tác gì nhiều. Cách này dùng cho những distro phổ biến
-   Không tùy biến icon được, một số distro mới Clover không nhận diện được

**Cách 2**: Thêm menu trực tiếp cho linux vào Clover bằng cách boot qua vmlinuz. Thực ra bản chất cũng như cách 1 nhưng ta có thể tùy chỉnh được các thiết lập

-   Không phụ thuộc vào grub2
-   Dễ chỉnh sửa thuộc tính boot
-   Không thành công ở một số trường hợp

**Cách 3**: Thêm menu cho Linux trên Clover bằng cách chạy thông qua file grubx64.efi (hoặc shimx64.efi hay bootx64.efi)

-   Tận dụng config mặc định của Grub2, dùng cho một số distro đặc biệt không thể load thông qua vmlinuz trực tiếp được thí dụ như các distro họ Android hay các distro mới ra gần đây có cấu trúc phân vùng đặc biệt khiến driver chưa thể nhận diện được
-   Chỉ cần địa chỉ phân vùng PARTGUID, bạn có thể lấy thông qua boot.log trên Clover configurator
-   Cần phải thao tác thêm vào grub2

Cách 4: Thêm menu cho Linux vào Clover bằng cách boot trực tiếp thông qua kernel và vmlinuz được người dung chỉ định

-   Menu tùy biến được nhiều, phù hợp với mọi distro Linux
-   Config cần thiết lập nâng cao, tận dung phương pháp BOOT STUB của linux theo Grub2_efi
-   Copy trực tiếp kernel và vmlinuz vào phân vùng EFI để Boot

Với cách 2 và cách 3 hầu hết các thiết lập đều đã có sẵn duy nhất có 2 giá trị ta cần phải tìm đó , còn với cách 4 khuyên dung nhất vì thành công tronh mọi trường hơp. Ngoài ra chúng ta cần tới UUID (AddArguments) và PARTUUID (Volume) của phân vùng OS mà chúng ta định them vào

-   Đối với linux thường là phân vùng hệ thống EX4 với kernel dạng initrd.img & vmlinuz
-   Đối với windows boot thông qua file bootmgfw.efi chứa trong \EFI\Microsoft\Boot\bootmgfw.efi với UUID là partition EFI

Có 2 cách để xác định UUID của phân vùng OS của bạn :

-   Thông qua linux với lệnh trên Terminal : “sudo blkid”

-   Sử dụng Clover Configurator với chức năng regenator boot.log ta sẽ xem được UUID của từng phân vùng

![](/assets/images/hackintosh/clover/log.png)

Các bạn mở file config.plist trong EFI/CLOVER rồi chuyển sang thẻ Gui và làm như hình. Click vào dấu “+” ở khung “Custom Entries” để thêm Menu mới cho UEFI mode

Thực ra đối với Windows và Mac OS thì không cần phải thêm menu làm gì vì Clover đã tự nhận diện trên trình đơn khởi động, nhưng nếu bạn muốn sắp xếp menu nào trước sau và tùy biên icon hay bootflag thì nên tùy chỉnh thêm menu cho nó

-   **Volume**: dán địa chỉ phân vùng vào đây (xem ở boot.log).
-   _Lưu ý nếu boot từ file  ******.efi**  thì địa chỉ phân vùng là của EFI; còn từ boot trực tiếp từ kernel thì địa chỉ phân vùng là phân vùng cài đặt OS_
-   **Path**: Là đường dẫn đến file  ****.efi  hoặc  vmlinuz
-   **AddArguments**:Với Mac OS kiểu như này:  
    **_slide=0 dart=0 nv_disable=1 -gux_defer_usb2 kext-dev-mode=1_**Với Linux thì có cấu trúc như này  
    **_root=UUID=__62b30549-d7c3-4e82-b905-92031a7a7f50_ _ro initrd=initrd.imgadd_efi_memmap_** or  **_root=UUID=__62b30549-d7c3-4e82-b905-92031a7a7f50_ _ro initrd=\EFI\ubuntu\initrd.img  add_efi_memmap_**

(thêm lệnh  **quite**  nếu muốn ẩn các tiến trình khi boot)  
Thay chuỗi số phía trên bằng UUID phân vùng chứa OS mà bạn muốn thêm menu

-   **Title / FullTitle**: Oánh tên bạn muốn hiện trên menu khi chọn
-   **Image**: tên của icon trong thư mục icon của theme bạn đang dùng, lưu ý nếu chưa có icon của OS bạn đang dùng bạn có thể tạo icon bằng photoshop định dạng **.png**sau đó đổi đuôi thành **.icns** (Thí dụ tên file icon là **icns** thì chỉ cần điền ở mục này là **kali**)

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/10.png)

-   **Type**: Chọn kiểu boot cho OS (Windows, OSX, Linux, First)
-   **VolumeType**: chọn  **Internal**  hoặc  **External**
Để trực quan trong chỉnh config.plist bạn có thể sử dụng máy ảo chạy Mac OS và cài thêm Clover configurator (xem cách sử dụng máy ảo Mac OS [tại đây](https://niemtin007.blogspot.com/2014/12/tao-may-ao-macos-vmware-hackintosh.html)). Nhưng để không mất thời gian bạn có thể chỉnh sửa config.plist bằng trình edit thông dụng như [Notepad++](https://notepad-plus-plus.org/) cũng được chỉ cần bạn cẩn thận một chút để tránh lỗi cú pháp là được

-   Tải Clover ISO tại đây: [https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/](https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/)
-   Tiến hành giải nén file vừa tải, mount Clover-v2.3k-xxxx-X64.iso và copy thư mục CLOVER ra phân vùng EFI của ổ cứng để tiến hành chỉnh sửa config.plist
-   Config mẫu của mình Tại Đây

Cách mount partition EFI trực tiếp trên windows : Sử dụng công cụ diskpart

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/1.png)

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/2.png)

Click vào dấu “-” để thu gọn các thẻ không dùng đến, ta chỉ quan tâm đến thẻ GUI. Các bạn so sánh nội dung code dưới đây và hình phía trên để chỉnh sửa code cho đúng nhé

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/3.png)

Sau khi chỉnh sửa config.plist xong copy thư mục Clover vào phân vùng ESP (EFI). Set boot cho Clover làm mặc định. Có rất nhiều cách Mount EFI :

-   Sử dụng công cụ diskpart trực tiếp trên windows (đã nói ở trên nhé)
-   sử dụng winPE bằng các công cụ quản lý HDD (Mini Tool Partition Wizard, Bootice….)
-   XorBoot Manager cũng tương tự Bootice nhưng tùy biến hơn

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/1-1.png)  ![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/2-1.png)

**Cách 1**. Nếu cài các distro phổ biến như Ubuntu, Linux Mint, Fedora, OpenSuse, … chỉ cần copy driver vào thư mục drivers64UEFI, sau đó tùy chỉnh thêm config.plist bật 2 thiết lập kernel và linux là được, menu của linux sẽ tự động hiện trên Clover bootloader

**Cách 2**. Nếu sau khi làm theo cách 1 nếu Clover nhận kernel nhưng hệ thống icon nhận sai bạn có thể dùng cách 2 để thêm menu entries cho Linux để dễ tùy chỉnh

Có 2 thiết lập quan trọng cần chú ý là:

Volume: **_vmlinuz_**

AddArguments: nhập theo cấu trúc

**_root=UUID=62b30549-d7c3-4e82-b905-92031a7a7f50 ro initrd=initrd.img add_efi_memmap_**

(thêm lệnh **quite** nếu muốn ẩn các tiến trình khi boot)

Thay chuỗi số phía trên bằng UUID phân vùng chứa OS mà bạn muốn thêm menu

Các thiết lập còn lại bạn tùy chỉnh giống như menu Ubuntu mình đã ví dụ ở phía trên

đối với add custom entry bằng EFISTUB Booting các bạn có thể làm như sau đối với một số trường hợp đã copy đấy đủ driver hỗ trợ nhận diện distro linux vào driver64UEFI và add argument theo đúng hướng dẫn nhưng vẫn không hiện custom entry :

**Cách 3**:

-   copy và paste file kenel và initrd của distro linux vào EFI partition ( có thể tạo folder vd như \EFI\ubuntu\*file kenel và initrd*)

![](https://docngaydi.tinsinhphuc.com/wp-content/uploads/2017/06/3-1.png)

-   mặc định hai file này nằm ở trong folder \**Boot** của phân vùng gốc của linux
-   Edit Argument  : thay initrd=\EFI\ubuntu\*” (thay * bằng tên tuỳ chỉnh của file kenel mà bạn đã copy vào EFI vừa nãy)
-   Edit Pach  : \EFI\ubuntu\vnlinuz-* (thay * tương tụ bằng tên file tuỳ chỉnh của initrd mà bạn đã copy vào EFI)
-   Volume  : tất nhiên là PartUUID của phân vùng EFI chứ không phải là phân vùng của distro linux
-   Save và reboot để xem kết quả  
    **NOTE**:
-   các bạn có thể tuỳ chỉnh argument như sau đối với bạn nào dùng distro có được splash screen (hiểu nôm na tương tự bootanimation của windows)  
    “…………..ro  **splash=silent quiet showopts**  initrd=\EFI…..” :  **openSUSE**  
    “…………..ro  **quiet splash $vt_handoff**  initrd=\EFI………..” :  **Mint linux**
-   Một số trường hợp có nhiều kenel do quá trình update và upgrade kenel lên bản mới linux sẽ không xoá kenel cũ mà vẫn giữ lại các bạn có thể tạo sub menuentry tương tự menuentry đối với kenel cũ đó phòng trường hợp kenel mới không tương thích (đã có hướng dẫn ở ảnh trên nhóe ^.^)

**Cách 4**. Sau khi làm cách 1 đầy đủ nhưng không thấy menu xuất hiện, làm luôn cách 2 mà cũng không được thì bạn có thể liệt kê cái Linux OS bạn đang cài là thuộc dạng đặc biệt. Đến thời điểm này mình chỉ mới biết là Parrot OS và các distro họ Android (Androidx86, RemixOS, PhoenixOS). Vì vậy lúc này load thông qua grubx64.efi là tối ưu hơn cả.

-   Bước 1: Thêm menu bạn làm tương tự với Parrot OS mà mình đã để hình thí dụ phía trên
-   Bước 2: Boot vào Linux quyền Root tìm đến file grub.cfg theo đường dẫn /boot/grub/grub.cfg; Tìm và sửa hết các dòng có nội dung  **set timeout**  về  **0**  hết cho mình và lưu lại thiết lập

