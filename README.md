# ssh_vpn
# 这里提供一种翻墙新思路，解决上网问题
- 1、本地windows电脑安装贝锐蒲公英异地组网软件和git（带ssh客户端）；
- 2、香港linux（ubuntu）服务器（到阿里腾讯上租香港服务器）安装贝锐蒲公英异地组网软件；
- 3、本地windows使用ssh指令使用本地指定端口连接蒲公英局域网内的香港linux服务器ip；
- 4、本地windows设置socks指定端口代理上网
## 1、组网代理具体步骤
- 1、客户端windows下载并安装：https://d.oray.com/pgy/windows/PgyVisitor_6.3.0_x64.exe
- 2、客户端windows登录贝锐账号，没有账号先创建账号，免费的可以组网三台设备，如果家里一台电脑，公司一台电脑，服务器一台电脑，这样免费的刚刚好。
- 3、服务器linux命令行下载蒲公英：wget https://pgy.oray.com/softwares/153/download/2156/PgyVisitor_6.2.0_x86_64.deb
- 4、服务器linux命令行安装蒲公英：sudo dpkg -i PgyVisitor_6.2.0_x86_64.deb 
- 5、服务器linux命令行登录之前注册的贝锐账号：pgyvisitor login
- 6、客户端windows在蒲公英客户端软件中取得服务器蒲公英网络ip如：172.16.0.198
- 7、客户端windows打开cmd命令行输入（这里用户名是ubuntu然后输入密码）：ssh -N -D 127.0.0.1:8080 ubuntu@172.16.0.198
- 8、客户端windows编辑一个.reg后缀的文件,内容如下
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings]

"ProxyServer"="socks://127.0.0.1:8080"
```
- 9、客户端windows左下角搜索框中搜索“代理”：进入网络和Internet>代理>手动代理点上“开”代理ip填入127.0.0.1端口填入8080
- a、客户端windows双击运行reg文件注册表注入后即可使windows全局用本地socks:127.0.0.1:8080上网，如果服务器放在“外地”的话就可以用“外地”的网络了上网，腾讯，阿里，UCLOUD......都可以。
- b、如果不要全局代理上网使用火狐浏览器，火狐浏览器设置中搜代理进入网络设置下面选手动代理SOCKS主机填入127.0.0.1 SOCKS v5选项 另外“使用SOCKSv5 时代理DNS查询” 这个要勾上。
- c、为什么要用蒲公英组网，是不是多此一举？ 答：并不是，不用蒲公英直接使用服务器的外网ip测试过了会很卡，卡到无法正常使用。
  
## 2、每次输入密码很麻烦，SSH 无密码登录服务器的办法
- 本地windows命令行运行：ssh-keygen -t rsa
- 本地windows命令行到.ssh文件夹：cd .ssh
- 本地windows命令行上传公钥文件到linux服务器：scp id_rsa.pub ubuntu@172.16.0.198:~/.ssh/authorized_keys
- 本地windows命令行无密码登录linux服务器：ssh ubuntu@172.16.0.198
