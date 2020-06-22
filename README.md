# Notes4Linux
Linux(Ubuntu1804)使用中遇到的一些问题和解决办法

+ Linux修复（添加）Windows 10引导项
  + <code>sudo fdisk -l</code> 找到系统安装磁盘与分区
  + 完成第一步后，编辑grub配置，<code>sudo vi /boot/grub/grub.cfg</code>

