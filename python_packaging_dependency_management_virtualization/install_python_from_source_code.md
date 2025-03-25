https://www.python.org

---

This guide walks you through building Python (for example, Python 3.11.2) from source code.

---

## 1. Download Python Source

Change to your Downloads folder and download the source archive:

```zsh
cd ~/Downloads
wget https://www.python.org/ftp/python/3.11.2/Python-3.11.2.tar.xz
```

## 2. Extract the Archive

Unpack the downloaded tarball:

```zsh
tar -xvf Python-3.11.2.tar.xz
```

---

## 3. Install and Configure OpenSSL

Before building Python, upgrade OpenSSL via Homebrew (or follow the [DreamHost guide](https://help.dreamhost.com/hc/en-us/articles/360001435926-Installing-OpenSSL-locally-under-your-username)):

```zsh
brew upgrade openssl
```

Edit your shell configuration file (e.g., `~/.zshrc`) to include OpenSSL’s paths:

```zsh
vim ~/.zshrc
```

Append the following lines (adjust the OpenSSL version/path if necessary):

```bash
export PATH=/opt/homebrew/opt/openssl@3.1/bin:$PATH
export LD_LIBRARY_PATH=/opt/homebrew/opt/openssl@3.1/lib:$LD_LIBRARY_PATH
export LDFLAGS="-L/opt/homebrew/opt/openssl@3.1/lib -Wl,-rpath,/opt/homebrew/opt/openssl@3.1/lib"
```

Reload your shell configuration:

```zsh
source ~/.zshrc
```

---

## 4. Configure the Python Build

Navigate into the extracted source directory:

```zsh
cd Python-3.11.2
```

Run the configure script with your desired installation prefix, enabling optimizations and specifying the OpenSSL directory. Be sure to replace `/Users/username/opt/python-3.x.x/` with your actual path:

```zsh
./configure --prefix=/Users/username/opt/python-3.x.x/ \
            --enable-optimizations \
            --with-openssl=/opt/homebrew/opt/openssl@3.1
```

---

## 5. Build and Test Python

Compile Python using parallel jobs (here using 8 cores):

```zsh
make -j8  # 8 refers to the number of cores available
```

(Optional) Run the test suite:

```zsh
make test
```

Install Python using "altinstall" to avoid overwriting the system default:

```zsh
make altinstall
```

---

## 6. Verify the Python Installation

Check that SSL support is correctly configured:

```zsh
python3.11 -m ssl
```

Alternatively, in an interactive Python shell:

```python
>>> import ssl
>>> ssl.OPENSSL_VERSION
'OpenSSL 3.1.0 14 Mar 2023'
```

---

## 7. Clean Up Build Files

Once installation is confirmed, clean the build directory:

```zsh
make distclean
```

---

## 8. Update Your PATH Environment Variable

To easily run your newly installed Python, update your PATH. Open your shell configuration file:

```zsh
vim ~/.zshrc
```

Add the following line (adjust the path as needed):

```bash
export PATH=/Users/username/opt/python-3.11.2/bin:$PATH
```

Reload your shell:

```zsh
source ~/.zshrc
```

---

## 9. Resolve SSL Certificate Verification Issues

If you encounter SSL errors when accessing HTTPS URLs (e.g., with `urllib.request`), install the `certifi` package using your system’s Python:

```zsh
python3.11 -m pip install certifi
```

Then update your shell configuration to point to the installed certificate bundle:

```zsh
vim ~/.zshrc
```

Append these lines:

```bash
CERT_PATH=$(python3.11 -m certifi)
export SSL_CERT_FILE=${CERT_PATH}
export REQUESTS_CA_BUNDLE=${CERT_PATH}
```

Reload your configuration:

```zsh
source ~/.zshrc
```

---

## 10. Create an Alias for Convenience

For easier access, set up an alias for your new Python version. Open your shell configuration:

```zsh
vim ~/.zshrc
```

Add the alias (replace `py3.x.x` with your desired alias):

```bash
alias py3.x.x="/Users/username/opt/python-3.11.2/bin/python3.11"
```

Reload the shell:

```zsh
source ~/.zshrc
```
