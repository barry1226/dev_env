# dev_env
## 背景
 - 今天买了thinkpad E490(用不惯mac os, 又买不起T系列高配)
 - 不想装双系统，用virtualbox装了ubuntu 18.04,然后在windows上用putty访问，并配置samba，方便用vscode编辑文件(vim水平只能写commit-msg的程度)
## 搭建过程
### 安装virtualbox，安装ubuntu 18.04
1. 非常顺利，感觉比VMware好用
2. 过程略
### 配置sshd服务
1. 默认安装后ubuntu有一个NAT网卡，进入该网卡的高级设置
2. 进入端口转发配置，配置子系统端口22，主机端口配置一个空端口即可，我选择了8080
3. ssh -p 8080 $USERNAME@localhost登录即可
4. localhost即127.0.0.1
### 配置samba服务
1. 设置网卡2为桥接网卡
2. 安装samba.

    sudo apt-get install samba

    sudo apt-get install smbclient

3. 添加samba用户

    1>, sudo useradd samba

    2>, sudo smbpasswd -a samba

4. 配置samba

    1>, 创建共享目录

    2, sudo cp /etc/samba/smb.conf /etc/samba/smb.conf_bak

    3, sudo vi /etc/samba/smb.conf, 并把下面内容复制到文件的最后，共享目录路径要改成你自己的：

        [paul_ubuntu_home]
        comment = paul share home
        path = /home/paul
        public = yes
        browseable = yes
        guest ok = yes
        writable = yes
        create mask = 0777

5. 重启samba服务

    sudo /etc/init.d/smbd restart

6. 在Windows中访问ubuntu的samba共享目录

    在explorer的地址栏里输入\\192.168.31.103(你自己的unbuntu网卡地址)，回车，就可以访问共享目录了。 
此部分参考：https://blog.csdn.net/lihewei126/article/details/82827861