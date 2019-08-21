---
  title: Setup Bios cho Hackintosh
  author: Nguyen Van Hung
  tags: hackintosh bios setup
  aside:
    toc: true
---
Vì có quá nhiều hãng máy và main nên mình không thể làm cụ thể từng loại bios được. Bạn cần tìm và chỉnh sửa một số mục sau trong bios:

## Đối với PC

* XHCI Handoff: Enabled \(nếu có, một số main sẽ không có lựa chọn này\)
* OS Type: Other \(nếu có\)
* Secure Boot: Disabled \(nếu không thấy đâu thì sẽ thấy cái OS Type bên trên chỉnh nó về other là được\)
* Legacy/CSM support: Enable \[Nhiều main tắt cũng được nhưng nhiều main tắt thì boot màn hình sẽ bị lỗi, bật cho chắc\]
* Fast Boot: Disable
* SATA Mode: AHCI
* VT-d: Disable
* Nếu bạn dùng Nvidia/AMD GPU:
  * __KHÔNG__ kết nối màn hình với các cổng DP/HDMI/VGA/DVI trên main.
  * Phần Graphics Settings, Main Display, Initial Display/Graphic: PEG hoặc PCIE (Chọn đến khe cắm card rời)
  * Disable những lựa chọn liên quan đến card onboard như: Hybrid Graphics, Dual Graphics, DVMT size ...
* Nếu bạn dùng Intel HD Graphics GPU:
  * Phần Graphics Settings, Main Display, Initial Display/Graphic: IGD hoặc IGFX \(chọn card onboard\)
  * DVMT pre-alloc, Graphic Memory: 64MB \(hoặc cao hơn, phần lớn thì 64MB là đủ\)
  * DMVT total/size/apertures/whatever: MAX

## Đối với laptop

* Disabled Secure Boot
* Disable Fast Boot
* SATA Mode: AHCI
* Disable VT-d, VT-x nên enable không thì không chạy máy ảo được.
* Security Chips/Security modules: Disabled \(nếu có thể, chúng có thể gây lỗi khi boot\)
* DVMT-prealloc, Graphics Memory: 64MB \(nếu có thể, một số bios laptop không có lựa chọn này mặc định ở 32MB thì bạn cần phải patch framebuffer\)
* Nếu màn hình của bạn là 2K, 4K thì cần chỉnh Graphics Memory lên 128MB hoặc cao hơn.
* Legacy/CSM support: Enable \(nếu tắt hay bị vỡ hình khi boot, nếu boot bình thường thì cứ tắt\)
* Disable LAN/WLAN/WWAN boot/wake
* Disable wake on usb

## Tiến hành cài đặt
Setup xong bios rồi thì cắm usb vào mà boot cài thôi!

__Nếu bạn cảm thấy bài viết hay và có ích hãy ủng hộ star github và donate để mình có thêm động lực viết bài.__
