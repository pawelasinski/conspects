## 1. Подключение к удалённому серверу

### Основное подключение

```zsh
ssh <user_name>@<remote_host> -p <port>
```
- Если порт по умолчанию (22), параметр `-p <port>` можно опустить.

### Подключение с использованием определённого ключа

```zsh
ssh -i ~/.ssh/id_rsa_example <user_name>@<remote_host>
```
- Опция `-i` указывает, какой приватный ключ использовать.

### Выполнение команды на удалённом сервере

Чтобы выполнить команду на сервере и сразу выйти:
```zsh
ssh <user_name>@<remote_host> <command>
```

Если команда требует интерактивного терминала, добавьте параметр `-t`:
```zsh
ssh -t <user_name>@<remote_host> <command>
```

### Запуск оболочки в определённой директории

Если нужно сразу перейти в определённую директорию после подключения:
```zsh
ssh <user_name>@<remote_host> -t 'cd /path/to/particular/directory && exec $SHELL'
```
- Рекомендуется использовать `&&` вместо `;` для проверки успешности перехода.
- Можно заменить `exec $SHELL` на `exec bash` или просто `bash`.

### Постоянное использование нужной директории

Чтобы не указывать директорию каждый раз:
1. Откройте профиль пользователя (например, `~/.bash_profile`, `~/.profile` или `~/.bashrc`):
```zsh
vim ~/.bash_profile
```
2. Добавьте в конец файла строку:
```sh
cd /path/to/particular/directory
```
3. Сохраните изменения и выполните:
```zsh
source ~/.bash_profile
```
4. Теперь при подключении:
```zsh
ssh <user_name>@<remote_host>
```
   вы автоматически попадёте в нужную директорию.

---

## 2. SCP (Secure Copy Protocol)

### Копирование файлов с локальной машины на удалённый сервер

```zsh
scp /path/to/local/file <user_name>@<remote_host>:/path/to/remote/directory
```

### Рекурсивное копирование директорий

```zsh
scp -r path/to/local/directory/with/nested/one(s) <user_name>@<remote_host>:/path/to/remote/directory
```

### Копирование файлов с удалённого сервера на локальную машину

```zsh
scp <user_name>@<remote_host>:/path/to/remote/file ~/Desktop
```

### Рекурсивное копирование директорий с удалённого сервера

```zsh
scp -r <user_name>@<remote_host>:/path/to/remote/directory/with/nested/one(s) ~/Desktop
```

---

## 3. Проброс STDIN/STDOUT

### Перенаправление вывода удалённой команды в локальный файл

```zsh
ssh <user_name>@<remote_host> <command> > /path/to/local/file
```

### Передача вывода локальной команды на удалённый сервер

```zsh
<command> | scp - <user_name>@<remote_host>:/path/to/remote/file
```

### Передача данных между двумя удалёнными серверами

```zsh
<command> | ssh <user_name_1>@<remote_host_1> 'scp - <user_name_2>@<remote_host_2>:/path/to/remote/file'
```

---

## 4. Fingerprint и безопасность подключения

При первом подключении к серверу появляется сообщение вроде:
```
The authenticity of host '52.307.149.244 (52.307.149.244)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```
- Введите `yes` для сохранения отпечатка (fingerprint) сервера.
- Отпечаток позволяет убедиться, что при повторных подключениях сервер не был изменён или скомпрометирован.

---

## 5. Генерация SSH-ключей

Для подключения без пароля создайте пару ключей:  
- **Приватный (закрытый)** — храните в секрете.  
- **Публичный (открытый)** — можно передавать на сервер.

### Создание пары ключей

```zsh
ssh-keygen
```
Параметры:
- `-t`: тип шифрования (например, rsa).
- `-b`: длина ключа (например, 4096 бит).
- `-C`: комментарий (например, user@machine).
- `-f`: имя файла (например, id_rsa_<new_name>).

> **Примечание:** Последнее поле в ключе (`user@machine`) служит лишь для идентификации и не влияет на аутентификацию.

### Обновление passphrase

Чтобы изменить пароль ключа (passphrase):
```zsh
ssh-keygen -p
```
Просто нажмите Enter, если хотите оставить его пустым.

### Удаление известного ключа сервера

Если необходимо удалить устаревший ключ:
```zsh
ssh-keygen -R <remote_host>
```

Новый ключ сервера можно найти на удалённой машине в файле `/etc/ssh/ssh_host_rsa_key.pub`.

---

## 6. Загрузка публичного ключа на сервер

### Автоматический метод (при наличии пароля)

```zsh
ssh-copy-id <user_name>@<remote_host>
```

