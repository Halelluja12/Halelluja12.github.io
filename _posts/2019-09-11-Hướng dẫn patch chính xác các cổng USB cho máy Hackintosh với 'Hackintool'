---
title: Hướng dẫn patch chính xác các cổng USB cho máy Hackintosh với 'Hackintool'
tags: hackintosh USB
author: Nguyen Van Hung
aside:
  toc: true
---


# Hướng dẫn patch chính xác các cổng USB cho máy Hackintosh với 'Hackintool'

**"Still waiting for root device"** sinh ra khi phân vùng được coi là System tự nhiên bị ngắt khỏi hệ thống. Ví dụ boot bộ cài mà cổng usb bị ngắt, boot ổ cứng mà cổng sata bị ngắt.  
Bản thân không có một ngắt vật lý nào hết, mà là do kext của Mac không sử dụng được cổng đó nên nó chờ System quay lại, nó sẽ lắng nghe tất cả các port.  
 Lý do sinh ra guide này vì lỗi huyền thoại trên khá là bất tiện và rất hay gặp phải bởi newbie, mặc dù có thể khắc phục tạm thời bằng Limited patch port USB on-the-fly như nó không ổn định và không sử dụng được lâu dài. 



# Các công cụ, công đoạn cần làm bao gồm :

 - Kext [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) của rehabman 
 - [Hackintool](https://www.tonymacx86.com/threads/release-hackintool-v2-8-0.254559/) là công cụ tìm và check inject đúng loại cổng USB
 - [Clover configurator](https://mackie100projects.altervista.org/download-clover-configurator/) để chỉnh sửa file config.plist 
 - Repo của RehabMan chưa các hotpatch và config cần thiết : [here](https://github.com/RehabMan/OS-X-Clover-Laptop-Config)
 - [MaciASL](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/) 
 - IO Registry Explorer
 - Các công đoạn bao gồm :

## Inject toàn bộ cổng usb với kext USBInjectAll của Rehabman .

 - Đặt kext vào **Clover/Kext/Other** 
 - Để kext hoạt động cần chọn đúng SMBIOS sát với cấu hình nhất có thể (đối với laptops) và Patch rename ACPI trong config 
 - Mở config.plist với Clover configurator và add các patch rename như bên dưới : 


![enter image description here](https://upanh.vn-zoom.org/images/2019/09/11/Screen-Shot-2019-09-11-at-9.47.13-PM.png)

*Hầu hết các trường hợp không cần patch **rename  XHC1 to XHCI**, đối với 1 số laptops quản lý bởi cổng XHCI cần rename, còn lại hầu hết đều sử dụng XHC, nên không cần rename*

 - SMBIOS của laptop đều sử dụng config trong repo của RehabMan nên có thể bỏ qua bước này
 - Override Method _DSM  và _OSI : sử dụng SSDT-XHC và SSDT-XOSI.aml trong gói hotpatch của RehabMan kèm patch rename sau trong config :
 

![enter image description here](https://upanh.vn-zoom.org/images/2019/09/11/Screen-Shot-2019-09-11-at-10.00.19-PM.png)
 - List patch rename :
 
 **Item 1:**  
Comment: **change EHC1 to EH01**  
Find: <45484331>  
Replace: <45483031>  
  
**Item 2:**  
Comment: **change EHC2 to EH02**  
Find: <45484332>  
Replace: <45483032>  
  
**Item 3:**  
Comment: **change XHC1 to XHC_**  
Find: <58484331>  
Replace: <5848435f>  
  
**Item 4:**  
Comment: **change _DSM to XDSM**  
Find: <5f44534d>  
Replace: <5844534d>  
  
**Item 5:**  
Comment: **change _OSI to XOSI**  
Find: <5f4f5349>  
Replace: <584f5349>
 - Reboot lại hackintosh để kext và hotpatch có tác dụng
 - Check tình trạng inject các cổng USB với **IO Registry Explorer**
 
 ![enter image description here](https://upanh.vn-zoom.org/images/2019/09/11/uia_exclude_ss-excludeUSR1USR2-injection.png)

## Tìm và xác định các port USB hoạt động, loại bỏ các port thừa với Hackintool

![All your files and folders are presented as a tree in the file explorer. You can switch from one to another by clicking a file in the tree.](https://upanh.vn-zoom.org/images/2019/09/11/Screen-Shot-2019-09-11-at-10.12.24-PM.png)

 - Mở hackintool qua tab USB
 - Tiến hành thử các cổng USB với các thiết bị 2.0, 3.0 và TypeC kết hợp với check tình trạng hoạt động trong IO Registry Explorer
 - Cổng vào hoạt động sẽ hiển thị màu xanh lá như hình
 - Riêng Webcam là một dạng cổng kêt nối 2.0 với conector là internal nên chắc chắn phải để là internal
 - Video hướng dẫn ví dụ :
