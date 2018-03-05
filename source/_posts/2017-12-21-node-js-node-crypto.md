---
title: Node Crypto
date: 2017-12-21 14:34:39
updated: 2137-12-21 14:34:39
categories: Node.js
---
>密码技术是互联网应用的一项最基本的技术之一，主要保证了数据的安全。安全定义是多维度的，通过不可逆的 hash 算法可以保证登陆密码的安全；通过非对称的加密算法，可以保证数据存储的安全性；通过数字签名，可以验证数据在传输过程中是否被篡改。
我们要做互联网应用，数据安全性一个是不容忽视的问题。

## 一、Crypto 介绍
[Crypto](http://nodejs.org/api/crypto.html) 库是随 Nodejs 内核一起打包发布的，主要提供了加密、解密、签名、验证等功能。Crypto 利用 OpenSSL 库来实现它的加密技术，它提供 OpenSSL 中的一系列哈希方法，包括 hmac、cipher、decipher、签名和验证等方法的封装。

## 二、Hash 算法
哈希算法，是指将任意长度的二进制值映射为较短的固定长度的二进制值，这个小的二进制值称为哈希值。哈希值是一段数据唯一且极其紧凑的数值表示形式。如果散列一段明文而且哪怕只更改该段落的一个字母，随后的哈希都将产生不同的值。要找到散列为同一个值的两个不同的输入，在计算上是不可能的，所以数据的哈希值可以检验数据的完整性。一般用于快速查找和加密算法。通常我们对登陆密码，都是使用 Hash算法进行加密，典型的哈希算法包括 `md5`,`sha`,`sha1`,`sha256`,`sha512`,`RSA-SHA`。
```js
const crypto = require('crypto');
const fs = require('fs');

// 打印支持的 hash 算法
let hashes = crypto.getHashes();
console.log(`${JSON.stringify(hashes)}`);
// ["DSA","DSA-SHA","DSA-SHA1","DSA-SHA1-old","RSA-MD4","RSA-MD5","RSA-MDC2","RSA-R
// IPEMD160","RSA-SHA","RSA-SHA1","RSA-SHA1-2","RSA-SHA224","RSA-SHA256","RSA-SHA38
// 4","RSA-SHA512","dsaEncryption","dsaWithSHA","dsaWithSHA1","dss1","ecdsa-with-SH
// A1","md4","md4WithRSAEncryption","md5","md5WithRSAEncryption","mdc2","mdc2WithRS
// A","ripemd","ripemd160","ripemd160WithRSA","rmd160","sha","sha1","sha1WithRSAEnc
// ryption","sha224","sha224WithRSAEncryption","sha256","sha256WithRSAEncryption","
// sha384","sha384WithRSAEncryption","sha512","sha512WithRSAEncryption","shaWithRSA
// Encryption","ssl2-md5","ssl3-md5","ssl3-sha1","whirlpool"]

// 遍历加密所有 hash 方法
function doHash(hashes) {
    hashes.forEach(item => hashAlgorithm(item));
}

// 根据不同的 hash 方法获得对应的 hash 值
function hashAlgorithm(algorithm) {
    console.time(algorithm);
    let str = 'Hello Hash';
    let hashHandle = crypto.createHash(algorithm);
    //digest: 'hex', 'latin1' , 'base64'
    let hashResult = hashHandle.update(str).digest('hex');
    console.timeEnd(algorithm);
    console.log(`\n${algorithm}: ${str} -> ${hashResult}`);
}
// 运行方法 hash 测试函数
doHash(hashes);
```

## 三、Hmac 算法
HMAC 是密钥相关的哈希运算消息认证码（Hash-based Message Authentication Code）,HMAC 运算利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出。HMAC 可以有效防止一些类似 md5 的彩虹表等攻击，比如一些常见的密码直接 MD5 存入数据库的，可能被反向破解。
定义 HMAC 需要一个加密用散列函数（表示为 H，可以是 MD5 或者 SHA-1）和一个密钥 K。我们用 B 来表示数据块的字节数。（以上所提到的散列函数的分割数据块字长B=64），用L来表示散列函数的输出数据字节数（MD5中L=16,SHA-1中L=20）。鉴别密钥的长度可以是小于等于数据块字长的任何正整数值。应用程序中使用的密钥长度若是比B大，则首先用使用散列函数H作用于它，然后用H输出的L长度字符串作为在HMAC中实际使用的密钥。一般情况下，推荐的最小密钥K长度是L个字节。
```js
const crypto = require('crypto');
const fs = require('fs');
let hashes = crypto.getHashes();

// 遍历加密所有 hash 方法
function doHmac(hashes, key) {
    hashes.forEach(item => hmacAlgorithm(item, key));
}

// 根据不同的 hash 方法获得对应的 hash 值
function hmacAlgorithm(algorithm, key) {
    console.time(algorithm);
    let str = 'Hello Hmac';
    let hmacHandle = crypto.createHmac(algorithm, key);
    //digest: 'hex', 'latin1' , 'base64'
    let hmacResult = hmacHandle.update(str).digest('hex');
    console.timeEnd(algorithm);
    console.log(`\n${algorithm}-${key}: ${str} -> ${hmacResult}`);
}

// 运行 hamac 测试函数
let key1 = 'kuku';
let key2 = 'luckykuku'
doHmac(hashes,key2);
```

## 四、Salt 算法
我们知道，如果直接对密码进行散列，那么黑客可以对通过获得这个密码散列值，然后通过查散列值字典（例如 MD5 密码破解网站），得到某用户的密码。
盐（Salt），在密码学中，是指通过在密码任意固定位置插入特定的字符串，让散列后的结果和使用原始密码的散列结果不相符，这种过程称之为“加盐”。加盐后的散列值，可以极大的降低由于用户数据被盗而带来的密码泄漏风险，即使通过彩虹表寻找到了散列后的数值所对应的原始内容，但是由于经过了加盐，插入的字符串扰乱了真正的密码，使得获得真实密码的概率大大降低。
```js
const crypto = require('crypto');
const fs = require('fs');

// 随机生成指定长度的随机字符串,返回的是 buffer 类型
// crypto.randomBytes(size[, callback])
let randomBuf = crypto.randomBytes(10);
console.log(`${randomBuf.length} bytes of random data: ${randomBuf.toString('hex')}`);

let password = '123456';
// iterations: 循环加密次数 digest: hmac 使用的 hash 算法
// crypto.pbkdf2Sync(password, salt, iterations, keylen, digest) 
let pbkdf2Result = crypto.pbkdf2Sync('123456',randomBuf,10,16,'sha1')
console.log(`${pbkdf2Result.length} bytes of random data: ${pbkdf2Result.toString('hex')}`);
```

## 五、加密和解密算法
对于登陆密码来说，是不需要考虑解密的，通常都会用不可逆的算法，像 md5,sha-1 等。但是，对于有安全性要求的数据来说，我们是需要加密存储，然后解密使用的，这时需要用到可逆的加密算法。对于这种基于 KEY 算法，可以分为对称加密和不对称加密。
- 对称加密算法的原理很容易理解，通信一方用 KEK 加密明文，另一方收到之后用同样的KEY来解密就可以得到明文。
- 不对称加密算法，使用两把完全不同但又是完全匹配的一对Key:公钥和私钥。在使用不对称加密算法加密文件时，只有使用匹配的一对公钥和私钥，才能完成对明文的加密和解密过程。

```js
const crypto = require('crypto');
const fs = require('fs');

let ciphers = crypto.getCiphers();
console.log(`${JSON.stringify(ciphers)} \n`);
// ["aes-128-cbc","aes-128-cbc-hmac-sha1","aes-128-cbc-hmac-sha256","aes-128-ccm","
// aes-128-cfb","aes-128-cfb1","aes-128-cfb8","aes-128-ctr","aes-128-ecb","aes-128-
// gcm","aes-128-ofb","aes-128-xts","aes-192-cbc","aes-192-ccm","aes-192-cfb","aes-
// 192-cfb1","aes-192-cfb8","aes-192-ctr","aes-192-ecb","aes-192-gcm","aes-192-ofb"
// ,"aes-256-cbc","aes-256-cbc-hmac-sha1","aes-256-cbc-hmac-sha256","aes-256-ccm","
// aes-256-cfb","aes-256-cfb1","aes-256-cfb8","aes-256-ctr","aes-256-ecb","aes-256-
// gcm","aes-256-ofb","aes-256-xts","aes128","aes192","aes256","bf","bf-cbc","bf-cf
// b","bf-ecb","bf-ofb","blowfish","camellia-128-cbc","camellia-128-cfb","camellia-
// 128-cfb1","camellia-128-cfb8","camellia-128-ecb","camellia-128-ofb","camellia-19
// 2-cbc","camellia-192-cfb","camellia-192-cfb1","camellia-192-cfb8","camellia-192-
// ecb","camellia-192-ofb","camellia-256-cbc","camellia-256-cfb","camellia-256-cfb1
// ","camellia-256-cfb8","camellia-256-ecb","camellia-256-ofb","camellia128","camel
// lia192","camellia256","cast","cast-cbc","cast5-cbc","cast5-cfb","cast5-ecb","cas
// t5-ofb","des","des-cbc","des-cfb","des-cfb1","des-cfb8","des-ecb","des-ede","des
// -ede-cbc","des-ede-cfb","des-ede-ofb","des-ede3","des-ede3-cbc","des-ede3-cfb","
// des-ede3-cfb1","des-ede3-cfb8","des-ede3-ofb","des-ofb","des3","desx","desx-cbc"
// ,"id-aes128-CCM","id-aes128-GCM","id-aes128-wrap","id-aes192-CCM","id-aes192-GCM
// ","id-aes192-wrap","id-aes256-CCM","id-aes256-GCM","id-aes256-wrap","id-smime-al
// g-CMS3DESwrap","idea","idea-cbc","idea-cfb","idea-ecb","idea-ofb","rc2","rc2-40-
// cbc","rc2-64-cbc","rc2-cbc","rc2-cfb","rc2-ecb","rc2-ofb","rc4","rc4-40","rc4-hm
// ac-md5","seed","seed-cbc","seed-cfb","seed-ecb","seed-ofb"]


// 使用对称密钥加解密
function doCipher(ciphers, key) {
    ciphers.forEach(item => cipherAlgorithm(item, key));
}

// 加密
function cipher(algorithm, key, data) {
    let encrypted = "";
    let cip = crypto.createCipher(algorithm, key);
    // update inputEncoding: 'utf8', 'ascii', 'latin1'
    // update outputEncoding: ''latin1', 'base64' , 'hex'
    encrypted += cip.update(data, 'utf8', 'hex');
    encrypted += cip.final('hex');
    return encrypted;
}

// 解密
function decipher(algorithm, key, encrypted) {
    let decrypted = "";
    let decipher = crypto.createDecipher(algorithm, key);
    // update inputEncoding: ''latin1', 'base64' , 'hex'
    // update outputEncoding: 'utf8', 'ascii', 'latin1'
    decrypted += decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
}

// 根据不同的 hash 方法获得对应的 hash 值
function cipherAlgorithm(algorithm, key) {
    console.time(`cipher->${algorithm}`);
    // 带加密数据
    let str = 'Hello Cipher';
    let cipherResult = cipher(algorithm, key, str);
    console.timeEnd(`cipher->${algorithm}`);
    console.log(`cipher-${algorithm} key:${key} data:${str} -> ${cipherResult}`);

    // 解密
    console.time(`decipher->${algorithm}`);
    let decipherResult = decipher(algorithm, key, cipherResult);
    console.timeEnd(`decipher->${algorithm}`);
    console.log(`decipher->${algorithm} key:${key} data:${cipherResult} -> ${decipherResult}\n`);
}


let key = 'luckykuku'
doCipher(['blowfish','aes-256-cbc','cast','des','des3','idea','rc2','rc4','seed'],key);
// TODO 非对称加解密
```

## 六、签名和验证算法
我们除了对数据进行加密和解密，还需要判断数据在传输过程中，是否真实际和完整，是否被篡改了。那么就需要用到签名和验证的算法，利用不对称加密算法，通过私钥进行数字签名，公钥验证数据的真实性。
数字签名的制作和验证过程，如下图所示:
![](http://blog.fens.me/wp-content/uploads/2015/03/screenshot.62.png)

使用 git bash 来生成(git 自带)公钥和私钥
```bash
// 生成私钥
openssl genrsa  -out private.pem 2048
// 生成公钥(在这个过程中会提示你输入一些列的信息，例如公司、邮箱等)
openssl req -key private.pem -new -x509 -out public.pem
```
Node.js 代码
```js
function doSign(algorithm) {
    let data = "Hello Sign"; // 待签名数据
    let privatePem = fs.readFileSync('private.pem');
    let key = privatePem.toString();
    // 数字签名
    let sig = signer(algorithm, key, data); 
    console.log(`sig: ${sig}`);

    let publicPem = fs.readFileSync('public.pem');
    let pubkey = publicPem.toString();
    // 验证签名
    let verify1 = verify(algorithm, pubkey, sig, data);// true 
    let verify2 = verify(algorithm, pubkey, sig, 'Hello Kuku');// false
    console.log(`verify: ${verify1} ${verify2}`);
}

// 签名
function signer(algorithm, key, data) {
    let sign = crypto.createSign(algorithm);
    sign.update(data);
    let result = sign.sign(key, 'hex');
    return result;
}

// 验签
function verify(algorithm, key, sig, data) {
    let verify = crypto.createVerify(algorithm);
    verify.update(data);
    let result = verify.verify(key, sig, 'hex');
    return result;
}

doSign('RSA-SHA256');
```


## 参考:
- [Crypto](http://nodejs.org/api/crypto.html)
- [Node.js 加密算法库 Crypto](http://blog.fens.me/nodejs-crypto/)
- [Node.js开发加密货币](http://bitcoin-on-nodejs.ebookchain.org/)
