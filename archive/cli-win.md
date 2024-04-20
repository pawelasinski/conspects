### Создать новый файл

```powershell
type nul > <file_name>
```

```powershell
cd. > <file_name>
```

```powershell
cd > <file_name>
```

```powershell
del <file_name>
```

### Создать/удалить директорию

```powershell
mkdir <dir_name>
```

```powershell
rmdir <dir_name>
```

### Открыть/переименовать/скопировать/переместить файл

```powershell
type <file_name>
```
```powershell
notepad <file_name>
```

```powershell
rename <file_name> <new_file_name>
```

```powershell
copy <file_name> </path>
```

```powershell
move <file_name> </path>
```


### Перенаправление потоков

```powershell
echo <some_text> > <file_name>
```

```powershell
echo <some_text> >> <file_name>
```


### Переменные окружения

```powershell
setx <ENV_VAR> "<value>"
```
