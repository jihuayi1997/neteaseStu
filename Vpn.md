### V2Ray + Web­socket + TLS + Ng­inx

1. 租一个境外服务器，一个域名，进行解析

2. 本地下载V2Ray客户端：https://github.com/2dust/v2rayN/releases

3. 配置服务器：

   `wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh`

   `touch v2ray_ins.log && wget https://gitee.com/zhou894267028/ziyong/raw/master/install.sh &&  bash install.sh`

   ```shell
   1
   y
   Y
   www.jhyserver.top
   443
   2
   3
   ```

4. 客户端二维码或者url链接连接服务器，选择PAC代理

