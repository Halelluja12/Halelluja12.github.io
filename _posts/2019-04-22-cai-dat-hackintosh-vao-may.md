---
  title: Cài đặt hackintosh vào máy
  author: Nguyen Van Hung
  tags: hackintosh install disk-utility
  aside:
    toc: true
---

Hiện tại máy mình đã xóa windows và cái điện thoại xiaomi cùi bắp chụp ảnh không tốt lắm nên mình sẽ lấy ảnh từ nguồn ngoài hoặc không có ảnh để hướng dẫn chi tiết.

## Chuẩn bị phân vùng ổ cứng cài đặt macOS
__Hãy backup dữ liệu__, vì sẽ phải format ổ cứng hoăc phân chia phân vùng ổ cứng. Cũng có thể sẽ phải cài lại win. Bạn phải tự trách nhiệm với dữ liệu của bạn.

__Lưu ý:__ macOS không có khái niệm format mà chỉ có erase, mình sẽ nói là xóa ổ cứng, xóa phân vùng.

Mình sẽ chỉ hướng dẫn cài UEFI thôi nên yêu cầu ổ cứng của bạn sẽ ở dạng GPT. __macOS yêu cần có phân vùng EFI >= 200MB__, nếu không thì bạn sẽ không thể xóa phân vùng cài sang định dạng HFS hoặc APFS để cài. Do đó tùy vào trình trạng ổ cứng của bạn, sẽ các phương án sau:
- Cài lên nguyên cả ổ cứng:
 Bạn không cần chuẩn bị gì, tí nữa bạn sẽ xóa cả ổ cứng bằng disk utility như lúc làm usb vì nó chỉ chứa macOS. Nếu có sẵn hoặc cài lại windows lên ổ cứng khác yêu cầu dùng windows UEFI, clover sẽ không boot windows legacy được.
