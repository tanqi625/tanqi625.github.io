## 【23密码】helloAES (ECB)

#### 题目

```python
from Crypto.Util.number import *
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import random

flag = b'HUBUCTF{*********}'

key = random.randbytes(16)
print(bytes_to_long(key))

aes = AES.new(key=key, mode=AES.MODE_ECB)
print(aes.encrypt(pad(flag, AES.block_size)))

'''
148804906758281503158006299943102617238
b'b5\xd2\xbbh\xfd}\x07s\x13\xab\xe6\xe9?\xde\x94}\xf6\x05\xcaL\xc0`h%\x9b\x82M\xae\xe4\xa1Y/\x87b\xbff4\x7f\nd\x1eTj\xe0\r\xafM'
'''
```

可以看到是一个AES，给了一个示例的加密过程。

**已知条件**  
密文`b'b5\xd2\xbbh\xfd}\x07s\x13\xab\xe6\xe9?\xde\x94}\xf6\x05\xcaL\xc0`h%\x9b\x82M\xae\xe4\xa1Y/\x87b\xbff4\x7f\nd\x1eTj\xe0\r\xafM'`​

key = `148804906758281503158006299943102617238`​

#### 题解

使用AES库的函数进行解密即可，注意密文不要复制遗漏了。

```python
from Crypto.Util.number import long_to_bytes
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

# 已知密钥和密文
key = 148804906758281503158006299943102617238
cipher_text = b'b5\xd2\xbbh\xfd}\x07s\x13\xab\xe6\xe9?\xde\x94}\xf6\x05\xcaL\xc0h%\x9b\x82M\xae\xe4\xa1Y/\x87b\xbff4\x7f\nd\x1eTj\xe0\r\xafM'

# 检查密文长度是否为16字节的整数倍
if len(cipher_text) % 16 != 0:
    print("密文长度不是16字节的倍数，请确认密文是否正确。")
else:
    # 将密钥转换为16字节
    key_bytes = long_to_bytes(key, 16)

    # 使用AES ECB模式解密
    aes = AES.new(key=key_bytes, mode=AES.MODE_ECB)
    plain_padded = aes.decrypt(cipher_text)

    # 去除填充
    try:
        plain_text = unpad(plain_padded, AES.block_size)
        print("解密得到的flag:", plain_text.decode())
    except ValueError:
        print("解密失败，填充无效或密文错误。")

```

答案为`HUBUCTF{11d6254e-f090-4b46-932d-3b75eadd661b}`​

#### 总结

* 加密过程是基于比特，所以密文和key任何时候都需要进行byte的转换
* AES基于128bit的分块，可以看到加密时有一个pad()，这是它自身的填充机制，因此直接解密出现的还会有这些填充的杂质，使用unpad()

‍
