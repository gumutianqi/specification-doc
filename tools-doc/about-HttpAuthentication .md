## 什么是Authentication
    首先解释一下两个长得很像的单词,对于英语不好的人看起来基本上一样
    Authentication:认证 
    举个例子:比如你去告诉别人一些事情,如何让别人相信你说的事情是真的,让人相信你???
    你告诉别人你叫陈湘宁,让别人怎么样相信你就是陈湘宁
    Authorization:授权
    词意:把权力委托给人或机构，代为执行。
    当你得到认证之后想要获取到或者使用该角色的其他功能时就要需要授权

## HTTP Basic 
    HTTP Basic指的是最简单的认证协议.简单到什么程度呢?就是直接告诉服务器用户名和密码.
    这里假设用户名**cxn**密码**123**
    
    使用linux curl命令访问服务器
    ```sh
    curl -vu cxn:123 http://localhost:9191/api/oauth/token?username=admin&password=admin&grant_type=password
    ```

### request头部   
    GET /secret HTTP/1.1
    Authorization: Basic Y3huOjEyMw==
    ...
    我们这里看到发送的request头部中含有Authentication这个字段，其值为Basic Y3huOjEyMw==
    Basic表示的使用的是HTTP Basic Authentication。
    而Y3huOjEyMw== 则是由“**cxn**:**123**”进行Base64编码以后得到的结果。

### response头部： 
    
    HTTP/1.1 200 OK
    ...
    
    因为我们输入的是正确的用户名密码，所以服务器会返回200，表示验证成功。
    如果我们用错误的用户的密码来发送请求，则会得到类似如下含有401错误的response头部：
    
    HTTP/1.1 401 Bad credentials
    WWW-Authenticate: Basic realm="Spring Security Application"
    ...
    
    乍看起来好像HTTP Basic还挺不Y3huOjEyMw==错的，Y3huOjEyMw==已经让人很难看出来用户名密码是什么了。
    但是，我们需要知道Base64编码是可逆的。也就是我们可以通过decode Base64的编码还原用户名和密码。
    
    在命令行输入如下命令：
    ```sh
       echo Y3huOjEyMw== | base64 -D
    ```
    得到：
    ```
       cxn:123
    ```
    轻松解密。试想，如果一个人通过一定的方法截获了向服务器发送的请求，那不是很容易就能够得到她的用户名和密码了吗？
    所以，为了保证用户的安全，我们不会直接通过HTTP的方式使用Basic Authentication，而是会使用HTTPS，这样更安全一些。

### Replay Attack  重复攻击

    通过前面介绍，我们知道了通过可逆的Base64的编码方式不是太靠谱，那我们在发送密码之前，
    将密码用不可逆的方式进行编码不就完了吗?
    
    比如，前面**cxn**的密码是**123**，进行MD5编码
    ```
       md5 -s 123
    ```
    以后得到的就是
    
    xxxxxxxxxxxxx
    
    这样不就是不可逆的了？恩，即使有一个叫**kaka**的家伙截获了我向服务器发送的用户名密码，
    他也不知道我的密码到底是什么了。
    
    的确，**kaka**拿到xxxxxxxxxxxxx这个被md5 hash过的密码也不知道**cxn**的密码是什么。
    但是，如果**kaka**直接拿着这个字符串放在HTTP头部，再发送给服务器不就OK了？
    **kaka**这样就根本不用解密这个密码也能装成“**cxn**”向服务器通信。这就叫做Replay Attack。

### HTTP Digest
    为了避免被坏人使用Replay Attack，一个简单的想法就是，让我们每次向服务器发送的认证信息都“必须”是不一样的，
    这样Craig即使拿到了这个认证信息也没有办法进行replay attack了。
    那如何让**cxn**每次向服务器发送的认证信息都是不一样的，同时能够让服务器知道这就是**cxn**呢？
    
    这就引出了Digest Authentication了。当**cxn**初次访问服务器时，并不携带密码。
    此时服务器会告知Alice一个随机生成的字符串（nonce）。
    然后**cxn**再将这个字符串与她的密码**123**结合在一起进行MD5编码，将编码以后的结果发送给服务器作为验证信息。
    
    因为nonce是“每次”（并不一定是每次）随机生成的，所以**cxn**在不同的时间访问服务器，其编码使用的nonce值应该是不同的
    如果携带的是相同的nonce编码后的结果，服务器就认为其不合法，将拒绝其访问。
    这样，即使**kaka**能够截获**cxn**向服务器发送的请求，也没有办法使用Replay Attack冒充成**cxn**了。

```
具体相关介绍需要查询网络
Digest Authentication比Basic安全，但是并不是真正的什么都不怕了，
Digest Authentication这种容易方式容易收到Man in the Middle式攻击

```

*陈湘宁 2016-4-2 23:19:39*
![](http://wenwen.soso.com/p/20120414/20120414172749-1022687677.jpg)   