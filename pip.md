Начиная с Python версии 3.4, pip поставляется вместе с интерпретатором языка Python.

Если pip отсутствует, то его можно установить двумя способами:
- при помощи модуля ensurepip, который обеспечивает поддержку начальной загрузки pip в виртуальную среду или существующую установку Python:
```zsh
python3 -m ensurepip
```

- при помощи скрипта установки get-pip.py, который можно скачать при помощи утилиты bash wget с сайта https://bootstrap.pypa.io/:
```zsh
wget https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
```

---

```zsh
python3 -m ensurepip --upgrade
pip install -U,--upgrade pip
```

---

Если вы не можете запустить pip команду напрямую (возможно, из-за отсутствия пути до директории с Python в системной переменной PATH), вы можете запустить pip через интерпретатор Python:

```zsh
python3 -m pip <arguments>
```

---

```zsh
pip download <package_name>
```

```zsh
pip install <package_name>  # установить последнюю версию пакета
pip install <package_name>==1.0.4  # установить определенную версию пакета
pip install <package_name>>=1.0.4  # установить минимальную версию пакета
pip install <package_name> --user  # установить пакет в домашнем каталоге, т.е. без sudo
pip install --force-reinstall <package_name>  # переустановить пакет, даже если он последней версии
pip install -e,--editable <path/to/local/package>  # используется при попытке установить пакет локально, чаще всего в случае его разработки в своей системе. Он просто свяжет пакет с исходным местоположением, что в основном означает, что любые изменения в исходном пакете будут отражаться непосредственно в вашей среде.
pip install --install-option="--no-deps"
```

```zsh
pip freeze > requirements.txt
pip install -r requirements.txt
```

```zsh
pip uninstall <package_name>
```

```zsh
pip list
pip list -o,--outdated
pip show <package_name>
pip search <package_name>
pip check <package_name>  # проверяет, что установленный пакет имеет совместимые зависимости
```

```zsh
pip help
```