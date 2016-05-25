API接口规范
----

## 协议   
API与用户的通信协议，使用HTTP协议。

## 版本   
应该将API的版本号放入URL：

        https://neusoft.com/api/v1/

## 路径   
每一个网址代表一种资源(resource)，所以网址中不能有动词，只能有名词。API中的名词对应集合的情况，需要使用复数。

## HTTP动词：

        对于资源的具体操作类型，由HTTP动词表示。
        get请求中url中全部小写,包括?后面的参数如果需要分词_
        在返回结果中的属性名称全部用驼峰,便于类解析.

#### 常用方式
常用的HTTP动词有下面五个（括号里对应的SQL命令）：

        GET（SELECT）：从服务器取出资源（一项或多项）
        POST（CREATE）：在服务器新建一个资源
        PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）
        PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）
        DELETE（DELETE）：从服务器删除资源

**注：PATCH模式使用不明白者，可使用PUT替代，但应尽可能的按要求使用。**

例子：

        GET /zoos：列出所有动物园   
        POST /zoos：新建一个动物园   
        GET /zoos/ID：获取某个指定动物园的信息   
        PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）   
        PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）   
        DELETE /zoos/ID：删除某个动物园   
        GET /zoos/ID/animals：列出某个指定动物园的所有动物   
        DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物

#### 特殊方式
默认情况下，都是通过资源的ID来对资源信息进行索引，当使用其他信息进行单一资源索引时，需在URL中使用key=value的方式，例：

        GET /zoos/name=peter 查询名字为Peter的一个动物的信息
        GET /zoos/name=peter/color=gray 查询名字为Peter且颜色为灰色的一个动物的信息
        使用此种方式，返回的为单个对象。

        在资源集合中采用后最标明获取相应的返回结果
        http://localhost:8501/api/v1/vehicles/vno={vno} 这个资源获取到的肯定是一个车辆的全部信息.
        {
                code:1
                payload:{
                        model:{..},
                        location:{..}...
                }

        }

        在这里我可能用不上这么多信息,可能只需要用几个属性,比如位置信息,比如模型信息,又比如其他.具体用例如下
        http://localhost:8501/api/v1/vehicles/vno={vno}/location
        {
                code:1
                payload:{..}
        }

        http://localhost:8501/api/v1/vehicles/vno={vno}/model
        {
                code:1
                payload:{..}
        }
        也就是说我们可以在精确查询url的结尾设置一个名称来做出精确的过滤,建议不超过3层.


## 过滤信息   
如果记录数据很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。   
下面是一些常见的参数，示例：

        _____________________________________
        |       分页相关的2个分页参数
        |
        | page:当前页面页码
        | count:当前页面需要显示多少条
        |____________________________________


参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

## 状态码   
服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）：

        200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
        201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
        202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
        204 NO CONTENT - [DELETE]：用户删除数据成功。
        400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
        401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
        403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
        404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
        406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
        410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
        422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
        500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。

## 返回结果   
针对不同操作，服务器向用户返回的结果应该符合以下规范：

        GET /collection：返回资源对象的列表（数组）
        GET /collection/resource：返回单个资源对象
        GET /collection/key=value: 返回单个资源对象
        POST /collection：返回新生成的资源对象
        PUT /collection/resource：返回完整的资源对象
        PATCH /collection/resource：返回完整的资源对象
        DELETE /collection/resource：返回一个空文档

内容参考：
* http://www.ruanyifeng.com/blog/2014/05/restful_api.html
