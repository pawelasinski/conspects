- venv создает изолированные среды только для разработки на Python, а conda может создавать изолированные среды для любого поддерживаемого ЯП.
- pip устанавливает только пакеты Python из PyPI, с помощью conda можно установить пакеты (написанные на любом языке) из репозиториев, таких как Anaconda Repository и Anaconda Cloud; пакеты из PyPI, используя pip в активной среде Conda.
- conda — одновременно менеджер пакетов и среды и не зависит от языка.

```zsh
conda search <package_name>
conda search --override-channels -c,--channel conda-forge <package_name>
conda search -f,--full-name <package_name>
```

```zsh
conda install <package_name>[=<version>]
conda install -c conda-forge <package_name>[=<version>]
```
Если вы не укажете имя среды, пакет будет установлен в текущей активной среде.

- Каналы — это места хранилищ, где Conda ищет пакеты.
- Каналы существуют в иерархическом порядке. Канал с наивысшим приоритетом является первым, который проверяет Conda в поисках пакета, который вы просили. Вы можете изменить этот порядок, а также добавить к нему каналы (и установить их приоритет).
- Рекомендуется добавлять канал в список каналов как элемент с самым низким приоритетом. Таким образом, вы можете включить «специальные» пакеты, которые не являются частью тех, которые установлены по умолчанию (каналы Continuum).

```zsh
conda update conda
conda update --all
conda update -n,--name <env_name> conda
```

```zsh
conda create -n <env_name> <package_name> 
conda create --no-default-packages --no-deps -n <env_name> <package_name>=<version>
```

```zsh
conda activate <env_name>
```

```zsh
conda deactivate
```

```zsh
conda env list
conda info -e,--envs
```

```python
import sys
sys.prefix; sys.exec_prefix  # указывают на базовый каталог виртуальной среды
sys.base_prefix; sys.base_exec_prefix  # указывают на базовую установку Python, ту, из которой была создана виртуальная среда
```

```zsh
conda info
```

```zsh
conda list
```
```python
import sys
sys.modules.keys()
```
```zsh
conda list -e > requirements.txt
```

```zsh
conda install -n <env_name> requirements.txt
```

```zsh
conda config --show
             --set
             --append
             --get  # получить все ключи и значения из файла .condarc
             --get channels  # получить значение ключевых каналов из файла .condarc
             --add channels <channel_name>  # добавить новое значение в каналы, чтобы conda искала пакеты в этом месте
```

```zsh
conda uninstall <package_name>
conda remove -n <env_name> --all
```

```zsh
conda clean -t  # удаление кеша - архивов .tar.bz2, которые могут занимать много места и не нужны
```

```zsh
conda env export > environment.yml
```

```zsh
conda env create -f environment.yml
```

```zsh
jupyter kernelspec list
```
```zsh
conda install ipykernel
conda create -n <env_name> python=<version>
conda activate <env_name>
python -m ipykernel install -n <kernel_name>
conda deactivate
```
```zsh
jupyter notebook
```

```zsh
conda -V | --version
```

```zsh
python -V
```
```python
import sys; sys.version
```

