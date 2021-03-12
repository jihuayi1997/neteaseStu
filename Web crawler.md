# 网络爬虫与信息提取

## Requests库

1. 主要方法

   `requests.request(method, url, **kwargs)`：构造一个请求，支撑下面方法

   `requests.get(url, params=None, **kwargs)`：获取HTML网页主要方法，对应HTTP的GET

   `requests.head(url, **kwargs)`：获取HTML网页头信息的方法，节约带宽，对应于HTTP的HEAD

   `requests.post(url, data=None, json=None, **kwargs)`：向HTML网页提交POST请求的方法，url后附加新数据，对应HTTP的POST

   ![image-20191209104152946](TyporaPics/image-20191209104152946.png)

   ![image-20191209104242634](TyporaPics/image-20191209104242634.png)

   `requests.put(url, data=None, **kwargs)`：向HTML网页提交PUT请求的方法，覆盖原url位置的资源，对应HTTP的PUT

   `requests.patch(url, data=None, **kwargs)`：向HTML网页提交局部修改请求，patch()只需提交修改字段，节约带宽，对应HTTP的PATCH

   `requests.delete(url, **kwargs)`：向HTML页面提交删除请求，对应HTTP的DELETE

2. Response对象的属性

   `r.status_code`：HTTP请求返回状态，200表示连接成功，404表示失败

   `r.text`：HTTP相应内容的字符串形式，即url对应的页面内容

   `r.encoding`：从HTTP header中猜测的响应内容编码方式，若header中不存在charset，则认为编码为"ISO-8859-1"

   `r.apparent_encoding`：从内容中分析的响应内容编码方式

   `r.content`：HTTP响应内容的二进制形式

3. 通用代码框架

   ```python
   import requests
   
   
   def getHTMLText(url):
       try:
           r = requests.get(url, timeout=30)
           r.raise_for_status()  # 如果状态不是200，引发HTTPError异常
           r.encoding = r.apparent_encoding
           return r.text
       except:
           return "exception occur"
   
   
   if __name__ == '__main__':
       url = "http://www.baidu.com"
       print(getHTMLText(url))
   ```

## 盗亦有道

