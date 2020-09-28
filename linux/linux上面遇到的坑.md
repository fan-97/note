## 在deepin环境上运行java项目，通过JNA将so库写入到了/usr/lib64里面，但是在加载so库文件的时候，找不到库文件
### 解决办法：
1.在 `/etc/ld.so.conf` 最后一行添加 so库所在的文件夹  /usr/lib64
2.使用 `/sbin/ldconfig` 命令 刷新配置，使其生效
