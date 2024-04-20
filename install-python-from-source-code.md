https://www.python.org

---

```zsh
cd ~/Downloads; wget https://www.python.org/ftp/python/3.11.2/Python-3.11.2.tar.xz
```

```zsh
tar -xvf Python-3.11.2.tar.xz
```

---

```zsh
brew upgrade openssl
```
or
https://help.dreamhost.com/hc/en-us/articles/360001435926-Installing-OpenSSL-locally-under-your-username

```zsh
vim ~/.zshrc
```
```text
export PATH=/opt/homebrew/opt/openssl@3.1/bin:$PATH
export LD_LIBRARY_PATH=/opt/homebrew/opt/openssl@3.1/lib
export LDFLAGS="-L /opt/homebrew/opt/openssl@3.1/lib -Wl,-rpath,/opt/homebrew/opt/openssl@3.1/lib"
```
```zsh
source .zshrc
```

---

```zsh
cd Python-3.11.2
```

https://docs-python.ru/tutorial/ustanovka-python/ustanovka-ubuntu-debian-ishodnikov/

```zsh
./configure --prefix=/Users/username/opt/python-3.x.x/
            --enable-optimizations
            --with-openssl=/opt/homebrew/opt/openssl@3.1
```

```zsh
make -j8  # 8 - количество ядер
make test
```

```zsh
make altinstall
```

---

```zsh
python3.11 -m ssl
# Nothing.
```
or
```python
>>> import ssl
>>> ssl.OPENSSL_VERSION
'OpenSSL 3.1.0 14 Mar 2023'
```

---

```zsh
make distclean
```

---

```zsh
vim ~/.zshrc
```
```text
export PATH=/Users/pawelasinski/opt/python-3.11.2/bin:$PATH
```
```zsh
source .zshrc
```

---

```python
import urllib.request

url = "https://pypi.org"
with urllib.request.urlopen(url) as f:
    print(f.read(1))

# SSL: CERTIFICATE_VERIFY_FAILED
```

```zsh
python3.10 -m pip install certifi
```

```zsh
vim ~/.zshrc
```
```text
CERT_PATH=$(python3.10 -m certifi)
export SSL_CERT_FILE=${CERT_PATH}
export REQUESTS_CA_BUNDLE=${CERT_PATH}
```
```zsh
source .zshrc
```

---

```zsh
alias py3.x.x="/Users/pawelasinski/opt/python-3.11.2/bin/python3.11"
```

```zsh
vim ~/.zshrc
```
```text
echo alias py3.x.x="/Users/pawelasinski/opt/python-3.11.2/bin/python3.11"
```
```zsh
source .zshrc
```


