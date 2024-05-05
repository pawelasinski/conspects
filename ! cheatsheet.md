Способ достучаться до приватных атрибутов или методов:
object._ClassName__attribute
object._ClassName__function

Явный импорт позволяет достучаться до защищённых атрибутов или методов.
from lib import _function 

_ в Jupyter Notebook:
1+2+3 -> 6
_ -> 6
None
_ -> 6

print(f'{"yes" if 10 > 5 else "no"}') -> yes

(func1 if y == 1 else func2)(arg1, arg2)
(class1 if y == 1 else class2)(arg1, arg2)




ClassName = type('ClassName', (), dict(attribute=value))


Магические методы в JN:
%%time <script>
%timeit <mini-script>
%run <link>
%load <link>
%store <var> / %store -r <var>
%pycat <link>
%who <type>
%%writefile <filename>


Способ вызвать команды CLI в JN:
!cat (UNIX-подобные), !more (Windows)

Способ вызвать справку в JN:
?list, ??list
np.con*?

Способ взывать справку в консоли Python:
help(list.insert)

Cпособ вызвать список методов у класса (интроспекция):
dir(object)

int32 - здесь 32 - это 32 бита = 8 байт

np.sort(ndarray) - inplace
ndarray.sort() - non-inplace

np.delete(ndarray, [indexes]), np.append(ndarray, [values]), np.insert(ndarray, index, [values]) - non-inplace

b = a[:] - this method don't work in NumPy. You should use "copy()" method.

A = [[0] * N] * M - DON'T USE IT!

SELECT first_name || ' ' || last_name AS full_name
FROM customers

MySQL JS> \sql
MySQL SQL> \connect root@localhost:3306
<password's requirements>
MySQL localhost:3306 ssl SQL>

mysql> show databases;
mysql> create database имя_базы_данных;
mysql> use имя_базы_данных;
mysql> create table имя_таблицы ();
...

\d имя_базы_данных

CREATE DATABASE [IF NOT EXISTS] имя_базы_данных;
DROP DATABASE [IF EXISTS] имя_базы_данных;

TRUNCATE TABLE имя_таблицы;
DROP TABLE имя_таблицы;

...
WHERE выражение [NOT] REGEXP регулярное_выражение

В PostgreSQL:
...
LIMIT 2, 3 (взять 3-и записи начиная со 2-й)

Оператор SELECT, который обычно начинается с SELECT *, а не со списка выражений или имен столбцов.
Чтобы повысить производительность, вы можете заменить SELECT * на SELECT 1, поскольку результат столбца подзапроса не имеет значения (имеют значение только возвращаемые строки).

? Операторы SQL, которые используют условие EXISTS в PostgreSQL, очень неэффективны, поскольку подзапрос перезапускается для КАЖДОЙ строки в таблице внешнего запроса.
WHERE [NOT] EXISTS (subquery);
? Но поскольку при применении EXISTS не происходит выборка строк, то его использование более оптимально и эффективно, чем использование оператора IN.

"table1 JOIN table2 ON true"  == "table1 CROSS JOIN table2"

SELECT COUNT(id) cnt FROM my_table;
SELECT COUNT(*) cnt FROM my_table;
SELECT COUNT(1) cnt FROM my_table;
SELECT COUNT(ALL 1) cnt FROM my_table;
SELECT COUNT(DISTINCT 1) cnt FROM my_table;

SELECT * FROM table WHERE id = id;


В Python <3.0:
 object
   |
basestring
  / \
str unicode


Включение в __slots__ элемент __dict__


newline - необязательно, режим перевода строк. Варианты: None, '\n', '\r' и '\r\n'. Следует использовать только для текстовых файлов.

file.truncate([size]) - метод усекает размер файла, где size - целое число int, количество символов или байт.
                        Если указан необязательный аргумент size, файл усекается до этого (максимально) размера.
                        По умолчанию size равен текущей позиции указателя чтения/записи файла.
                        Этот метод не будет работать, если файл открыт в режиме только для чтения.
file.flush()          - метод очищает внутренний буфер
file.tell()           - метод, позволяющий узнать текущую позицию считывания/записи в файле

