# 网络爬虫与信息提取

## Requests库

1. 主要方法

   构造一个请求，支撑下面方法：`requests.request(method,url,**kwargs)`

   获取HTML网页主要方法，对应HTTP的GET：`requests.get(url,params=None,**kwargs)`

   获取HTML网页头信息的方法，对应于HTTP的HEAD：`requests.head()`

   ​	**只获得头部，节约带宽**

   向HTML网页提交POST请求的方法，url后附加新数据，对应HTTP的POST：`requests.post()`

   ![image-20191209104152946](TyporaPics/image-20191209104152946.png)

   ![image-20191209104242634](TyporaPics/image-20191209104242634.png)

   向HTML网页提交PUT请求的方法，覆盖url位置资源，对应HTTP的PUT：`requests.put()`

   ​	**put()方法与post()类似，区别只是覆盖**

   向HTML网页提交局部修改请求，对应HTTP的PATCH：`requests.patch()`

   ​	**patch()只需提交修改字段，节约带宽**

   向HTML页面提交删除请求，对应HTTP的DELETE：`requests.delete()`

2. Response对象的属性

   HTTP请求返回状态：`r.status_code`

   HTTP相应内容的字符串形式：`r.text`

   从HTTP header中猜测的响应内容编码方式：`r.encoding`

   ​	**若header中不存在charset，则认为r.encoding为'ISO-8859-1'**

   从内容中分析的响应内容编码方式：`r.apparent_encoding`

   HTTP响应内容的二进制形式：`r.content`

3. 通用代码框架

   ```python
   import requests
   def getHTMLText(url)
   	try:
           r=requests.get(url,timeout=30)
           r.raise_for_status()	#如果状态不是200，引发HTTPError异常
           r.encoding=r.apparent_encoding
           return r.text
       except:
           return "exception occur"
   if _name_=="_main_"
   	url="http://www.baidu.com"
       print(getHTMLText(url))
   ```

4. 