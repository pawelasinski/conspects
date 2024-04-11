[Learn Git Branching Sandbox](https://learngitbranching.js.org/?locale=ru_RU)

### INSTALLATION

Mac:
```zsh
brew install git 
```

Linux:
```zsh
sudo apt-get -y update && sudo apt-get -y upgrade
sudo apt-get install -y git 
```

### CONFIG

```zsh
git config
```  
	--global  # использовать глобальный файл конфигурации
	--system  # использовать системный файл конфигурации
	--local  # использовать файл конфигурации репозитория. Локальные настройки имеют приоритет над глобальными и системными настройками.
	-f,--file <file_name>  # использовать указанный файл конфигурации
	
	--get <key>  # get value
	--get-all <key>  # get all values
	--add <key <value>  # add new a variable
	--unset <key>  # remove a variable
	--unset-all <key>  # remove all matches
	-l,--list  # show all list of variables
	-e,--edit  # open in editor

| Key             | Value                                     |
| :-------------- | :---------------------------------------- |
| user.name       | <user_name>                               |
| user.email      | <user_email>                              |
| core.editor     | vim                                       |
| commit.template | "add"  # can be both a path and some text |
| alias.st        | "status"                                  |
| core.longpaths  | true                                      |

### INIT

```zsh
mkdir <dir_name>
```

Пустые директории в git не добавляются в принципе. Физически директория находится в рабочей директории, но её нет в git, и он её игнорирует.

В <dir_name>:
```zsh
git init
```

### CLONE

```zsh
git clone <HTTPS/SSH> <dir_name>
```

### ИНТЕГРАЦИЯ С GITHUB

```zsh
git branch -M main
git remote add origin git@github.com:<username_on_github>/<reponame_on_github>.git
git push -u origin main
```

### РАБОТА С УДАЛЕННЫМ РЕПОЗИТОРИЕМ

Чтобы просмотреть список настроенных удалённых репозиториев
```zsh
git remote
```
	-v  # чтобы просмотреть адреса для чтения и записи, привязанные к репозиторию
    show <remote_name>  # получить побольше информации об одном из удалённых репозиториев
    rename <old_name> <new_name>
    remove <remote_name>

```zsh
git push <remote_name> -d,--delete <branch_name>
git push <remote_name> :<branch_name>
```

### PULL

When the current branch is main:

```zsh
git fetch <origin (имя удаленного репозитория)> <main (имя ветки)> (или <source>:<target> (можно использовать относительные ссылки; если источник пустой, то target создается))
```

`<источник>:<получатель>` сработает для ветки, на которой вы не находитесь в настоящий момент.

Если команда `git fetch` выполняется без аргументов, она скачивает все-все коммиты с удалённого репозитория и помещает их в соответствующие удалённо-локальные ветки в локальном репозитории.

```zsh
git merge <origin/main (имя удаленного репозитория/имя ветки)>
```

or
```zsh
git pull <origin (имя удаленного репозитория)> <main (имя ветки)>
```

```zsh
git fetch <origin (имя удаленного репозитория)> <main (имя ветки)> (или <source>:<target> (можно использовать относительные ссылки; если источник пустой, то target создается))
```
```zsh
git rebase <origin/main (имя удаленного репозитория/имя ветки)>
```

or
```zsh
git pull --rebase <origin (имя удаленного репозитория)> <main (имя ветки)>
```

### STASH

```zsh
git stash push
```
	-p,--patch -> git add -i
	-S,--staged
	-k,--keep-index;--no-keep-index
	-u,--include-untracked
	-a,--all
	-m,--message <message>

```zsh
git stash branch <branch_name>
```

```zsh
git stash list
```
	+ log-options

```zsh
git stash show <stash_number>
```
	+ diff-options
	-u,--include-untracked;--only-untracked

```zsh
git stash apply <stash_number>
```

```zsh
git stash pop <stash_number>
```

```zsh
git stash drop <stash_number>
```

```zsh
git stash clear
```

### .gitignore

Каждая строчка — это шаблон, по которому происходит игнорирование.

* Игнорируется файл в любой директории проекта:
```text
access.log
```

* Игнорируется директория в любой директории проекта:
```text
node_modules
```

* Игнорируется директория в корне git-репозитория:
```text
/coverage
```

* Игнорируются все файлы с расширением sqlite3 в директории db, но не игнорируются такие же файлы внутри любого вложенного каталога в db, например /db/something/lala.sqlite3:
```text
/db/*.sqlite3
```

* Игнорировать все .txt файлы в каталоге doc/ на всех уровнях вложенности:
```text
doc/**/*.txt
```

### ИНДЕКС

```zsh
git add <path_to_dir_w_files_or_file>
```
	.  # добавление всех изменений в индекс
	-i <path_to_dir_w_files_or_file>  # показывает измененные куски файлов и спрашивает, что с ними сделать

```zsh
git rm  # rm + git add
```
	--cached <filename>  # if you want to remove files from the Git but keep the files in your local repository 

### ОТМЕНА ИЗМЕНЕНИЙ В РАБОЧЕЙ ДИРЕКТОРИИ

> Откат незакоммиченных изменений безвозвратен.

```zsh
git clean -n  # "Будет удалено ..."
```
	-f  # удалить неотслеживаемые файлы из рабочей директории
	-fd  # + директории
	-i  # интерактивный режим

```zsh
git restore <path_to_filename(s)>  # откат изменений в отслеживаемых файлах/папках
```
	--staged <path_to_dir_w_files_or_file> <path_to_dir_w_files_or_file>  # откат изменений, которые попали в индекс

Такой командой можно удалить все наши эксперименты на ветке.
```zsh
git checkout HEAD, master, …
git checkout <filename>
git checkout -- <path_to_filename(s)>
```
	-f, --force  # если нужно сделать принудительно
	Можно HEAD не указывать, используется по умолчанию.
Если файлы между ветками различны, то ошибка. Если одинаковы, то изменения беспрепятственно переносятся между ветками.

```zsh
git checkout <filename>  # восстановить удаленный файл
git checkout $<хеш> <filename>  # восстановить удаленный файл из определенной временной точки
```

Сброс локальной ветки до состояния удалённой
```zsh
git fetch <remote_branch_name>
git reset --hard origin/<remote_branch_name>
```
При наличии коммита, который нужно сохранить, перед сбросом нужно создать новую ветку и произвести коммит.

### COMMIT

```zsh
git commit
```
	-m "<Комментарий>"
    <Название файла> -m "<Комментарий>"  # только для отслеживаемых файлов
    -am "<Комментарий>"  # добавление всего в индекс, а затем - коммит
    --amend  # добавление изменений в текущий коммит, + -m, чтобы изменить навзвание коммита напрямую, без редактора
	--no-edit  # без редактора

[Как склеить коммиты и зачем это нужно](https://htmlacademy.ru/blog/git/how-to-squash-commits-and-why-it-is-needed)

### ОТМЕНА КОММИТОВ

```zsh
git revert <хеш>  # добавление ещё одного коммита с противоположными действиями данному, для удаленных репозиториев
```
	-m parent-number  # чтобы избежать конфликта с удалением merge-commit
	-n HEAD  # простая отмена последнего коммита


```zsh
git reset --mixed (by default) HEAD~ (or @~)  # отмена последнего коммита, кладет отменненые изменения в work directory, для локальных репозиториев
```
	--soft HEAD~  # отмена последнего коммита, кладет отменненые изменения в индекс
	--hard HEAD~2  # полное удаление последних двух коммитов и файлов/директорий с изменениями
	--hard HEAD@{1} (or <хеш>)  # восстановление последнего удаленного коммита и файла/директории из истории

Git позволяет удалять коммиты. Это опасная операция, которую нужно делать только в том случае, если речь идет про новые коммиты, которых нет ни у кого, кроме вас. Если коммит был отправлен во внешний репозиторий, то менять историю ни в коем случае нельзя, это сломает работу у тех, кто работает с вами над проектом.


Чтобы переместить коммит в нужную ветку из той, куда был сделан коммит по ошибке, нужно переключиться на новую ветку, которую вы забыли предварительно создать:
```zsh
git checkout -b <new_branch_name>
```
Затем переключиться к оригинальной ветке:
```zsh
git checkout <affected_branch_name>
```
Откатиться до последнего коммита, который нужно сохранить, и сбросить 
```zsh
git reset --hard <хэш_последнего_коммита>
```

### PUSH

```zsh
git push <remote> <branch>
```
	можно и без аргументов, но тогда это будет непринудительный push, то есть если голова указывает не на удаленную ветку, то ничего не произойдет
	<remote> <source>:<target>  # можно использовать относительные ссылки; если источник пустой, то target удаляется
    -u, --set-upstream <remote> <branch>  # для связи локальной ветки с веткой удалённого репозитория
    -f, --force <remote> <branch>  # удаляет из ветки на сервере все коммиты, которых нет в локальной версии, и записывает новые
    --force-with-lease <remote> <branch>  # заставляет команду завершиться с ошибкой, если в удалённом репозитории есть коммиты, добавленные другими пользователям
    --all <remote>  # push all of your local branches

[Откат ошибочной команды `git push --force`](https://gist.github.com/Envek/13d9e406bb2af23f739197e3934ad4f0)

### АНАЛИЗ ИЗМЕНЕНИЙ

```zsh
git diff
```
	--staged  # + изменения, которые попали в индекс
Навигация по пейджеру: `b` - вниз, `f` - вверх, `q` - выход, ...

---

```zsh
git status
```
	-s  # краткий статус

---

```zsh
git log
```
	-p  # с дифом для каждого коммита
	--oneline
	--graph
	--all
	--author="<commiter>"
	--diff-filter=D --summary  # покажет список коммитов, в которых удалялись файлы
	--follow  # позволяет вывести все изменения над файлом, даже если в процессе работы он был переименован
	Если добавить имя файла в конец команды, отделив его знаками `--`, можно увидеть в каких коммитах он изменялся

Просмотр всех неотправленных коммитов
```zsh
git log --branches --not --remotes
git log origin/master..HEAD
```

Эта команда выведет список, отсортированный в порядке убывания количества коммитов
```zsh
git shortlog -s -n
```

---

```zsh
git gc  # сборщик мусора
```

Удаление старых веток, стёртых из внешнего репозитория
```zsh
git-remote prune <remote_branch_name>
```

---

How do I list all the files in a commit?
```zsh
git diff-tree --no-commit-id --name-only -r <хеш> 
```
	--no-commit-id - suppresses the commit ID output
	--name-only - shows only the file names that were affected
	-r - argument is to recurse into subtrees

```zsh
git show --pretty="" --name-only <хеш>
```
	--pretty specifies an empty format string to avoid the cruft at the beginning
	--name-only shows only the file names that were affected

---

Просмотр старой ревизии файла
```zsh
git show <хеш>:<filename>
```

```zsh
git show <хеш>  # можно посмотреть все изменения, сделанные в рамках одного коммита
```
> Хеш довольно длинный, поэтому можно оперировать только первыми 4 или 8 символами.

---

```zsh
git blame <path_to_dir_w_files_or_file>  # можно узнать, кто последним менял конкретную строку в файле
```

---

```zsh
git grep <some_string_to_be_found>
```
	-i <some_string_to_be_found>  # без учёта регистра
	<some_string_to_be_found> <хеш>  # поиск в конкретном коммите
	<some_string_to_be_found> $(git rev-list --all)  # поиск по всей истории

---

```zsh
git reflog
# данные о каждом изменении вершины ветки
git rev-list --all 
# <list_of_all_commits>
git rev-parse HEAD
git rev-parse HEAD^
git rev-parse origin/master 
# <хеш>
git rev-parse --abbrev-ref HEAD
# <branch_name>
```

### ВЕТВЛЕНИЕ

```zsh
git checkout <хеш>  # detaching HEAD, или переключение между коммитами
git checkout <branch_name>  # переключение между ветками, = git switch <branch_name>
```

```zsh
git checkout -b <branch_name>
git switch -c, --create <branch_name>
```
	Cоздание и переключение на новую ветку.

```zsh
git checkout HEAD^<num>  # перемещение в ширину
git checkout HEAD~<num>  # перемещение в глубину
```
	HEAD~1 = HEAD~
	HEAD = @

![[HEAD.png]]

```zsh
git checkout -b <branch_name> --track <name_remote_repo>/<branch_name>
```

Восстановление удалённой ветки
```zsh
git checkout <хэш последнего коммита в удаленной ветке>
git checkout -b <имя восстановленной удаленной ветки>
```

---

```zsh
git branch  # способ узнать место нахождения в дереве веток
```
	<branch_name>  # cпособ просто создать ветку
	-r  # список веток
	-d <branch_name>  # удалить ветку
	-D <branch_name>  # удалить ветку принудительно
    -m <branch_name>  # переместить ветку
    -M <branch_name>  # переместить ветку принудительно


```zsh
git branch -m <new_branch_name>  # if you are in an affected branch
git branch -m <deprecated_branch_name> <new_branch_name>  # if you are in other branch
git push origin :<deprecated_branch_name>
git push origin <new_branch_name>
```

---

```zsh
git merge <название ветки, которую мы хотим влить в текущую ветку>
```
	--abort  # прервать слияние
	--no-ff  # не использовать быструю перемотку

---

```zsh
git rebase <название ветки, куда производим rebase> <название ветки, которую rebase (можно не указывать, если нужно текущую)>
```
	Если ff, то наоборот.
	--continue  # если все конфликты разрешены
	--skip  # skip the current commit and continue with the rest of the sequence
	--abort  # чтобы отменить rebase

```zsh
git rebase -i HEAD~<num>/<хеш>  # интерактивный rebase
```

---

```zsh
git cherry-pick <коммит (его хеш), который мы хотим переместить на текущую ветку>
```
	--continue  # если все конфликты разрешены
	--skip  # skip the current commit and continue with the rest of the sequence
	--abort  # чтобы отменить копирование коммита
	--quit  # forget about the current operation in progress
	-e, --edit  # it will let you edit the commit message prior to committing

### ТЕГИ

```zsh
git tag <tag-name> (<commit> or HEAD)  # легковесный тег (указатель)
git tag -a <tag-name> <tag-name> (<commit> or HEAD) -m <text>  # аннотированный тег (полноценный объект)
```

```zsh
git tag -l, --list  # the flags are optional; список всех тегов, можно матчить: <tag-name*>
git show <tag-name>
```

```zsh
git checkout <tag-name>
```

```zsh
git push origin <tag-name>
git push origin --tags  # запушить все теги
git push origin --follow-tags  # запушить только аннотированные теги
```

```zsh
git tag -d, --delete <tag-name>
git push origin -d, --delete <tag-name>
git push origin :refs/tags/<tag-name>
```

```zsh
# восстановление удалённого тега
git fsck --unreachable | grep tag  # его поиск
git update-ref refs/tags/<tag-name>. # его восставление
```

```zsh
git describe <branch_name>, <commit>, HEAD (by default) ---> <tag>_<num_commits>_g<hash_current_commit>
```


```zsh
git tag новое-название-тега старое-название-тега
git tag -d старое-название-тега
git push origin :refs/tags/старое-название-тега
git push --tags
```

### EXIT

```zsh
exit
```
or
```
logout
```

### REFERENCE

```zsh
git help <command>
```

```zsh
git --help
```

```zsh
git --version
```

### ...

```zsh
git cat-file -p <hash_commit>
```

---

```zsh
git checkout <filename>  # если вы случайно удалили файл
git checkout $<хэш> <filename>  # если требуется восстановить файл из конкретной временной точки истории коммитов
```

### TODO

1. [git bisect](https://habr.com/ru/articles/591447/)
2. [pre-commit](https://www.youtube.com/watch?v=p2hAddDJ96E&t=1389s)
3. [git submodule](https://radioprog.ru/post/1395)