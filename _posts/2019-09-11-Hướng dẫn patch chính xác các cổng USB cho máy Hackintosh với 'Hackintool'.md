---
title: Hướng dẫn patch chính xác các cổng USB cho máy Hackintosh với 'Hackintool'
tags: hackintosh USB
author: Nguyen Van Hung
aside:
  toc: true
---


**"Still waiting for root device"** sinh ra khi phân vùng được coi là System tự nhiên bị ngắt khỏi hệ thống. Ví dụ boot bộ cài mà cổng usb bị ngắt, boot ổ cứng mà cổng sata bị ngắt.  
Bản thân không có một ngắt vật lý nào hết, mà là do kext của Mac không sử dụng được cổng đó nên nó chờ System quay lại, nó sẽ lắng nghe tất cả các port.  
 Lý do sinh ra guide này vì lỗi huyền thoại trên khá là bất tiện và rất hay gặp phải bởi newbie, mặc dù có thể khắc phục tạm thời bằng Limited patch port USB on-the-fly như nó không ổn định và không sử dụng được lâu dài. 

![enter image description here](https://upanh.vn-zoom.org/images/2019/09/11/23ol9v1.jpg)

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

<iframe width="560" height="315" src="https://www.youtube.com/embed/VMBlKsDp23E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*Sau khi config đúng cổng, tiến hành xoá bỏ các cổng USB không hoạt động (không sáng màu xanh) sau đó tiến hành export SSDT-UIAC.aml config các cổng đã làm vừa nãy cho vào Clover/ACPI/Patched*

##  USB power property injection for Sierra & Later MacOS version

**Chú ý** : *bước này chỉ cần làm với các bản macOS từ Sierra trở về sau, do từ 10.12 Apple sử dụng cách quản lý năng lượng cho các cổng USB khác với **AppleBusPowerControllerUSB** thay vì **AppleBusPowerController** như trước* 

Có hai trường hợp xảy ra : 
- PC có ECDT.aml  OEM ACPI với Device EC trong bảng DSDT hiệu quả ta làm như sau :
- Nhấn F4 từ màn hình clover để dump toàn bộ các bảng ACPI, tiến hành kiểm tra trong Clover/APCI/Origin/ thấy một bảng ACPi có tên ECDT.aml mở lên là kiểm tra đường dẫn tơi device EC

![enter image description here](https://upanh.vn-zoom.org/images/2019/09/11/Screen-Shot-2019-09-11-at-11.44.03-PM.png)

- Nếu trùng với đường dẫn của device EC trong DSDT chứng tỏ device EC có hiệu quả chỉ cần patch rename EC0 to EC trong config.plist là được, một số trường hợp là H_EC hoặc ECDT

> **Trường hợp không có Device EC0, H_EC..vv trong DSDT hoặc giá trị của Method_STA trả về return (zero) trong device EC0 ta tiến hành Inject Fake EC device**
- Tạo SSDT-EC.dsl với nội dung như sau :
```
// Inject Fake EC device
DefinitionBlock("", "SSDT", 2, "hack", "EC", 0)
{
Device(_SB.EC)
{
Name(_HID, "EC000000")
}
}
//EOF
```
- Tạo SSDT quản lý USBX device 
- SSDT-USBX sẽ tự động tạo ra cùng với SSDT-UIAC trên hackintool đối với trường hợp không có Method quản lý device USBX trong IOUSBHostFamily.kext/Contents/Info.plist
```
// USB power properties via USBX device
DefinitionBlock("", "SSDT", 2, "hack", "USBX", 0)
{
Device(_SB.USBX)
{
Name(_ADR, 0)
Method (_DSM, 4)
{
If (!Arg2) { Return (Buffer() { 0x03 } ) }
Return (Package()
{
// these values from iMac17,1
"kUSBSleepPortCurrentLimit", 2100,
"kUSBSleepPowerSupply", 5100,
"kUSBWakePortCurrentLimit", 2100,
"kUSBWakePowerSupply", 5100,
})
}
}
}
//EOF
```

## Sau khi hoàn thành các bước trên các bạ có thể bỏ Patch on the fly limited port USB trong config.plist 

- Kiểm tra power properties inject trong system infomation :
- Mở IO Reg kiểm tra **AppleBusPowerController**
![enter image description here](https://upanh.vn-zoom.org/images/2019/09/12/Screen-Shot-2019-09-12-at-10.57.28-PM.png)

- Tiến hành cắm cable thiết bị iphone hoặc ipad vào hackintosh kiểm tra system information/USB các thông số sau, nếu hiện thì đã thành công

>    Current Available (mA):      
>    Current Required (mA):    
>    Extra Operating Current (mA):    
>    Sleep current (mA):
