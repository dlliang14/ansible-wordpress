
## ansible部署wordpress集群

- 本项目是使用ansible部署wordpress集群，包括负载均衡、web服务器、数据库服务器、nfs服务器
- 项目经用参考，是默认5.5的mysql版本，如果需要其他版本请自行修改
- TODO mysql主从复制
- TODO nfs高可用

```bash
目前机器的配置和ip如下，自行更改

- 控制主机
10.0.0.11 A_m1

- 被控主机 ！！！
10.0.0.12 A_lb1
10.0.0.13 A_nfs1
10.0.0.14 A_web1
10.0.0.15 A_web2
10.0.0.16 A_db1
```

## 开始使用

```bash

# 控制机安装ansible
yum install -y ansible

# 安装mysql插件
ansible-galaxy collection install community.mysql

# 修改hosts文件 主机清单里的ip
web_group可以自行扩充，其他暂不支持

# 修改group_vars/all.yml 变量

# 根据变量里的域名 创建ssl 下面是简易自签证书命令
openssl genrsa -out server.key 2048 
openssl req -new -x509 -days 3650 -key server.key -out server.crt -subj "/C=CH/ST=mykey/L=mykey/O=mykey/OU=mykey/CN=domain1/CN={你的域名}/CN=domain3"

mv server.* lb/files/

# 执行playbook
ansible-playbook run.yml  -i hosts 

# 访问域名 测试

# 刷新dns
ipconfig /flushdns

# 修改hosts C:\Windows\System32\drivers\etc\hosts
10.0.0.12 www.dll.com # ansible_wp

# 访问域名
https://www.dll.com/wordpress/
https://10.0.0.12/wordpress/

# 正常的话，自动跳转
https://10.0.0.12/wordpress/wp-admin/install.php

```

## 故障排除

```bash
# nginx配置
vi /etc/nginx/nginx.conf

# nginx日志
tail -f /var/log/nginx/access.log

# web端口
netstat -tunlp | grep nginx

# mysql 权限
mysql -uroot -p
select user,host from mysql.user;
```