### ...

В некоторые каталоги попасть очень тяжело и программы в них обновляют медленно. Связано это с тем, что разработчики стараются добавлять туда только проверенный софт.
В других всё происходит просто и быстро. В любом случае необходимо пройти некоторую процедуру, после которой программа будет добавлена.
Это один из ключевых аспектов, по которому дистрибутивы Linux отличаются друг от друга.

В Unix-системах регистр имеет значение. macOS в этой ситуации идёт по пути Windows и тоже не учитывает регистр.
В отличие от Windows, в Unix-системах отсутствует понятие "расширение файла".

Длинный формат опций помогает легче понимать, что они означают (те, которые начинаются с --). Опции можно комбинировать.

#### JQ

```zsh
brew install jq
```

![json.txt](json.txt)

```zsh
cat json.txt | jq '.name'
cat json.txt | jq '.location.city'
cat json.txt | jq '.employees[0].name'
cat json.txt | jq '.location | {street, city}'
```

#### GPG

```zsh
apt-get update && apt install -y gpg
```

```zsh
gpg --full-gen-key
```

```zsh
echo "keyid-format 0xlong 
throw-keyids
no-emit-version
no-comments" > ~/.gnupg/gpg.conf
```

```zsh
gpg -k,--list-keys <- PUB
gpg -K,--list-secret-keys <- SEC
```

Экспорт своего приватного ключа
```zsh
gpg --export-secret-key -a,--armor -o,--output my_sec_key.gpg my_email@gmail.com
```

Экспорт своего публичного ключа, чтобы отправить его собеседнику
```zsh
gpg --export -a,--armor -o,--output my_pub_key.[pub|gpg] my_email@gmail.com
```

Импорт публичного ключа собеседника
```zsh
gpg --import interlocator's_pub_key.[pub|gpg]
```

Зашифровать файл my-secret-information.txt ключом собеседника
```zsh
gpg -e,--encrypt --sign [-a,--armor] -r,--recipient interlocator's_email@gmail.com my-secret-information.txt
# gpg my-secret-information.txt.[asc|gpg]
```
Если вы указали флаг `--sign`, то к зашифрованному файлу будет добавлена цифровая подпись, чтобы убедиться в его подлинности.

```zsh
gpg -d,--decrypt -o,--output my-secret-information.txt my-secret-information.txt.[asc|gpg]
```

```zsh
gpg --delete-secret-keys my_email@gmail.com 1
gpg --delete-keys my_email@gmail.com 2
```

#### PASS

```zsh
apt-get update && apt-get install -y pass
```

```zsh
pass init my_email@gmail.com
```

```zsh
pass insert social/vk
pass insert social/vk -m
```
  
```zsh
pass = tree
```

```zsh
cat .password-store/social/vk.gpg
pass social/vk
```

```zsh
pass generate social/vk 15
```

```zsh
pass rm social/vk
```

#### XXD

```zsh
echo hello_world > hello_world.txt
```

```zsh
xxd hello_world.txt
# 00000000: 6865 6c6c 6f2c 2077 6f72 6c64 0a         hello, world.
```

```zsh
xxd -c 1 -b hello_world.txt
# 00000000: 01101000  h
# 00000001: 01100101  e
# 00000002: 01101100  l
# 00000003: 01101100  l
# 00000004: 01101111  o
# 00000005: 00101100  ,
# 00000006: 00100000   
# 00000007: 01110111  w
# 00000008: 01101111  o
# 00000009: 01110010  r
# 0000000a: 01101100  l
# 0000000b: 01100100  d
# 0000000c: 00001010  .
```
	-c  # character
	-b  # binary

```zsh
xxd -p calc.exe calc_dump.txt
```
```zsh
xxd -r -p calc_dump.txt calc.exe
```
	-p,--plain  # flag specifies that the input is in plain hex format (without line numbers or ASCII representation)
	-r,--reverse  # reversed

#### DOTENV

