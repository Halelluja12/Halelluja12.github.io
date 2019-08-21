---
  title: Boot mặc định vào Clover
  author: Nguyen Van Hung
  tags: hackintosh post-install clover boot default
  aside:
    toc: true
---

Tùy vào main mà việc boot vào clover sẽ có độ khó khăn khác nhau.

Sẽ có các cách sau đây theo thứ tự từ dễ tới phức tạp.

## Boot từ BOOTX64.efi

Giải thích một chút hệ thống UEFI sẽ tự tìm các file đuôi .efi và chứa boot loader để chạy. Mặc định nếu bạn chọn boot option là ổ cứng nào đó thì hệ thống sẽ boot từ file EFI/BOOT/BOOTX64.efi .

![bootx64](/assets/images/hackintosh/boot/bootx64.png)

Nếu ổ cứng chỉ có chứa duy nhất macOS thì chỉ cần chỉnh boot của cái ổ cứng đó lên đầu là được vì sau khi chạy clover installer thì nó đã tự tạo EFI/BOOT/BOOTX64.efi tương ứng với EFI/CLOVER/CLOVERX64.efi.

Nếu ổ cứng của bạn đã có chứa windows trước, thì BOOTX64.efi lúc này lại là boot của windows. Bạn hãy thử copy CLOVER/CLOVERX64.efi qua thư mục BOOT và đổi tên nó thành BOOTX64.efi sau đó chỉnh boot của ổ cứng lên đầu. Cách này thành công thì ít mà thất bại thì nhiều.

## Thêm boot option
Đa số BIOS đều cho phép thêm boot option. Nếu không thể boot mặc định từ BOOTX64.efi thì hãy tìm trong BIOS dòng tùy chọn này __Add New Boot Option__, nếu không thấy vui lòng chuyển qua cách cuối cùng.

![add](https://2.bp.blogspot.com/-deg_-y4Aunk/VEYuB8HOqmI/AAAAAAAAAz0/-34xaLPTHjE/s1600/IMG_0225_zpsc090d8b7.jpg)

Sau đó thêm một boot option và chỉnh đường dẫn như sau:

Chọn ổ cứng có EFI chứa clover -> EFI -> CLOVER -> CLOVERX64.efi

![custom](https://3.bp.blogspot.com/-7_lMzbDn9os/VEYuPKsqV2I/AAAAAAAAAz8/FTZ4dGr8AaI/s1600/IMG_0226_zps2ef2c14c.jpg)

Cuối cùng thì chỉnh boot option mới lên đầu.

## Boot từ bootmgfw.efi

Nếu bạn đã cài windows lên ổ cứng và trong BIOS không thể thêm boot option (mấy cái máy ACER hay thế này), không thấy boot ổ cứng đâu chỉ thấy Windows Boot Manager ở đầu tiên thì chỉ còn cách này. Nhưng cách này thì đảm bảo chắc chắn bạn có thể boot vào clover.

![cloverx64](/assets/images/hackintosh/boot/cloverx64.png)

Cách làm như sau:

1. Vào /EFI/Microsoft/Boot/ đổi tên file __bootmgfw.efi__ thành
__bootmgfw-orig.efi__

2. Vào /EFI/CLOVER, copy __CLOVERX64.efi__ sang /EFI/Microsoft/Boot/ và đổi tên nó thành __bootmgfw.efi__

3. Giữ nguyên Windows Boot Manager ở đầu trong list boot option

![bootmgfw](/assets/images/hackintosh/boot/bootmgfw.png)

Nguyên lí rất đơn giản là mặc định nó boot vào windows thì thay boot windows thành boot của clover là được. Bước một khá quan trọng nếu bạn không làm theo thì sẽ mất boot windows.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
