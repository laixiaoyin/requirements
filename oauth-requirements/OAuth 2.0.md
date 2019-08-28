# OAuth 2.0实现授权

**OAuth 是一种授权机制，其核心就是向第三方应用颁发令牌。数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。系统从而产生一个短期的进入令牌（token），用来代替密码，供第三方应用使用。**

OAuth 引入了一个授权层，用来分离两种不同的角色：客户端和资源所有者。......资源所有者同意以后，资源服务器可以向客户端颁发令牌。客户端通过令牌，去请求数据。

## 一、令牌的四种授权类型

不管哪一种授权方式，第三方应用申请令牌之前，都必须先到系统备案，说明自己的身份，然后会拿到两个身份识别码：客户端 ID（client ID）和客户端密钥（client secret）。这是为了防止令牌被滥用，没有备案过的第三方应用拿不到令牌。

### 1、授权码

**授权码（authorization code）方式，指的是第三方应用先申请一个授权码，然后再用该码获取令牌**

这种方式较为常用，安全性高，适用于那些**有后端**的web应用。授权码通过前端传送，而令牌存储在后端，且所有与资源服务器的通信都在后端完成。前后端分离，可以避免令牌泄露。（共四步）



第一步，A 网站提供一个链接，用户点击后就会跳转到 B 网站，授权用户数据给 A 网站使用。

```
https://b.com/oauth/authorize?
  response_type=code&
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&
  scope=read
```

response_type 表示参数要求返回授权码

client_id 让B知道谁在请求

redirect_uri B 接受或拒绝请求后的跳转网址

scope 表示要求的授权范围（这里是只读）



第二步，用户跳转后，B 网站会要求用户登录，然后询问是否同意给予 A 网站授权。用户表示同意，这时 B 网站就会跳回`redirect_uri`参数指定的网址。跳转时，会传回一个授权码。

```
https://a.com/callback?code=AUTHORIZATION_CODE
```

code 即是B返回的授权码



第三步，A 网站拿到授权码以后，就可以在后端，向 B 网站请求令牌。

```
https://b.com/oauth/token?
 client_id=CLIENT_ID&
 client_secret=CLIENT_SECRET&
 grant_type=authorization_code&
 code=AUTHORIZATION_CODE&
 redirect_uri=CALLBACK_URL
```

`client_id`参数和`client_secret`参数用来让 B 确认 A 的身份

（`client_secret`参数是保密的，因此只能在后端发请求）；

`grant_type`参数的值是`AUTHORIZATION_CODE`，

表示采用的授权方式是授权码；

`code`参数是上一步拿到的授权码；

`redirect_uri`参数是令牌颁发后的回调网址。



第四步，B 网站收到请求以后，就会颁发令牌。具体做法是向`redirect_uri`指定的网址，发送一段 JSON 数据。

```
{    
  "access_token":"ACCESS_TOKEN",
  "token_type":"bearer",
  "expires_in":2592000,
  "refresh_token":"REFRESH_TOKEN",
  "scope":"read",
  "uid":100101,
  "info":{...}
}
```

`access_token`字段就是令牌，A 网站在后端拿到了。

### 2、隐藏式

适用于**纯前端**，没有后端的应用。这种方式没有授权码，称为（授权码）隐藏式。这种方式把令牌直接传给前端，是很**不安全**的。因此，只能用于一些安全要求不高的场景，并且令牌的有效期必须非常短，通常就是会话期间（session）有效，浏览器关掉，令牌就失效了。

第一步，A 网站提供一个链接，要求用户跳转到 B 网站，授权用户数据给 A 网站使用。

```
https://b.com/oauth/authorize?
  response_type=token&
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&
  scope=read
```

`response_type`参数为`token`，表示要求直接返回令牌



第二步，用户跳转到 B 网站，登录后同意给予 A 网站授权。这时，B 网站就会跳回`redirect_uri`参数指定的跳转网址，并且把令牌作为 URL 参数，传给 A 网站。

```
https://a.com/callback#token=ACCESS_TOKEN
```

`token`参数就是令牌，A 网站因此直接在前端拿到令牌



注意，令牌的位置是 URL 锚点（fragment），而不是查询字符串（querystring），这是因为 OAuth 2.0 允许跳转网址是 HTTP 协议，因此存在"中间人攻击"的风险，而浏览器跳转时，锚点不会发到服务器，就减少了泄漏令牌的风险。

### 3、密码式

如果你高度信任某个应用，用户也可以把用户名和密码直接告诉该应用。应用可以使用用户的密码申请令牌。风险较大，适合其他授权方式都无法采用的情况，而且必须是用户高度信任的应用。

第一步，A 网站要求用户提供 B 网站的用户名和密码。拿到以后，A 就直接向 B 请求令牌。

```
https://oauth.b.com/token?
  grant_type=password&
  username=USERNAME&
  password=PASSWORD&
  client_id=CLIENT_ID
```

grant_type=password 表示授权方式为密码式

username和password是B用户的用户名和密码

第二步，B 网站验证身份通过后，直接给出令牌。注意，这时不需要跳转，而是把令牌放在 JSON 数据里面，作为 HTTP 回应，A 因此拿到令牌。



### 4、凭证式

**适用于没有前端的命令行应用，即在命令行下请求令牌。**

A应用在命令行向B应用发出请求

```
https://oauth.b.com/token?
  grant_type=client_credentials&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET
```

grant_type=client_credentials  表示采用凭证式；

client_id=CLIENT_ID和 client_secret=CLIENT_SECRET 

用于让B确认A的身份

## 二、令牌的使用

A网站拿到令牌后，就可以向B网站的API请求数据了。

此时发送的API请求，都需要带有令牌。

可在请求的头信息中加Authorization字段，令牌就存在这个字段当中。

```
curl -H "Authorization: Bearer ACCESS_TOKEN" \
"https://api.b.com"
```

ACCESS_TOKEN，即拿到的令牌。

## 三、令牌的更新

具体方法是，B 网站颁发令牌的时候，一次性颁发两个令牌，一个用于获取数据，另一个用于获取新的令牌（refresh token 字段）。令牌到期前，用户使用 refresh token 发一个请求，去更新令牌。

```
https://b.com/oauth/token?
  grant_type=refresh_token&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET&
  refresh_token=REFRESH_TOKEN
```

`grant_type`参数为`refresh_token`表示要求更新令牌，

`client_id`参数和`client_secret`参数用于确认身份，

`refresh_token`参数就是用于更新令牌的令牌。

B 网站验证通过以后，就会颁发新的令牌。