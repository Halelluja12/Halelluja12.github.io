---
title: Chuẩn bị kiến thức cơ bản
tags: hackintosh base knowledge
author: Nguyen Van Hung
aside:
  toc: true
---
Để có thể tự cài đặt hackintosh bạn cần phải có kiến thức cơ bản về máy tính, ít nhất bạn nên biết cài win uefi. Trong blog này mình sẽ chỉ nói về cài hackintosh chuẩn uefi-gpt mà thôi, không support legacy. Bạn nào biết rồi thì có thể bỏ qua đọc bài viết khác.

## Legacy - UEFI, MBR - GPT
  Bạn cần hiểu về những khái niệm này để có thể cài win uefi và hackintosh uefi. Bạn vui lòng đọc bài viết sau từ blog của __anh-dv__:
  - [Tìm Hiểu Và So Sánh MBR Với GPT Và Legacy (Bios) Với UEFI/EFI](https://anh-dv.com/thu-thuat-hay/so-sanh-mbr-voi-gpt-va-legacy-voi-uefi)

## Windows GPT - UEFI
  Càn phải biết cài win uefi, biết setup bios. Cài hackintosh rất dễ hỏng win nên bạn cần biết để còn cài lại. Bạn vui lòng đọc bài viết sau từ __gocinfo.com__:
  - [Hướng dẫn cài đặt Windows 10 chuẩn UEFI/GPT bằng USB WinPE](https://gocinfo.com/huong-dan-cai-dat-windows-10-chuan-uefi-gpt-usb-winpe.html)

## Một số thuật ngữ trong hackintosh
  - __Clover__ là một bootloader để có thể chạy macOS, đây là thành phần quan trọng nhất trong hackintosh. Clover sẽ đánh lừa macOS răng đây là cái real mac và macOS sẽ chạy. Các hệ điều hành đều có bootloader riêng của nó, windows có windows bootloader, linux có grub, ...
  - __Kext__ là driver ở trong macOS. Trong macOS không có khái niệm driver mà thay vào đó là kext.
  - __config.plist__ là file cấu hình, thiết lập của clover bạn đây sẽ là file cần chỉnh sửa nhiều nhất để có thể boot vào macOS.
  - __DSDT/SSDT__ là các bảng giao thức điều khiển thiết bị, được lưu trong BIOS/UEFI của máy. DSDT, SSDT mô tả các thiết bị có trong máy, và cung cấp các hàm vận hành thiết bị. Các thiết bị, hàm này sẽ được sử dụng bởi driver trong các hệ điều hành như Windows, OS X, Linux để điều khiển thiết bị. Nếu không có các bảng này, hệ điều hành sẽ không thể vận hành được.
  - __Patch DSDT/SSDT__ là chỉnh sửa DSDT/SSDT để Hackintosh có thể nhận diện phần cứng và hoạt động đúng cách. macOS chỉ hỗ trợ các phần cứng do Apple quy định. Các thiết bị, hàm vận hành trong DSDT trên máy Mac được tối ưu hóa cho hệ điều hành, với những quy định chặt chẽ. Trên hệ thống Hackintosh, một số thiết bị như card đồ họa, âm thanh, pin không hoạt động, vì bảng DSDT trên máy không tuân theo chuẩn của macOS.

  __Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
