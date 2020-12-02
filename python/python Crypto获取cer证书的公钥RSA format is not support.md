代码
```
from Crypto import Random
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5 as Cipher_pkcs1_v1_5
import base64

def rsa_encrypt(message):
    with open('xx.cer', mode='rb') as f:
        key = f.read()
        rsakey =RSA.importKey(key)
        cipher = Cipher_pkcs1_v1_5.new(rsakey)
        cipher_text = base64.b64encode(cipher.encrypt(message))
    return cipher_text
```python

原因：PyCryptodome 不支持x509格式的证书
解决：先将证书的公钥导出来，然后再使用 RSA.importKey()

## 1.使用openssl进行公钥的导出
openssl x509 -inform der -in xx.per -pubkey -noout > xx.pem

## 2.修改代码如下
```
def rsa_encrypt(message):
    with open('xx.pem', mode='rb') as f:
        key = f.read()
        rsakey =RSA.importKey(key)
        cipher = Cipher_pkcs1_v1_5.new(rsakey)
        cipher_text = base64.b64encode(cipher.encrypt(message))
    return cipher_text
```python    

