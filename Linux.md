# Linux

## 基础指令

```bash
yum update										#升级包及系统内核
uname -a										#查看系统信息
cat /etc/redhat-release							#查看centos版本号
getenforce										#查看SElinux状态
systemctl is-enabled firewalld.service;echo $?	#查看防火墙是否开机自启
systemctl disable firewalld.service				#关闭防火墙开机自启
systemctl status firewalld.service				#查看防火墙状态
systemctl stop firewalld						#关闭防火墙
whoami											#查看当前用户
```

## 并发编程



## 网络编程

