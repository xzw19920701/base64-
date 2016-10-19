# base64-
 　在iOS中使用RSA加密解密，需要用到.der和.p12后缀格式的文件，其中.der格式的文件存放的是公钥（Public key）用于加密，.p12格式的文件存放的是私钥（Private key）用于解密. 首先需要先生成这些文件，然后再将文件导入工程使用，不多说，开始做！
 
 
 一、使用openssl生成所需秘钥文件
 
 生成环境是在mac系统下，使用openssl进行生成，首先打开终端，按下面这些步骤依次来做：
 以下代码在终端运行:
 
 
 1. 生成模长为1024bit的私钥文件private_key.pem
 openssl genrsa -out private_key.pem 1024
 2. 生成证书请求文件rsaCertReq.csr
 openssl req -new -key private_key.pem -out rsaCerReq.csr
 注意：这一步会提示输入国家、省份、mail等信息，可以根据实际情况填写，或者全部不用填写，直接全部敲回车.
 3. 生成证书rsaCert.crt，并设置有效时间为1年
 openssl x509 -req -days 3650 -in rsaCerReq.csr -signkey private_key.pem -out rsaCert.crt
 4. 生成供iOS使用的公钥文件public_key.der
 openssl x509 -outform der -in rsaCert.crt -out public_key.der
 5. 生成供iOS使用的私钥文件private_key.p12
 openssl pkcs12 -export -out private_key.p12 -inkey private_key.pem -in rsaCert.crt
 注意：这一步会提示给私钥文件设置密码，直接输入想要设置密码即可，然后敲回车，然后再验证刚才设置的密码，再次输入密码，然后敲回车，完毕！
 在解密时，private_key.p12文件需要和这里设置的密码配合使用，因此需要牢记此密码.
 
 温馨提示:会生成6个文件  主要看自己生成的后缀名
 
 private_key.pem
 rsaCerReq.csr
 rsaCert.cr
 rsaCert.crt
 public_key.der 
 private_key.p12
 
 需要的文件是:public_key.der  (公钥)  private_key.p12(私钥)
 