### Указание конкретного ключа

```zsh
ssh-copy-id -i ~/.ssh/id_rsa.pub <user_name>@<remote_host>
```

### Использование нестандартного порта

```zsh
ssh-copy-id '<user_name>@<remote_host> -p <port>'
```

### Ручной способ

Скопируйте содержимое файла `~/.ssh/id_rsa.pub` и вставьте его в файл `~/.ssh/authorized_keys` на сервере.  
Например, в macOS можно выполнить:
```zsh
cat ~/.ssh/id_rsa.pub | pbcopy
```

---

## 7. Отключение подключения по паролю

Для повышения безопасности рекомендуется разрешить доступ только по ключу.

1. Откройте конфигурационный файл SSH-сервера:
```zsh
vim /etc/ssh/sshd_config
```
2. Измените следующие параметры:
   - `PasswordAuthentication yes` → `no`
   - `PermitRootLogin yes` → `no`
   - (При необходимости) ограничьте доступ с помощью:
```
AllowUsers <user_name> [root]
```
3. Перезапустите SSH-сервер:
```zsh
sudo service ssh restart
```

---

## 8. SSH-агент

SSH-агент позволяет:
- Хранить ключи в памяти, чтобы не вводить passphrase при каждом подключении.
- Автоматически выбирать нужный ключ для соединения.

### Запуск ssh-agent

```zsh
eval '$(ssh-agent -s)'
```

### Добавление ключа в агент

```zsh
ssh-add ~/.ssh/id_rsa
```
Если путь не указан, агент пытается добавить ключи из:
- `~/.ssh/id_rsa`
- `~/.ssh/id_dsa`
- `~/.ssh/id_ecdsa`
- `~/.ssh/id_ed25519`
- `~/.ssh/identity`

### Просмотр списка ключей

```zsh
ssh-add -L
```

### Удаление ключей

- Удалить конкретный ключ:
  ```zsh
  ssh-add -d ~/.ssh/id_rsa
  ```
- Удалить все ключи:
  ```zsh
  ssh-add -D
  ```

### Добавление ключа с ограничением по времени

Чтобы добавить ключ на 1 час (3600 секунд):
```zsh
ssh-add -t 3600 ~/.ssh/id_rsa_temp
```

> **Важно:** ssh-agent привязан к текущей сессии.

---

## 9. Форвардинг (проброс портов)

### Проброс агента (Agent Forwarding)

Позволяет передавать ключи от одного сервера к другому:
```zsh
ssh -A <user_name_1>@<remote_host_1> ssh <user_name_2>@<remote_host_2>
```

### Реверс-проброс портов

Перенаправляет порт с удалённого сервера на локальную машину:
```zsh
ssh -R <remote_host>:80:localhost:8000 <user_name>@<remote_host>
```

> **Замечание:** Порт 80 является привилегированным. Если нет прав root, используйте порт выше 1024.

### Локальный проброс портов

Перенаправляет локальный порт на удалённый:
```zsh
ssh -L 8000:localhost:8000 <user_name>@<remote_host>
```

После подключения откройте в браузере:
```
http://localhost:8000
```

### Динамический проброс (SOCKS proxy)

```zsh
ssh -D 1080 <user_name>@<remote_host>
```

#### Проверка пропускной способности сети с использованием iperf3

Установка:
- macOS:
  ```zsh
  brew install iperf3
  ```
- Linux:
  ```zsh
  sudo apt install iperf3
  ```

Запуск сервера:
```zsh
iperf3 -s
```

Запуск клиента:
```zsh
iperf3 -c localhost -p 1080
```
При выполнении команды клиент инициирует тест, отправляя данные серверу, и измеряет параметры сетевого соединения, такие как пропускная способность, задержка и, при необходимости, потерю пакетов.

### Проверка доступности удаленного сервера

```zsh
ssh -T <user_name>@<remote_host>
```

---

## 10. SSH-алиасы

Чтобы упростить подключение, можно настроить алиасы в файле конфигурации.

1. Откройте файл:
```zsh
vim ~/.ssh/config
```
2. Добавьте настройки:
```text
Host <alias>
	Hostname <remote_host>
	User <user_name>
    Port <port>
    IdentityFile ~/.ssh/id_rsa_example
    IdentitiesOnly yes
```
3. Теперь можно подключаться по алиасу:
```zsh
ssh <alias>
```

---

## Дополнительные ссылки

- [Что такое протокол SSH | Hexlet](https://guides.hexlet.io/ru/ssh)
- [Памятка пользователям SSH | Habr](https://habr.com/ru/post/122445)