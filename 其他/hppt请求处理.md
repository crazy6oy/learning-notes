url: https://google.com/search?filename=cat

其中

protcol（协议）：https://

hostname: google.com

path（是指要访问的内容的在hostname下的位置）: /search

query-string（传入的一些参数）: filename=cat



REST（Representational State Transfer）

四个常用方法（PUT、POST、GET、DELETE）

常用的方式：

PUT/PATCH: 修改

POST: 创建

GET: 读取

DELETE: 删除



Request中含: method、uri、headers、body

Response中含: status、headers、body

其中headers带了一些信息，请求和相应不尽相同，但肯定包含了解析数据的方式；body应该更多的就是传输的一些参数、数据、文件数据等一些handle会用到会用户需要看到的数据信息。



举个例子：

POST -> handle function -> 读写数据库，生成返回数据 -> 返回客户端 socket.write(status_code, headers, body:{}) -> 返回response()



举例：

编写一些handle method/function，输入为request和response；

那么这个handle就是要解析request中的传入数据，对这个数据处理后，把需要返回的东西写到或者说存在response里面；

然后把这个method加route；

最后开一个端口监听。别人就可以网络访问这个api接口了。