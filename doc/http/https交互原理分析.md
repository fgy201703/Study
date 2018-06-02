### HTTP交互原理

uri/url   uri 统一资源描述符

指定协议类型  ftp/https
host
port 默认是80端口
patth 资源路径
query-string 查询字符串

 ‘#head

uri

POST  创建   GET  查询 DELETE 删除 PUT 更析   OPTION (get/post) HEAD()




Request URL: https://www.csdn.net/
Request Method: GET
Status Code: 200 OK
Remote Address: 47.95.164.112:443
Referrer Policy: unsafe-url

Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Sun, 27 May 2018 12:12:11 GMT
Keep-Alive: timeout=20
Server: openresty
Strict-Transport-Security: max-age= 31536000
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Powered-By: PHP/5.5.23


Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Connection: keep-alive
Cookie: uuid_tt_dd=10_20283617140-1527318675226-977100; dc_session_id=10_1527423122324.992739; TY_SESSION_ID=4c8f82c1-4cf5-4703-9dce-dd3ff32c011d; Hm_lvt_6bcd52f51e9b3dce32bec4a3997715ac=1527321381,1527321831,1527321953,1527423119; Hm_lpvt_6bcd52f51e9b3dce32bec4a3997715ac=1527423119; ADHOC_MEMBERSHIP_CLIENT_ID1.0=18fa96e1-a692-6ca7-1bf4-9d082430a808; dc_tos=p9dz7y
Host: www.csdn.net
Referer: https://www.baidu.com/link?url=YAmjxCiG8oMBbdOyqMqvma75WUu7ZkATxVnUE71pQ6C&wd=&eqid=8d5042910003bd1e000000065b0aa08f
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36



#### HTTP 协议是无状态

   引用cookie解决无状态问题 ，保存一些信息 由sessionId  如: uuid_tt_dd

  session 是服务端保存  cookie是客户端保存

![1527424275292](D:\study\github\github-track\doc\http\image\httpcookie.png)

#### https 协议

HTTP:   http >  TCP > IP     

HTTPS: http > ssl/tls >TCP > IP

https的通信原理:

客户端    ----【数据】----->   服务端    

1,通过对【数据】 对称加密   意味 密钥要统一 【客户端拿到统一的密钥】     非法者也可以拿到密钥   即密钥被破解

2,非对称加密    即存在公钥和私钥 

​    每个客户端 拿到公钥  

​    服务端拿到一个私钥

​    客户端公钥如何取到  

   第三方机构 发布一个数字证书  发给服务端      

​    客户端接收到非法者替换服务端发过来的数字证书

​     客户端如何验证数字证书的有效性？   第三方机构CA通过私钥对服务端的公钥进行加密 生成一个证书编号如通过md5进行加密 得到一个编号(签名 +签名的加密算法)

​    第三方机构对数字证书进行解密  通过公钥和 加密算法进行解密得到数字证书与之前提供的摘要进行对比是否一致如一致即是合法

流程：

1，先在服务端中生成 私钥和公钥，将公钥发送给第三方机构

2，第三方机构使用私钥对服务端发过来的公钥进行加密  生成一个数字证书CA

3，第三方机构将CA 发送到客户端内置到浏览器CA根证书 C.PUB

4，客户端发送请求    TCP三次握手发起请求   client.random  生成一个随机数

​      客户端支持的加密算法协议版本

5，服务端返回服务器的证书sever.random确认使用的加密算法

6，客户端验证证书 :

​      6.1，验证CA证书可受信任

​       6.2，证书的有效期

​       6.3，证书是否被吊销

​       6.4，数字签名的验证   

​       6.5，验证所有者

​       6.6，客户端再次发送 pre-master    secret+client.random+server.random 

​                利用前面协商好的加密算法，对当前传输的消息进行签名

​      6.7，服务端解密  pre-master secret+client.random+server.rando

​      6.8，客户端再次验签  确认消息是否被篡改 验签

​                session key (对称加密)

第一次拿到对称加密KEY，最后还是通过对称加密来进行通讯来完成每次的通讯

![1527428819006](D:\study\github\github-track\doc\http\image\CA流程1.png)

![1527429190764](D:\study\github\github-track\doc\http\image\CA流程2.png)

#### WEB攻击手段

被动  ：请求伪造、跨站脚本攻击 sql注入





  

 







