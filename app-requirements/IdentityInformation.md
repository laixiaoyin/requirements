# Identity Information

## 概述

* 用户自主选择提供的信息，App提供可能提交的所有信息的交互界面。
* 用户在申请Claim时，App根据本地记录检查信息完备性。
* 用户申请Claim后，IDHub根据用户提供的信息审核是否合规和发放Claim。

## 可能提交的信息

可能提交的信息意味着所有信息参数都是可选的，完全由用户意愿自主选择，但是Claim的结果取决于用户提交的信息是否满足Claim所要求的信息审核策略。

### 文本身份信息
App内以 “编辑身份信息” 标示此行为。

* 姓名，`姓/Last Name 名/First Name`
* 生日，`生日/Date of Birth （月）/ （日）/ （年）`
* 国籍，`国籍/ Nationality`
* 居住国，`居住国/ Country of Residence`
* 身份证件号码，`身份证件号码/Government Issued Identification Number`
* 护照号码，`护照号码/Passport Number`
* 地址，`地址/Address`
* 邮箱，`邮箱/eMail`
* 手机号，`手机号/Phone Number`
* 纳税号，`纳税号/TaxID`
* 社保号，`社保号/SSN`

### 文件身份信息
App内以 “编辑证明文件” 标示此行为。
#### 可供选择的文件类型 `Type`
* 身份证件正面照片
* 身份证件反面照片
* 护照照片
* 银行余额证明文件
* 银行流水文件
* 评估机构的评估文件
* CFA净资产证明文件
* 税表文件
* 征信报告
* 住址证明文件/`Proof of Address`
* 非自住房的房产证明

#### 文件名称 `Name`
用户可以自定义输入证明的名称用于标示。

## 可供申请的Claim

### 分类

#### IDHub会员 `IDHub VIP`
提交以下信息则称为IDHub会员：

* 姓名，`姓/Last Name 名/First Name`，必填
* 生日，`生日/Date of Birth （月）/ （日）/ （年）`，必填
* 国籍，`国籍/ Nationality`，必填
* 居住国，`居住国/ Country of Residence`，必填
* 身份证件号码，`身份证件号码/Government Issued Identification Number`，必填
* 护照号码，`护照号码/Passport Number`
* 地址，`地址/Address`
* 邮箱，`邮箱/eMail`，必填
* 手机号，`手机号/Phone Number`，必填
* 纳税号，`纳税号/TaxID`
* 社保号，`社保号/SSN`
* 身份证件正面照片，必填
* 身份证件反面照片，必填
* 护照照片
* 住址证明文件/`Proof of Address`

#### IDHub超级会员 `IDHub VVIP`

以上一步信息的提交内容通过KYC Provider审核的用户可以申请成为IDHub超级会员，申请页面应该提醒用户可选信息填写的越全越容易通过KYC

#### 合格投资人 
满足下列条件之一即可，用户可以自由选择审核类型：

1. 连续三年年收入超过20万美金，或与配偶一起超过30万 Any individual who had an income in excess of US$200,000 in each of the two most recent years or joint income with that person’s spouse in excess of US$300,000 in each of those years and reasonably expects to reach the same income level in the current year.(上传图片或PDF格式的银行流水）
2. 净资产超过1百万美金（除自住房之外）Any individual whose net worth, or joint net worth with that person’s spouse, at the time of his or her purchase of an Interest, exceeds US$1,000,000. As used herein, “net worth” means the excess of total assets at fair market value, including homes (but excluding the value of the undersigned’s primary residence), home furnishings and automobiles, over total liabilities.（上传图片或PDF格式的资产证明/负债证明，如果国籍或居住国任一为美国需要上传税表）

#### 合格购买人
承诺满足以下条件之一即可，用户可以自由选择审核类型：

1. A natural person (including any person who holds a joint, community property, or other similar shared ownership interest in an issuer that is excepted under Section (c)(7) of the 1940 Act with that person’s qualified purchaser spouse) who owns not less than $5,000,000 in investments as defined by the Securities and Exchange Commission.(上传图片或PDF格式的银行流水）
2. A trust that is not covered by clause the family entity definition and that was not formed for the specific purpose of acquiring the Partnership Interest offered, as to which the trustee or other person authorized to make decisions with respect to the trust, and each settlor of the other person who has contributed assets to the trust, a natural person, family entity.
3. A natural person, a corporation, partnership, association, joint stock company, trust, fund or other organized group of persons, whether incorporated or not, acting for his/her/its own account or the account of other qualified purchasers, (i) who in the aggregate owns and invests on a discretionary basis, not less than $25,000,000 in investments and (ii) An entity in which each of the beneficial owners of the entity’s securities is a qualified purchaser, as that term is defined in the 1940 Act.
4. A “knowledgeable employee,” as defined for purposes of Section 3(c)(5) under the 1940 Act.

#### ST合规投资者

同时成为合格投资人和合格购买人即可申请成为ST合规投资者

### 审核条件

#### IDHub会员
提交信息
#### IDHub超级会员
通过KYC Provider的KYC审核
#### 合格投资人
需要首先成为IDHub超级会员

1. 条件一需要以下证明文件的完备且审核真实满足条件：
	* 银行流水文件
2. 条件二需要证明文件真实满足条件且满足如下完备性：
	* CFA净资产证明文件 || 银行余额证明文件 && (税表文件 || 征信报告）
	* 非自住房的房产证明、评估机构的评估文件可作为辅助的审核文件
	
#### 合格购买人
需要首先成为IDHub超级会员，仅要求承诺满足条件之一即可，但若选择承诺条件一，需要审核以下证明文件：

* 银行流水文件

#### ST合规投资者
同时成为合格投资人和合格购买人即可

## 身份信息本地记录
移动端身份App应记录用户身份信息的提交记录，包括以下字段：

* 类型，`Type`，必须
* 名称，`Name`，可选
* 提交时间，`timestamp`，必须
* DID标识符，·`did:ethr:...`，必须

App通过以上所描述的信息在用户提交Claim申请之前自主检查信息完备性。





















 