## ST平台合规身份服务

### 功能定义

用户通过IDHub移动端App登录ST平台并授权ST平台获得合规投资者的身份信息。

### 使用场景

用户在ST平台使用ST相关服务时，ST平台通过IDHub平台确认用户的合法身份以帮助用户合法获得ST相关服务。

### 功能流程图

[![流程图](https://github.com/idhubnetwork/identity-resources-and-research/raw/master/MagicCircle/img/st-user-service.png)](https://github.com/idhubnetwork/identity-resources-and-research/blob/master/MagicCircle/img/st-user-service.png)

### 功能流程

- 前置条件

  - 用户已有去中心化数字身份
  - ST平台已有去中心化数字身份

- 功能规则

  1. ST平台将自己的去中心化数字身份的标识符（`did:idhub:身份标识符密钥对的以太坊地址`）通过线下合作登记到IDHub平台的合作商系统，IDHub平台会赋予ST平台一个`[Your-Api-Key-Token]`访问权限令牌，ST平台至少需要登记以下信息

     - 去中心化数字身份的标识符
     - ST平台全名
     - ST平台网站主页URL
     - ST平台去中心化数字身份登录URL

  2. 用户访问ST平台网站点击【DID登录】按钮，打开去中心化数字身份登录页面

  3. ST平台在去中心化数字身份登录页面展示一个包含JSON消息的二维码，同时前端登录页面开始轮询请求ST平台服务器以监测登录状态，其中JSON消息说明如下

     - 必须包含`aud`字段且值恒为ST平台的去中心化数字身份的标识符
     - 必须包含`sub`字段且值恒为`did-st`
     - 必须包含`act`字段且值恒为`login-author`
     - 必须包含`url`且必须为ST登录页面URL，建议从前端页面`location.href`属性获得
     - 必须包含`rdt`且必须为ST平台接受登录和授权令牌的URL，后续
       - 应验证登录行为
       - 可能依靠授权令牌访问IDHub平台获取合规用户的身份数据
     - 示例

     ```
     {
     	"aud": "did:idhub:0xdid1d8f906c745b0a82f4d21e02bafd7df1a0e14",
     	"sub": "did-st",
     	"act": "login-author",
     	"url": "http://login.magiccircle.com/did/",
     	"rdt": "http://login.magiccircle.com/did/token/"
     }
     ```

  4. 用户通过IDHub移动端App扫描上一步展示的二维码，原样得到上一步所述的JSON消息，App会为用户进行以下提示

     - 请用户核对`url`和网页地址栏的值是否相同
     - 通过`aud`字段查询ST平台全名且请用户核实
     - 通过`act`字段使用户知晓正在登录且授权ST平台访问必要的个人身份数据

  5. IDHub移动端App打包待认证消息，待认证消息说明如下

     - JSON字符串数据格式且原样保留上一步二维码扫描信息里的字段和值
     - 必须添加`iss`字段且值恒为用户的去中心化数字身份标识符
     - 必须包含`exp`字段且值为当前时间增加10秒，数据格式必须为`Unix timestamp`
     - 示例

     ```
     {
     	"aud": "did:idhub:0xdid1d8f906c745b0a82f4d21e02bafd7df1a0e14",
     	"iss": "did:idhub:0x1ddid8f906c745b0a82f4d21e02bafd7df1a0e14",
     	"sub": "did-st",
     	"exp": "1560404678",
     	"act": "login-author",
     	"url": "http://login.magiccircle.com/did/",
     	"rdt": "http://login.magiccircle.com/did/token/"
     }
     ```

  6. 用户通过IDHub移动端App使用身份标识符密钥对的私钥签名待认证消息生成一个 [JSON Web Token](https://jwt.io/) 形式的令牌（Token），令牌说明如下

     - `HEADER`字段编码前值恒为

     ```
     {
     	"alg": "ES256k",
     	"typ": "JWT"
     }
     ```

     - `PAYLOAD`字段的值必须原样保留前三步生成的待认证信息里的字段和值
     - `SIGNATURE`字段的值采用以太坊非对称加密算法（此文档内暂定命名为`ES256k`）生成，算法参考[EIP191](https://github.com/ethereum/EIPs/issues/191)
     - JSON Web Token 的组装方式请参考[此处](https://jwt.io/introduction/)

  7. 用户使用IDHub移动端App发起HTTP POST请求将JWT令牌发送至ST平台，说明如下

     - POST请求的参数为JSON格式
     - HTTP请求发至`rdt`字段值所示的链接
     - HTTP请求的参数为`{"jwt":"[JSON Web Token]"}`，其中`[JSON Web Token]`即为上一步组装好的JWT令牌

  8. ST平台收到令牌之后，通过`iss`字段的值查询平台自己的用户系统

     - 如果用户系统中存在与此去中心化数字身份相绑定的用户档案，则进入第14步
     - 如果用户系统中尚未存在与此去中心化数字身份相绑定的用户档案，则进入下一步

  9. ST平台使用自己的去中心化身份标识符密钥对的私钥对上一步收到的令牌签名，生成一个`[SIGNATURE]`消息，签名方式参考[EIP191](https://github.com/ethereum/EIPs/issues/191)

  10. ST平台将令牌和上一步的`[SIGNATURE]`消息通过HTTP请求发送至IDHub平台服务端以获取用户合规身份信息，说明如下

      - API:`http://service.idhub.network/st/identity?apikey=[Your-Api-Key-Token]`
      - `[Your-Api-Key-Token]`为第1步提前申请的访问权限令牌
      - HTTP请求的方法为POST
      - HTTP请求的参数为`{"jwt":"[JSON Web Token]", "sig":"[SIGNATURE]"}`，其中`[JSON Web Token]`即为第8步收到的JWT令牌，`[SIGNATURE]`为上一步生成的`[SIGNATURE]`消息

  11. IDHub平台服务端收到请求之后，先查询数据库验证`[JSON Web Token]`中的`iss`是否为合规投资者，若不是则返回`HTTP 503`错误代表用户不是合规投资者拒绝服务并且流程终止，若是合规投资者则继续验证`[JSON Web Token]`和`[SIGNATURE]`的有效性，验证规则如下

      - `[JSON Web Token]`中的`exp`字段值必须大于IDHub平台服务端当前时间戳
      - `[JSON Web Token]`中的任何字段的值必须符合上述步骤中所有字段描述的要求
      - 对`[JSON Web Token]`使用以太坊非对称加密算法的`ecrecover`计算出的地址必须和`iss`字段的值中的以太坊地址相同
      - 对`[SIGNATURE]`使用以太坊非对称加密算法的`ecrecover`计算出的地址必须和`[JSON Web Token]`中`aud`字段的值中的以太坊地址相同

  12. IDHub平台服务端根据验证结果作出以下响应

      - 若验证通过，查询数据库并打包用户身份信息作为响应返回给ST平台并且进入第13步，JSON格式的响应说明如下

      ```
      {
      	"LastName": "姓",
      	"FirstName": "名",
      	"Birth": "生日",
      	"Nationality": "国籍",
      	"Nationality": "居住国",
      	"eMail": "电子邮箱",
      	"PhoneNumber": "手机号",
      	"TaxID": "纳税号",
      	"SSN": "社保号",
      	"ID": {
      		"Number": "身份证件号码",
      		"Certification": "证明文件链接"
      	},
      	"Passport": {
      		"Number": "护照证件号码",
      		"Certification": "证明文件链接"
      	},
      	"Address": {
      		"Address": "住址",
      		"Certification": "证明文件链接"
      	},
      	"QualifiedInvestor": {
      		"Type": "合格投资人类型",
      		"Description": "类型描述",
      		"Certification": "证明文件链接"
      	},
      	"QualifiedPurchaser": {
      		"Type": "合格购买人类型",
      		"Description": "类型描述",
      		"Certification": "证明文件链接"
      	}
      }
      ```

      - 若验证未通过，则返回`HTTP 401`错误代表未经用户授权而获取信息，因此不予服务并且流程终止

  13. ST平台收到用户合规身份信息之后，在自己的用户系统中建立用户档案并和去中心化数字身份标识符进行绑定，然后直接进入第15步

  14. ST平台验证第8步收到的`[JSON Web Token]`令牌，验证通过进入下一步否则应当拒绝为用户服务，验证规则如下

      - `[JSON Web Token]`中的`exp`字段值必须大于ST平台服务端当前时间戳
      - `[JSON Web Token]`中的任何字段的值必须符合上述步骤中所有字段描述的要求
      - 对`[JSON Web Token]`使用以太坊非对称加密算法的`ecrecover`计算出的地址必须和`iss`字段的值中的以太坊地址相同

  15. ST平台设置用户登录状态为成功，前端页面在第3步发起的轮询监测到成功登录，然后使用户进入ST平台的用户中心或为用户提供后续ST相关服务

- 后置流程

  - ST平台用户中心
  - ST相关服务