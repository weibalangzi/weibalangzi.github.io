---
title: 使用DES加密信息
tag:
- 加密
- DES
---

> DES是一种对称加密算法，所谓对称加密算法即：加密和解密使用相同密钥的算法;
DES加密算法出自IBM的研究，后来被美国政府正式采用，之后开始广泛流传;
DES算法在POS、ATM、磁卡及智能卡（IC卡）、加油站、高速公路收费站等领域被广泛应用;

`注意`：`DES加密和解密过程中，密钥长度都必须是8(8个字节)的倍数`
<p style="color:white;background:#0f88eb;padding:10px;border-radius:3px;font-weight:900">常见算法分类</p>
**线性散列算法（签名算法）**

例如MD5、SHA1、HMAC

<!--more-->

<span style="background:#0f88eb;border-radius:3px;color:white;">MD5</span>

即Message-Digest Algorithm 5(信息摘要-算法5)，用于确保信息传输完整一致；

`特点`：

 - 压缩性：任意长度的数据看，算出的MD5值长度都是固定的；
 - 容易计算：从原数据计算出MD5值很容易；
 - 抗修改性：对原数据进行任何改动哪怕一个字节，所得到的MD5值都有很大区别；
 - 强抗碰撞：已知原数据和其MD5值，想找到一个具有相同MD5值的数据（即伪造数据）是非常困难的；

MD5的作用是让大容量信息在用数字签名软件签署私人密钥前被“压缩”成一种保密格式（即把任意一个长度的字节串变换成一定长的十六进制数字串）

**对称性加密算法**

例如：AES、DES、3DES

<span style="background:#0f88eb;border-radius:3px;color:white">AES</span>

Advanced Encryption Standard（AES）：在密码学中又称Rijndael机密法，是美国采用的一种区块加密标准，这个标准用来替代原先的DES，已经被多方分析且广为全世界所使用；

**非对称性加密算法**

例如：RSA、DSA、ECC

RSA:RSA公开密钥密码体制，所谓的公开密钥密码体制就是使用不同的加密密钥与解密密钥，是一种由已知加密密钥推导出解密密钥在计算上是不可行的密码体制；

在公开密钥密码体制中，加密密钥（即公开密钥）PK是公开信息，而解密密钥（即秘密密钥）SK是需要保密的，加密算法E和解密算法D也是公开的，虽然解密密钥SK是有公开密钥PK决定的，但却不能根据PK计算出SK；

<p style="color:white;background:#0f88eb;padding:10px;border-radius:3px;font-weight:900">DES加密的基本原理</p>
`DES`:`Data Encryption Standard`

DES（数据加密标准）算法主要采用替换和移位的方式进行加密，它用56位（64位密钥只有56位有效）对64位二进制数据块进行加密，每次加密对64位的输入数据进行16轮编码，经过一系列替换和移位后，输入的64位元数据转换成完全不同的64位输出数据；
<p style="color:white;background:#0f88eb;padding:10px;border-radius:3px;font-weight:900">在react中使用DES加密</p>
**首先安装该模块**

使用`npm`安装`npm install cryto-js`，安装的过程中如果报错，或者安装后使用过程中报错，建议使用`yarn`进行安装，即`yarn add crypto-js`;

**导入DES模块**

google的cryto-js里面包含各种加密算法，只需要在项目中导入所需要的算法即可；

```javaScript
import {DES,mode,pad,enc} from 'crypto-js';
```
要使用DES，上面的mode,pad,enc也是必需的；

**创建一个密钥**
```javaScript
const key = 12345678
```
**html部分**
```html
<input type="text" id="desTest"/>
<button id="btn" onClick={this.toDes}>加密</button>
<button onClick={this.unDes}>解密</button>
```
**js(加密)部分**
```javaScript
  toDes=()=>{
    let iptmes = document.getElementById("desTest").value;//获取数据
    let thekey = enc.Utf8.parse(key);//处理密钥
    let theMes = DES.encrypt(iptmes,thekey,{
      mode: mode.ECB,
      padding: pad.Pkcs7
    });//加密处理
    this.setState({mes:theMes.toString()});//保存加密后得到数据
    console.log(theMes.toString());//打印加密后的数据
  }
```
**js（解密部分）**
```javaScript
  unDes=()=>{
    let iptmes = this.state.mes;//获取需要解密的数据
    let thekey = enc.Utf8.parse(key);//处理密钥
    let theMes = DES.decrypt(iptmes,thekey,{
      mode: mode.ECB,
      padding: pad.Pkcs7      
    });//解密处理
    console.log(theMes.toString(enc.Utf8));//打印解密后数据
  }
```
以上代码中，`encrypt`和`decrypt`都是DES内置的方法，`mode`指的是分组加密的模式，`padding`指的是最后一个分组的填充方法；

填充的原因是在通常情况下，明文并非刚好是64位的倍数，对于最后一个分组，如果长度小于64位，则需要用数据填充至64位；

虽然DES的有效密钥长度是56位，但是要求密钥长度是64位（8字节）；




