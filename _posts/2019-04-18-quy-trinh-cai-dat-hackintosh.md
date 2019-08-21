---
title: Quy trình cài đặt Hackintosh
tags: hackintosh install
author: Nguyen Van Hung
toc:
---

# Hãy hình dung trước khi bước đi.

## Quy trình cơ bản để bắt đầu cài:
1. __Chuẩn bị kiến thức cơ bản (đọc các guide trước), backup dữ liệu và USB.__ Nhấn mạnh về dữ liệu, bạn cần phải để ý đến dữ liệu vì cài hackintosh sẽ phải xóa ổ cứng hoặc phân vùng, rất dễ hỏng windows và phải cài lại.
2. __Xác định chi tiết phần cứng.__ Cần phải xác đĩnh rõ phần cứng để xem có cài được hackintosh, dùng kext nào, config thế nào, ...
3. __Chuẩn bị file cần thiết và macOS.__ macOS ở đây có thể là máy ảo, real mac hay một máy hackintosh khác đều được. Hiện tại có thể không cần thiết phải chuẩn bị macOS nữa vì có một số cách có thể tạo usb cài hackintosh mà không cần tới macOS. Tuy nhiên để hiểu và ít lỗi nhất bạn vẫn nên tạo usb cài trên macOS.
4. __Tạo USB cài đặt Hackintosh.__ Đây là bước gần như quan trọng nhất vì cần phải tạo usb boot vào được mac trước rồi mới cài đặt và sửa lỗi được.
5. __Setup bios cho hackintosh.__
6. __Boot USB và tiến hành cài đặt.__
7. __Tinh chỉnh Clover và patch DSDT/SSDT.__ Boot usb vào cài được macOS và ổ cứng không có nghĩa là xong, sẽ có nhiều lỗi xảy ra và bạn cần phải sửa nó chủ yếu là thông qua clover và dsdt/ssdt. Đối với PC thì nếu có thể sửa được lỗi thông qua clover thì sẽ không cần patch dsdt/ssdt.
8. __Hoàn thiện máy sau cài đặt và sử dụng__

# Hình dung xong rồi bắt đầu làm
