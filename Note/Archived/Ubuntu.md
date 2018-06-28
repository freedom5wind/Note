># 防火墙配置
* 配置文件：/etc/sysconfig/iptables
* 重启服务：service iptables restart
***
># 用户管理
1. 创建用户：useradd
2. 设置密码：passwd
3. 查看账户信息：id
4. 创建用户目录：mkdir /home/freedom5wind
5. 更改文件夹所有者：chown freedom5wind freedom5wind/
***
># Ubuntu更改用户shell
1. vi /etc/passwd
2. 在对应用户后修改shell，如/bin/bash
***
># sudo管理
>visudo
>在root行下添加用户名加相同格式的设置
***
># 终端字体大小控制
* ctrl+shift+'+'
* ctrl+'-'