[github.com/theskumar/python-dotenv](https://github.com/theskumar/python-dotenv)

### НАВИГАЦИЯ

```zsh
pwd
```

```zsh
cd
```
	.  # из текущей директории на уровень ниже
    ../..  # из текущей директории на уровень выше
	 <без_аргументов>  # выброс в домашнюю директорию
     ~/  # заменяет абсолютный путь

```zsh
tree
```

### ПЕРЕМЕННЫЕ ОКРУЖЕНИЯ

Они существуют в рамках запущенной сессии командного интерпретатора, подгружаются туда во время его инициализации (но это не единственный путь их появления).

Основное предназначение переменных окружения — конфигурирование системы и программ.
#### Просмотреть все переменные

```zsh
env
```
or
```zsh
export (-p)
```
If you wish to view all exported variables on the current shell and for exporting to child shells.
#### Просмотреть определенную переменную

```zsh
echo $<ENV_VAR>
```
or
```zsh
printenv <ENV_VAR>
```
#### Экспорт переменных

```zsh
export MYVAR=1729
echo $MYVAR
# 1729
bash
echo $MYVAR
# 1729
```
vs
```zsh
MYVAR=1729
echo $MYVAR  # 'printenv MYVAR' не работает в данном случае
# 1729
bash
echo $MYVAR
# 
```

```zsh
<ENV_VAR>='<value>' ... <command>
```
Изменение для запущенной команды, НЕ для текущей сессии!

```zsh
 export <ENV_VAR>=$<EXISTING_ENV_VAR>
```

```zsh
export <ENV_VAR>='<value>'
```
Глобальный способ задания значения переменной для текущей сессии.

```zsh
func() { echo "hi"; }
func
# hi
bash
func
# bash: func: command not found
exit
export -f func
bash
func
# hi
```
На MacOS не работает, на Linux — да.

#### PATH

Еcли файл с одним и тем же именем находится одновременно в нескольких директориях, то будет найден тот, который находится в директории, расположенной левее в PATH.

`.bashrc` | `.zshrc`
`.bash_profile` | `.profile`

Для обновления переменной оболочки PATH включаем дополнительный путь в существующее определение PATH следующим образом:
```zsh
export PATH=$PATH:/path/to/file
```

Для вновь устанавливаемых программ обычно это `/usr/local/bin`.

Прямой запуск программ всегда должен быть путём до файла, например, `/path/to/executable/file` или `./`. Ради безопасности!

#### SET

Вывод списка всех локальных переменных:
```set
set 
```

Удалить локальную переменную:
```zsh
unset <ENV_VAR>
```

To remove a variable from export:
```zsh
export -n <ENV_VAR>
```


```zsh
set -a            # Enable allexport using single letter syntax
set -o allexport  # Enable using full option name syntax
```

```zsh
set +a            # Disable allexport using single letter syntax
set +o allexport  # Disable using full option name syntax
```


```zsh
echo 'export MYVAR=1729' > myscript.sh
chmod +x myscript.sh
./myscript.sh
echo $MYVAR
# 
```
or
```zsh
echo 'MYVAR=1729' > myscript.sh
source myscript.sh
echo $MYVAR
# 1729
```

#### АРХИВЫ

```zsh
zip --help
unzip --help
gzip --help
tar --help
```

### ТЕКСТОВЫЕ РЕДАКТОРЫ

```zsh
vim <filename>
vi <filename>
nano <filename>
```

[How do I exit from Vim?](how-do-I-exit-from-vim.md)

#### Чтение файлов

```zsh
head <filename>  # вывести первые 10 строчек
```
	-n  # вывести другое количество

```zsh
tail <filename>  # вывести последние 10 строчек
```
	-n  # вывести другое количество
	-f  # не просто выводит последние строчки файла, но ждёт появления новых; для выхода - Ctrl + C

---

```zsh
cat <filename>
```
	-n  # вывести с нумерацией строк (начинается с 1)

```zsh
cat some_file.json | python -m json.tool  # для красивого вывода JSON файлов
```

### СИМВОЛИЧЕСКИЕ ССЫЛКИ

Если необходимо связать папки на постоянной основе, возможно, лучшим решением будет создать символическую ссылку.

```zsh
ln -s <filename> <softlink>
```
	Основные особенности символических ссылок:
	- Могут ссылаться на файлы и каталоги;
	- После удаления, перемещения или переименования файла становятся недействительными;
	- Права доступа и номер inode отличаются от исходного файла;
	- При изменении прав доступа для исходного файла, права на ссылку останутся неизменными;
	- Можно ссылаться на другие разделы диска;
	- Содержат только имя файла, а не его содержимое.

```zsh
ln <filename> <hardlink>
```
	Основные особенности жестких ссылок:
	- Работают только в пределах одной файловой системы;
	- Нельзя ссылаться на каталоги;
	- Имеют ту же информацию inode и набор разрешений что и у исходного файла;
	- Разрешения на ссылку изменяться при изменении разрешений файла;
	- Можно перемещать и переименовывать и даже удалять файл без вреда ссылке.

### КОМАНДНАЯ СТРОКА | SHELL, КОМАНДНАЯ ОБОЛОЧКА, КОМАНДНЫЙ ИНТЕРПРЕТАТОР, КОМАНДНЫЙ ПРОЦЕССОР, REPL | ТЕРМИНАЛ vs. КОНСОЛЬ

```zsh
clear  # = Ctrl + L
```

```zsh
reset
```

```zsh
exit
```

```bash
Ctrl + Shift + C
Ctrl + Shift + V
```

---
```zsh
command_1; command_2  # последовательное выполнение
```

```zsh
(command_1 &); (command_2 &)  # параллельное выполнение
```

```zsh
command_1 && command_2  # запуск нескольких команд за раз при условии, что предыдущая команда была выполнена успешно
```

```zsh
command_1 || command_2  # запуск нескольких команд за раз при условии, что предыдущая команда была выполнена НЕуспешно
```

```zsh
(subshell)  # запуск подпроцесса
```

```zsh
command &  # запуск в фонов режиме
```

### СИСТЕМА

```bash
poweroff
```

```bash
reboot
```

---

```zsh
uname  # печатает определенные сведения о системе. Если ПАРАМЕТР не задан, то подразумевается -s.
```
	-a, --all  # напечатать всю информацию, в следующем порядке, кроме -p и -i, если они неизвестны:
	-s, --kernel-name  # напечатать имя ядра
	-n, --nodename  # напечатать имя машины в сети
	-r, --kernel-release  # напечатать информацию о выпуске ядра
	-v, --kernel-version  # напечатать версию ядра
	-m, --machine  # напечатать тип оборудования машины
	-p, --processor  # напечатать тип процессора (непереносима)
	-i, --hardware-platform  # напечатать тип аппаратной платформы (непереносима)
	-o, --operating-system  # напечатать имя операционной системы

### МАНИПУЛИРОВАНИЕ ФАЙЛОВОЙ СТРУКТУРОЙ

```zsh
> <filename>  # очистить файл, не удаляя его
```

```zsh
echo <text> > <filename>  # перезапись файла
```

```zsh
$ echo <text> >> <filename>  # добавление в файл
```

---

```zsh
echo <sometext> | xclip  # копировать <sometext> в буфер обмена
xclip -o >> <filename>  # вставить содержимое буфера в текст файла
xsel >> <filename>  # вставить содержимое буфера в текст файла
```

В MacOS (и Linux при желании) есть удобная консольная команда `pbcopy`, которая позволяет копировать что-то в системный буфер обмена. Например, мы хотим скопировать строку `wow` в системный буфер:
```zsh
echo wow | pbcopy
```

Скопировать файл в буфер обмена:
```zsh
cat file.txt | pbcopy
```

Команда `pbpaste` вставляет текущее значение системного буфера обмена.  
  
Утилита `pass`, которая хранит пароли, умеет копировать данные в системный буфер обмена:
```zsh
pass somefile -c
```
Это скопирует первую строку из файла `somefile`. Если нужна вторая, то передаём `-c2`, третья — `-c3` и т.д.  
  
Копирование из `nvim` в системный буфер — выделяем текст в визуальном режиме (нажав `v` и выбрав нужный текст) и тыкаем `*y` или `+y`. В случае Linux предварительно установить `xclip` и `xsel`.

---

```zsh
type <command_name>
whereis <command_name>
which <command_name>
```

---

```zsh
dirname <path>
basename <path>
```

---

```zsh
ls
```
	<without_args>  # текущую директорию
	<abspath>  # абсолютный путь директории
	<dirname>  # просто название директории, которую нужно проанализировать (выше - нельзя, ниже - можно)
     ..  # директорию выше
     .  # текущую директорию
     -a  # опция показывает и скрытые (которые начинаются с точки) директории и файлы
     -l  # опция показывает дополнительную информацию, ls -l = ll
     -d  # информация о текущем каталоге
     -h  # информация о размерах

---

```zsh
touch <filename>
```
	Основная задача утилиты — поменять время последнего доступа к файлу, но она обладает побочным эффектом - если файла не существует, то он будет создан.

---

```zsh
mkdir <dirname>
```
	-p <dirname>/<dirname>

---

```zsh
rm <filename>
```
	-r <dirname>  # для удаления директории
    -f <filename>  # для удаления файлов с ограниченными правами доступа

---

```zsh
mv <current_filename> <new_filename>  # перемещение=переименование файла
```

---

```zsh
cp <current_filepath> <new_filepath>  # копирование файла
     
```
	-r <current_dirpath> <new_dirpath>  # для копирования директории

---

```zsh
wc <filename> -l  # узнать количество строк в файле
```
 
### ИСТОРИЯ

Самый простой способ просматривать историю команд - нажимать клавиши "вверх/вниз".

Если не установлен HISTFILESIZE, то файл `.bash_history` будет расти бесконечно.

```zsh
history
```
	<n>  # выводит последние n команд

```zsh
!<номер команды>  # для повторного запуска
```

* `Ctrl + R` - для рекурсивного поиска (он ожидает ввода символов и сразу отображает ближайшую команду, в которой эти символы встречаются. Если найденное соответствие вас не устроило, то повторное нажатие выберет следующее соответствие из истории).
- `Ctrl + U` -> `Ctrl + Y`  - запоминаем набираемую команду
- Введите `Space` перед командой, чтобы она не сохранилась в истории
- Использование аргумента предыдущей команды с помощью `!$`
* Использование предыдущей команды в текущей с помощью `!!`

### ПРОЦЕССЫ

`Ctrl + C` заставляет терминал послать сигнал SIGINT процессу, который на данный момент его контролирует. Когда foreground-программа получает сигнал SIGINT, она обязана прервать свою работу.

`Ctrl + D` говорит терминалу, что надо зарегистрировать так называемый EOF (End of File – конец файла), то есть поток ввода окончен. Bash интерпретирует это как желание выйти из программы.
При работе в конкретной программе могут срабатывать оба способа, но может только один. Выйти из интерпретатора Python с помощью `Ctrl + C` нельзя.

`Ctrl + Z` посылает процессу сигнал, который приказывает ему остановиться. Это значит, что процесс остается в системе, но как бы замораживается. Само собой разумеется, что он уходит в бэкграунд (background) – в фоновый режим. С помощью команды `bg` его можно снова запустить, оставив при этом в фоновом режиме. Команда `fg` не только возобновляет ранее приостановленный процесс, но и выводит его из фона на передний план.

```zsh
ps
fg <CMD or ID>
bg <CMD or ID>
```

```zsh
jobs
fg %<ID>
bg %<ID>
```

https://younglinux.info/bash/ctrl-c

#### Родительские и дочерние процессы

Все процессы могут быть родительскими и дочерними одновременно. Единственным исключением является процесс init - родительский для всех процессов, запущенных в системе Linux.

```zsh
ps -p 1
# PID TTY          TIME CMD
# 1 ?        00:00:02 init
```
	p, -p, --pid pidlist - select by process ID.

Любой создаваемый процесс имеет родительский процесс, из которого он создается, и может быть определен как потомок этого родительского процесса.

```zsh
echo $$
# 27861
bash
echo $$
# 28034
ps --ppid 27861
# PID TTY          TIME CMD
# 28034 pts/3    00:00:00 bash
```
	--ppid pidlist - select by parent process ID.


### ПЕРЕНАПРАВЛЕНИЕ ПОТОКОВ

> <, >, >> — их можно комбинировать
> STDIN — 0, STDOUT — 1, STDERR — 2

Сначала STDERR перенаправляется в STDOUT, затем STDOUT в файл
```zsh
cd lala > output 2>&1
cat output
# bash: cd: lala: Нет такого файла или каталога
```

STDERR просто перенаправляется в STDOUT
```zsh
cd lala 2>&1
# bash: cd: lala: Нет такого файла или каталога
```

Таким образом можно сразу перенаправить STDERR в файл
```zsh
cd lala 2> output
cat output
# bash: cd: lala: Нет такого файла или каталога
```

Оба потока, STDERR и STDOUT, перенаправляются в файл
```zsh
cd lala &> output
cd lala >& output
cat output
# bash: cd: lala: Нет такого файла или каталога
```

---

### CRON

```zsh
crontab -l
crontab -e
<* * * * * command> | crontab
```

---
### ...

```bash
say <текст>
```

---

```zsh
date
```

```zsh
cal <year>
```

---

```zsh
echo $((<выражение с операндами и операторами>))  # калькулятор
```

---

```zsh
man <command>
```

```zsh
<command> --help
```

---

```zsh
!<command>  # запуск команд в Jupyter Notebook/Colab
```

---

```zsh
ping 8.8.8.8
```

---
#### HOW TO CHECK FILE'S INTEGRITY BY DOWNLOADING THE SHA256 FILE

```zsh
brew install coreutils
```

```zsh
vim ~/.zshrc
```
```text
export PATH=/opt/homebrew/opt/coreutils/libexec/gnubin:$PATH
```
```zsh
source .zshrc
```

```zsh
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz.sha256
```

```zsh
sha256sum openssl-1.1.1g.tar.gz
# ddb04774f1e32f0c49751e21b67216ac87852ceb056b75209af2443400636d46 openssl-1.1.1g.tar.gz
cat openssl-1.1.1g.tar.gz.sha256
ddb04774f1e32f0c49751e21b67216ac87852ceb056b75209af2443400636d46
```
#### Как хранить/юзать ключи в проекте
	setenv.sh -> .gitignore