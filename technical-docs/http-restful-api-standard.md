# Http Restful API Standard

采用面向资源的api。

## 1. 协议

API与用户的通信协议，总是使用HTTPs协议。

## 2. 域名

将API部署在专用域名之下。

    https://api.shudong-test.cn

## 3. 版本

将API的版本号放入URL。

    https://api.shudong-test.com/v1/

（另一种做法是，将版本号放在HTTP头信息中，但不如放入URL方便和直观。Github采用这种做法。）

## 4. 路径（Endpoint）

- 网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。
- API中的名词使用复数。

```text
https://api.shudong-test.cn/v1/users
https://api.shudong-test.cn/v1/posts
https://api.shudong-test.cn/v1/posts/ID/comments
```

## 5. HTTP动词

常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

```text
GET（SELECT）：从服务器取出资源（一项或多项）。
POST（CREATE）：在服务器新建一个资源。
PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
DELETE（DELETE）：从服务器删除资源。
```

给出一些例子：

```text
GET /posts 获取所有posts
GET /posts/ID 获取指定post
POST /posts 新建一个post
PUT /users/ID 更新某个用户的信息（请求中提供完整信息）
PATCH /users/ID 更新某个用户的信息（请求中提供需要更改的信息）
DELETE /posts/ID 删除某个post
GET /posts/ID/comments 列出指定post的所有评论
DELETE /posts/ID/comments 删除指定posts下的所有评论
```

## 6. 过滤信息（Filtering）

API应该提供参数，过滤返回结果。下面是一些常见的参数。

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，`GET /posts/ID/comments` 与 `GET /comments?post_id=ID` 的含义是相同的。

```text
?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件
```

## 7. 状态码(Status Code)

服务器向用户返回的状态码和提示信息。下面列出常用到的。

```text
200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
204 NO CONTENT - [DELETE]：用户删除数据成功。
400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
```

## 8. 错误处理（Error handling）

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

```text
{
    error: "Invalid API key"
}
```

## 9. 返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

```text
GET /collection：返回资源对象的列表（数组）
GET /collection/resource：返回单个资源对象
POST /collection：返回新生成的资源对象
PUT /collection/resource：返回完整的资源对象
PATCH /collection/resource：返回完整的资源对象
DELETE /collection/resource：返回一个空文档(状态码：204)
```

## 10. Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。

```json
{"link": {
    "rel":   "collection https://www.shudong.com/posts",
    "href":  "https://api.posts.com/posts",
    "title": "List of posts",
    "type":  "application/vnd.yourformat+json"
}}
```

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。

Hypermedia API的设计被称为HATEOAS。Github的API就是这种设计，访问api.github.com会得到一个所有可用API的网址列表。

```json
{
    "current_user_url": "https://api.github.com/user",
    "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
```

从上面可以看到，如果想获取当前用户的信息，应该去访问`api.github.com/user`，然后就得到了下面结果。

```json
{
    "message": "Requires authentication",
    "documentation_url": "https://developer.github.com/v3"
}
```

上面代码表示，服务器给出了提示信息，以及文档的网址。

## 11. 其他

- API的身份认证应该使用OAuth 2.0框架。
  - 因为非登录用户不允许进入和查看树洞内的帖子，所以对所有资源的GET也都要使用token
  - token应定期更新
  - 每个用户同时拥有2个不同的token，分别用于GET操作和非GET操作。
- 服务器返回的数据格式，应该尽量使用JSON，避免使用XML。
