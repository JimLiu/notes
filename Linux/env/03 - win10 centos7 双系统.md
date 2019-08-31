## 双系统安装
### 参考资料
联想 thinkpad s2: bios 模式：http://www.udaxia.com/upqd/10119.html
制作启动盘：https://www.jianshu.com/p/38e6be8efecf
    关闭快速启动模式
安装双系统：https://blog.csdn.net/flyer1011/article/details/78185509
安装双系统：https://blog.csdn.net/Gatherfly/article/details/51864247
安装双系统：https://blog.csdn.net/luanpeng825485697/article/details/80274399
centos 启动引导 win10: https://blog.csdn.net/q260864798/article/details/53502242
安装时问题解决：https://blog.csdn.net/Gatherfly/article/details/51864247
一整套安装方法：
https://www.cnblogs.com/NineKit/p/9649745.html
https://www.cnblogs.com/NineKit/p/9649746.html
https://www.cnblogs.com/NineKit/p/9649748.html

### 挂载硬盘
#### https://blog.csdn.net/dgyingling/article/details/66477272
#### http://blog.sina.com.cn/s/blog_437ff56b0101e5kj.html

挂载硬盘时，需要参考第二个连接： vim /etc/fstab
    /dev/sda2     /mnt/win    ntfs-3g  ro 0 0
