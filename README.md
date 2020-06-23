# Notes4Linux
Linux(Ubuntu1804)使用中遇到的一些问题和解决办法

+ Linux修复（添加）Windows 10引导项（错的，不可行）
  + <code>sudo fdisk -l</code> 找到系统安装磁盘与分区
  + 完成第一步后，编辑grub配置，<code>sudo vi /boot/grub/grub.cfg</code>
  + 用下面的代码替代<code>### BEGIN /etc/grub.d/40_custom ###</code>和<code>### END /etc/grub.d/40_custom ###</code>之间原有的代码即可（位置可能不重要）。
  ```
    menuentry "Windows 10" {
      insmod part_msdos
      insmod ntfs
      set root='(sd1,msdos1)'
      chainloader +1    
  }
  ```
  + 这里特别需要注意的是这行
    ```
      set root='(sd1,msdos1)'
    ```
  + 举例，如果win10所在分区为<code>sda1</code>,则该行的配置为<code>set root='(sd1,msdos1)'</code>
  + 保存文件后执行下面语句
    ```
    sudo update-grub
    ```
+ 问题二 新安装Ubuntu后要做些什么？
  + 步骤一
    + 更新内核
  
  + 步骤二
