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

6. 安装泰拉瑞亚服务器：

   `cd /opt`

   `wget "https://terraria.org/system/dedicated_servers/archives/000/000/037/original/terraria-server-1403.zip"`

   ` mkdir /opt/terraria-server #新建一个目录` 

   `unzip terraria-server-1403.zip -d /opt/terraria-server`

   `cd /opt/terraria-server/1403/Linux #进入到服务器目录中`

   `chmod 744 TerrariaServer.bin.x86_64 #给服务器运行文件设置权限`

   `screen -S terrariaServer`

   `./TerrariaServer.bin.x86_64 `

   创建完即可ctrl+a+d进行detach再关闭ssh连接，会话会持续运行

   `screen -R terrariaServer #重连会话`
   
7. 服务器传递地图：

   `find / -name 2083Master.wld #查找地图存储位置`

   `scp  /root/.local/share/Terraria/Worlds/2083Master.wld.bak root@47.114.182.39:/home/.local/share/Terraria/Worlds`