- Cài macOS lên một phân vùng ổ cứng:
  + Nếu bạn đang dùng Windows Legacy, ổ cứng dạng MBR thì tốt nhất bạn nên xóa cả ổ cứng để cài macOS trước rồi cài lại Windows UEFI sau. Bạn vẫn có thể tìm những cách khác để chuyển từ legacy sang uefi và hãy đảm bảo bạn tạo phân vùng EFI >= 200MB.
  + Nếu bạn đang có Windows UEFI và có phân vùng recovery 500MB và efi 100MB ở trước phân vùng win. Cần EFI 200MB nên bạn sẽ xóa phân vùng recovery và thêm vào EFI. Sau đó chia ra một phân vùng dành cho macOS, format NTFS và đặt tên cho nó (không đặt tên tí nữa nó không hiện trong disk utility). Hãy dùng [MiniTool Partition Wizzard](https://www.partitionwizard.com/free-partition-manager.html) để thực hiện bước này.

## Boot USB và cài đặt
  Hãy thực hiện các bước sau:

  + Cắm usb vào, tốt nhất không cắm dây lan vào máy
  + Bật nguồn và vào boot option, tùy main và hãng máy mà phím tắt khác nhau, nó có hiển thị trên màn hình lúc mới bật hoặc bạn có thể hỏi google
  + Chọn USB của bạn trong danh sách (hãy chọn cái boot mà có chữ UEFI đứng trước cái tên USB)
  + Clover sẽ khởi động, bạn hãy di chuyển trái phải và chọn __Boot macOS Install from install_osx__ (mình đặt lại tên là vì chọn cái này cho dễ hướng dẫn, vì có nhiều phiên bản macOS)
  + Chờ đợi cho một màn hình đen toàn chữ chạy, và cảm nhận mình như một __hacker__
    - Nếu bạn bị dừng ở chỗ nào đó và kèm theo một màn hình đen với một vòng tròn gạch chéo thì xin chia buồn bạn đã bị panic. Cần chỉnh sửa lại clover hoặc bios.
    - Nếu bị panic bạn hãy cố chụp lại lỗi và đăng lên group hỏi. Bạn nên thử lại bằng cổng usb khác hoặc thay usb tạo lại bộ cài hoặc xem xét lại bios kĩ trước khi hỏi.
  + Sau khi màn hình chữ chạy xong tiếp đó là logo Apple và thanh loading thì chúc mừng bạn đã boot vào được bộ cài.
  ![lang](/assets/images/hackintosh/install/lang.png)
  + Chọn __Disk Utility__
  ![USB Install](/assets/images/hackintosh/install/usb-disk-utility.png)
  + Giờ đến lúc xóa ổ cứng hoặc phân vùn để cài macOS, đặt tên thế nào là tùy bạn (ở đây mình đặt nó là "macOS")
    - Nếu bạn sạch cả ổ cứng thì hãy chọn View -> Show All Devices -> chọn tên ổ cứng của bạn ở cột bên trái -> Erase -> lựa chọn như hình sau rồi chọn Erase
    ![Erase Disk](/assets/images/hackintosh/install/erase-disk.png)
    - Nếu bạn chỉ xóa phân vùng ổ cứng đã chuẩn bị (cần efi >= 200MB mới xóa theo cách này được) thì chọn tên phân vùng bạn đã chọn ở cột bên trái -> Erase -> lựa chọn như hình sau rồi chọn Erase
    ![Erase Partition](/assets/images/hackintosh/install/erase-partition.png)
  + Xóa xong rồi thì đóng disk-utility lại (bấm nút đỏ trên góc)
  + Quay trở lại màn hình cũ, giờ bạn chọn __Install macOS__
  + Chọn Continue cho đến khi nào ra cái màn hình chọn phân vùng cài, lúc này bạn chọn phân vùng tên là macOS (nếu đặt tên khác thì chọn tên đó), chọn Continue và quá trình cài sẽ bắt đầu.
  + Chờ đợi khoảng 15-30 phút, tùy vào tốc độ ổ cứng của bạn.

## Boot macOS trên ổ cứng lần đầu
  + Sau khi cài đặt xong máy sẽ tự khởi động lại, bạn chú ý sẽ phải vào boot vào usb trước rồi mới boot được macOS từ ổ cứng.
  + Di chuyển trái phải chọn ổ có tên là __Boot macOS from macOS__, macOS là tên mình đặt, bạn đặt tên khác thì chọn Boot __macOS from \<tên bạn đặt\>__
  + Chờ màn hình toàn chữ chạy đến cái màn hình loading
  + Làm theo hình cho dễ

  |----|----|
  |![1](/assets/images/hackintosh/install/1.png)|![1](/assets/images/hackintosh/install/2.png)|
  |![1](/assets/images/hackintosh/install/3.png)|![1](/assets/images/hackintosh/install/4.png)|
  |![1](/assets/images/hackintosh/install/5.png)|![1](/assets/images/hackintosh/install/6.png)|
  |![1](/assets/images/hackintosh/install/7.png)|![1](/assets/images/hackintosh/install/8.png)|
  |![1](/assets/images/hackintosh/install/9.png)|![1](/assets/images/hackintosh/install/10.png)|
  |![1](/assets/images/hackintosh/install/11.png)||

## Những việc cần làm sau cài đặt lên ổ cứng
  Như vậy là đã xong quá trình cài đặt vào ổ cứng. Tiếp theo cần phải làm một số việc để mac có thể chạy ổn định. Mình sẽ viết bài sau.
  + Cài đặt clover lên ổ cứng và chỉnh sửa
  + Đối với card NVIDIA dòng Maxwell và bạn cần cài đặt web driver nhận card rời.
  + Thêm các kext cần thiết và chỉnh sửa config
  + Patch DSDT/SSDT, bắt buộc đối với laptop, PC có thể không cần patch toàn bộ nhưng ít nhất bạn cần patch cổng usb.

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
