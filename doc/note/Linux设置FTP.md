### Linux添加FTP用户并设置权限

在linux中添加ftp用户，并设置相应的权限，操作步骤如下：
1、环境：ftp为vsftp。被限制用户名为test。被限制路径为/home/test
 
2、建用户，命令行状态下，在root用户下：
    
    运行命令：“useradd -d /home/test test”　　//增加用户test，并制定test用户的主目录为/home/test
    运行命令：“passwd test”　　//为test设置密码，运行后输入两次相同密码
    
3、更改用户相应的权限设置：
    
    运行命令：“usermod -s /sbin/nologin test”　　//限定用户test不能telnet，只能ftp
    运行命令：“usermod -s /sbin/bash test”　　//用户test恢复正常
    运行命令：“usermod -d /test test”　　//更改用户test的主目录为/test

4、限制用户只能访问/home/test，不能访问其他路径
   
    修改/etc/vsftpd/vsftpd.conf如下：
    chroot_list_enable=YES
    chroot_list_file=/etc/vsftpd/vsftpd.chroot_list
    编辑上面的内容
    第一行：chroot_list_enable=YES　//限制访问自身目录
    第三行：编辑vsftpd.chroot_list。根据第三行说指定的目录，找到chroot_list文件。（因主机不同，文件名也许略有不同）
    编辑vsftpd.chroot_list，将受限制的用户添加进去，每个用户名一行

5、重启服务器

    改完配置文件，不要忘记重启vsFTPd服务器
    运行命令：/etc/init.d/vsftpd restart

6、如果需要允许用户修改密码，但是又没有telnet登录系统的权限：

    运行命令：“usermod -s /usr/bin/passwd test”　　//用户telnet后将直接进入改密界面
