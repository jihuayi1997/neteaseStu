# 网络爬虫与信息提取

## 基础知识

渲染方式：

* 服务端渲染：服务器把数据和html整合在一起，返回给浏览器
* 客户端渲染：第一次请求只要一个html骨架，第二次请求拿到数据，进行数据展示

可能遇到Proxy Error的问题（新版pip和urllib3支持了https，而windows的系统代理默认只支持http代理）

* PAC + TUN + HTTPS_PROXY=http://127.0.0.1
* PAC + TUN
  * `proxies = {"http": "http://127.0.0.1:7890", "https": "http://127.0.0.1:7890"}`
  * `requests.get(url, proxies=proxies)`

## Requests库

1. 主要方法

   `requests.request(method, url, **kwargs)`：构造一个请求，支撑下面方法

   `requests.get(url, params=None, **kwargs)`：获取HTML网页主要方法，对应HTTP的GET

   `requests.head(url, **kwargs)`：获取HTML网页头信息的方法，节约带宽，对应于HTTP的HEAD

   `requests.post(url, data=None, json=None, **kwargs)`：向HTML网页提交POST请求的方法，URL后附加新数据，对应HTTP的POST

   ![image-20191209104152946](TyporaPics/image-20191209104152946.png)

   ![image-20191209104242634](TyporaPics/image-20191209104242634.png)

   `requests.put(url, data=None, **kwargs)`：向HTML网页提交PUT请求的方法，覆盖原URL位置的资源，对应HTTP的PUT

   `requests.patch(url, data=None, **kwargs)`：向HTML网页提交局部修改请求，patch()只需提交修改字段，节约带宽，对应HTTP的PATCH

   `requests.delete(url, **kwargs)`：向HTML页面提交删除请求，对应HTTP的DELETE

2. Response对象的属性

   `r.status_code`：HTTP请求返回状态，200表示连接成功，404表示失败

   `r.text`：HTTP相应内容的字符串形式，即URL对应的页面内容

   `r.encoding`：从HTTP header中猜测的响应内容编码方式，若header中不存在charset，则认为编码为"ISO-8859-1"

   `r.apparent_encoding`：从内容中分析的响应内容编码方式

   `r.content`：HTTP响应内容的二进制形式

3. 通用代码框架

   ```python
   import requests
   
   
   def getHTMLText(url):
       try:
           # 更改请求头，防止反爬虫
           headers = {
           "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
                         "Chrome/89.0.4389.82 Safari/537.36 Edg/89.0.774.50"}
           r = requests.get(url, timeout=30, headers=headers)
           # 如果状态不是200，引发HTTPError异常
           r.raise_for_status()
           r.encoding = r.apparent_encoding
           return r.text
       except:
           return "exception occur"
   
   
   if __name__ == '__main__':
       url = "http://www.baidu.com"
       print(getHTMLText(url))
   ```

4. 实例

   百度搜索关键词提交

   ```python
   import requests
   
   url = "http://www.baidu.com/s"
   keyword = "周杰伦"
   try:
       kv = {"wd": keyword}
       r = requests.get(url, timeout=30, params=kv)
       r.raise_for_status()
       r.encoding = r.apparent_encoding
       print(r.text[-70000:-50000])
   except:
       print("异常")
   ```

   网络图片的爬取与存储

   ```python
   import requests
   import os
   
   url = "http://img.jj20.com/up/allimg/911/111G5133427/15111G33427-7-1200.jpg"
   root = "D://图片//"
   path = root + url.split('/')[-1]
   try:
       if not os.path.exists(root):
           os.mkdir(root)  # 判断根目录是否存在，如果不存在就创建一个
       if not os.path.exists(path):  # 判断文件是否存在
           r = requests.get(url)
           with open(path, 'wb') as f:
               f.write(r.content)
               f.close()
               print("文件保存成功")
       else:
           print("文件已存在")
   except:
       print("爬取失败")
   ```

   IP地址查询

   ```python
   import requests
   
   url = "https://www.ip138.com/iplookup.asp?ip="
   ip = "120.53.124.207"
   try:
       headers = {
           "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
                         "Chrome/89.0.4389.82 Safari/537.36 Edg/89.0.774.50"}
       r = requests.get(url + ip+"&action=2", headers=headers)
       r.raise_for_status()
       r.encoding = r.apparent_encoding
       print(r.text[1000:2000])
   except:
       print('爬取失败')
   ```