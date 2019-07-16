# 需求文档 v0.0.1

## 数字身份钱包
**用户第一次打开手机App，显示以下三个按钮**
### 生成数字身份
用户点击“生成数字身份”按钮，App生成一对新的私钥和地址，将地址注册到ERC1484获得EIN
### 找回数字身份

### 创建数字身份
用户点击“创建数字身份”按钮，输入提前创建好或已拥有的私钥，App将私钥对应的地址注册到ERC1484获得EIN

**以上功能需要保存私钥，参考下个功能**
### 保存备份私钥
App将新生成的或者用户新导入的私钥加密保存，并提示用户通过冷钱包形式备份私钥。

**注册数字身份之后进入以下功能**
### 填写用户profile
用户上传以下信息到IDHub服务器和新注册的数字身份绑定

* 头像
	* 后续可以通过点击头像之后的设置按钮来进行修改
* 昵称
	* 后续可在profile页面点击修改
* 个性签名
	* 后续可在profile页面点击修改

**进入App主页面展示以下功能**
### 用户主页[我]
#### profile
* 头像，点击可放大并提供设置按钮可修改
* 昵称，点击可修改
* 个性签名，点击可修改

#### IDHub会员
* 查询链上claim验证用户是否为IDHub会员
	* 若是点亮会员勋章
	* 若不是会员勋章设为灰色（点灭）
* 点击会员勋章进入投资者身份信息页面
	* 若还未成为会员，则
		* 提醒用户申请会员及显示会员相关权益
		* 显示IDHub会员协议并检测用户阅读之后点选同意按钮才进入下一步
		* 填入身份信息，[参考](https://github.com/idhubnetwork/identity-resources-and-research/blob/master/MagicCircle/%E4%BA%A7%E5%93%81%E6%96%B9%E6%A1%88_did%E7%9B%B8%E5%85%B3.md#9idhub%E4%BC%9A%E5%91%98%E7%94%B3%E8%AF%B7) 
			1. 填入基本身份信息，包括
				* 姓名，`姓/Last Name      名/First Name`，必填
				* 生日，`生日/Date of Birth        （月）/      （日）/     （年）`，必填
				* 国籍，`国籍/ Nationality`，必填
				* 居住国，`居住国/ Country of Residence`，必填
				* 身份证件号码，`身份证件号码/ID Number`
					* 美国可选，若填写需上传身份证件照片（图片或PDF）
					* 中国必填，需上传身份证件照片（图片或PDF）
				* 护照号码，`护照号码/Passport Number`，可选
					* 若填写需上传护照照片（图片或PDF）
			2. 根据国籍和居住地填写基本认证信息，说明如下
				* 若国籍或居住国任一为美国，则填写
					* 地址，`地址/Address`，需上传住址证明照片`Proof of Address`（图片或PDF）
					* 邮箱，`邮箱/eMail`
					* 手机号，`手机号/Phone Number`
					* 纳税号，`纳税号/TaxID`
					* 社保号，`社保号 SSN` 
				* 国籍和居住国都不是美国，则**暂定**填写
					* 地址，`地址/Address`
					* 邮箱，`邮箱/eMail`
					* 手机号，`手机号/Phone Number`
			3. 进行已填信息确认`information confirm`
			4. 进行双因子认证确保邮箱和手机号信息真实可控，以便后续进行账户信息权限管理
				* 短信验证码 `SMS Code`
				* 邮箱认证 `Email confirmation`
			5. 合格投资人所需的认证信息填写，[参考](https://github.com/idhubnetwork/identity-resources-and-research/blob/master/MagicCircle/%E4%BA%A7%E5%93%81%E6%96%B9%E6%A1%88_did%E7%9B%B8%E5%85%B3.md#10%E5%90%88%E6%A0%BC%E6%8A%95%E8%B5%84%E4%BA%BA%E8%AE%A4%E8%AF%81)
			6. 合格购买人所需的认证信息填写，[参考](https://github.com/idhubnetwork/identity-resources-and-research/blob/master/MagicCircle/%E4%BA%A7%E5%93%81%E6%96%B9%E6%A1%88_did%E7%9B%B8%E5%85%B3.md#11%E5%90%88%E6%A0%BC%E6%8A%95%E8%B5%84%E4%BA%BA%E8%AE%A4%E8%AF%81)
			7. 通知用户等待信息审核，提交以下信息至IDHub数据库并严密保存
				* 以上所有已填信息
				* 用户身份标识符密钥对的私钥签名过的会员申请协议，采用EIP191签名方案
	* 查看已提交的信息
	* 编辑更新信息，参考身份信息填入步骤

### 数字资产信息[钱包]
#### 钱包信息
* 地址
* 钱包名称
* 导出私钥（明文、KeyStore文件、助记词）
* 更换钱包按钮
* 二维码收款地址
#### 资产信息
* ETH
* Token
	* ST
	* ERC20

#### 搜索框
* 用户根据地址、名称搜索资产
* 搜到资产后可添加到资产信息中

### Dapp浏览[浏览]
#### Dapp市场
* IDHub支持的Dapp
* ST平台Dapp

#### 搜索栏和地址栏
* 可输入Dapp名称搜索对应的dapp
* 可输入地址通过webview注入web3的方式访问对应的dapp

### 历史记录[历史]
#### 消息通知
* 系统通知
* 版本更新

#### 操作记录
* 资产转移记录
* 身份信息授权
* 第三方登录

## IDHub网站

## ST平台Demo