file.seek(offset[, whence]) - метод устанавливающий текущую позицию считывания/записи в файле, где offset задаёт отступ, а whence - точку, от которой данный отступ считается:
 - 0 означает, что нужно сместить указатель на offset относительно начала файла (by default)
 - 1 означает, что нужно сместить указатель на offset относительно текущей позиции.
 - 2 означает, что нужно сместить указатель на offset относительно конца файла.


1. Установка виджетов:
    $ pip install ipywidgets
    $ conda install -c conda ipywidgets
2. Разрешаем их использование в JN:
    $ jupyter nbextension enable --py --sys-prefix widgetsnbextension
3. Restart Kernal



<dict> = collections.defaultdict(lambda: 1)  # Creates a dict with default value 1.


Type and class are synonymous
<type> = type(<el>)                          # Or: <el>.__class__
<bool> = isinstance(<el>, <type>)            # Or: issubclass(type(<el>), <type>)


{<el>:<10}   # '<el>      '
{<el>:^10}   # '   <el>   '
{<el>:>10}   # '      <el>'
{<el>:.<10}  # '<el>......'
{<el>:0}     # '<el>'

{'abcde':10}    # 'abcde     '
{'abcde':10.3}  # 'abc       '
{'abcde':.3}    # 'abc'
{'abcde'!r:10}  # "'abcde'   "

{123456:10}    # '    123456'
{123456:10,}   # '   123,456'
{123456:10_}   # '   123_456'
{123456:+10}   # '   +123456'
{123456:=+10}  # '+   123456'
{123456: }     # ' 123456'
{-123456: }    # '-123456'

{1.23456:10.3}   # '      1.23'
{1.23456:0.3f}   # '1.234'
{1.23456:10.3f}  # '     1.235'
{1.23456:10.3e}  # ' 1.235e+00'
{1.23456:10.3%}  # '  123.456%'

{10:0}   # 10
{10:02}  # 10
{10:03}  # 010
{10:04}  # 0010

When both rounding up and rounding down are possible, the one that returns result with even last digit is chosen.
That makes '{6.5:.0f}' a '6' and '{7.5:.0f}' an '8'.

{90:c}  # 'Z'
{90:b}  # '1011010'
{90:X}  # '5A'

{n:,.2f} -> 2,000,000,000.00

{s}
{s!s}
{s!r}

format(1.15, '.51f') -> '1.1499999999999999111821580299874767661094665527343750'
Некоторые десятичные числа в двоичной системе - периодические дроби, которые не влезают в память.

int('ABC', 16) -> 2748


loads(dumps(x)) != x  # JSON


https://nuancesprog.ru/p/11111/


os.chdir(os.path.join(os.path.expanduser('~'), 'Desktop'))


print(True + 100) -> 101  # int


names = ['Pavel']
another_names = ['Pavel']
print(names is another_names) -> False
print(names == another_names) -> True

with open("hello.txt", "a") as hello_file:
    print("Hello, world!", file=hello_file)


1.00.as_integer_ratio() -> (1, 1)
1.00.is_integer() -> True
1.00 == 1 -> True


import importlib
importlib.reload(<some_module_which_you_wanna_reload>)


https://metanit.com/python/tutorial/6.3.php


https://metanit.com/python/tutorial/6.4.php


fp = open("foo.txt", "r")
fp.closed -> False
fp.mode -> 'r'
fp.name -> '~/foo.txt'
fp.close()


format(14, '#b'), format(14, 'b') -> ('0b1110', '1110')
f'{14:#b}', f'{14:b}' -> ('0b1110', '1110')


flatter_list = list(itertools.chain.from_iterable(<list>))


Counter(a=1) == Counter(a=1, b=0) -> True
cnt.most_common()[:-n-1:-1]  # n наименее распространенных элементов
+Counter(a=2, b=-4) -> Counter({'a': 2})
-Counter(a=2, b=-4) -> Counter({'b': 4})


with open(filepath_1, 'r') as infile, 
        open(filepath_2, 'a') as outfile:
    pass


"".splitlines() -> []
"One line\n".splitlines() -> ['One line']

"".split("\n") -> [""]
"Two lines\n".split("\n") -> ["Two lines", ""]


