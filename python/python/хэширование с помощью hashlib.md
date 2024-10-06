hashlib - стандартная библиотека python для хэширования данных
## hashlib.scrypt(_password_, _*_, _salt_, _n_, _r_, _p_, _maxmem=0_, _dklen=64_)
- password must be `bytes`
- salt - 16 или больше байтов полученных, например, из `os.urandom()`
n is the CPU/Memory cost factor, r the block size, p parallelization factor and maxmem limits memory (OpenSSL 1.1.0 defaults to 32 MiB). dklen is the length of the derived key in bytes.
```python
import hashlib
import os

def hash(password: bytes):
    salt = os.urandom(16)
    return str(hashlib.scrypt(password, salt=salt, n=2048, p=1, r=8))

passwords = [random_password(10) for _ in range(10_000)] # random_password возвращает рандомный пароль в виде bytes

for password in passwords: # выполнение этого кода заняло
	hash(password)         # 118 сек (синхронное выполнение)
```
### _тут только функция scrypt, потому что в книге пока рассматривалась только она_