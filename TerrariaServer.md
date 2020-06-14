1. Xshell连接阿里云服务器

2. 查看内存：

   `free -m`

3. 设置swap，防止内存不足：

   `swapoff -a`

   `dd if=/dev/zero of=/root/swapfile bs=1M count=2048`

   `mkswap /root/swapfile`

   `swapon /root/swapfile`

4. 添加开机自启：

   `vi /etc/fstab`

   在文件末尾加入：

   `/root/swapfile swap swap defaults 0 0`

   重启服务器重新查看内存swap是否配置成功

5. 安装必备工具：

   `yum -y install "wget" "unzip" "screen" "vim"`

6. 安装tshock服务器（可选）：

   ```shell
   #安装mono：https://www.mono-project.com/download/stable/#download-lin-centos
   rpmkeys --import "http://pool.sks-keyservers.net/pks/lookup?op=get&search=0x3fa7e0328081bff6a14da29aa6a19b38d3d831ef"
   su -c 'curl https://download.mono-project.com/repo/centos7-stable.repo | tee /etc/yum.repos.d/mono-centos7-stable.repo'
   yum install mono-devel
   #安装tshock：https://github.com/Pryaxis/TShock/releases
   unzip TShock4.4.0_Pre11_Terraria1.4.0.5.zip
   screen mono Terrariaserver.exe
   ```

7. 安装官方服务器（可选）：

   ```shell
   cd /opt
   wget "https://terraria.org/system/dedicated_servers/archives/000/000/038/original/terraria-server-1404.zip"
   #新建一个目录
   mkdir /root/terraria-server
   #解压
   unzip terraria-server-1404.zip -d /root/terraria-server
   #进入到服务器目录中
   cd /root/terraria-server/1404/Linux
   #给服务器运行文件设置权限
   chmod 744 TerrariaServer.bin.x86_64
   #创建完即可ctrl+a+d进行detach再关闭ssh连接，会话会持续运行
   screen -S terrariaServer
   ./TerrariaServer.bin.x86_64 
   #重连会话
   screen -R terrariaServer
   ```

8. 服务器传递地图：

   `find / -name 2083Master.wld #查找地图存储位置`

   `scp  /root/.local/share/Terraria/Worlds/2083Master.wld.bak root@175.24.112.197:/root/.local/share/Terraria/Worlds`