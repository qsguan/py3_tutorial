[toc]

**Python3 基础教程**

# Python2 与 Python3 版本区别

## `print`函数

`print`语句没有了，取而代之的是`print()`函数。

在 Python 2.6 与 Python 2.7 里面，以下三种形式是等价的：

```python
print "fish"
print ("fish") # 注意print后面有个空格
print("fish")  # print()不能带有任何其它参数
```

然而，Python 2.6 实际已经支持新的`print()`语法：

```python
from __future__ import print_function
print("fish", "panda", sep=', ')
```

## Unicode

Python2 有 ASCII str() 类型，unicode() 是单独的，不是 byte 类型。现在在 Python 3中，我们最终有了 Unicode (utf-8) 字符串，以及一个字节类：byte 和 bytearrays。由于 Python3.X 源码文件默认使用utf-8编码，这就使得以下代码是合法的:

```python
>>> 中国 = 'china'
>>>print(中国)
china
```

## 除法运算

在 Python 3 中对于整数之间的相除 (/)，结果也会是浮点数。<br>而对于 `//` 除法，这种除法叫做 floor除法，会对除法的结果自动进行一个 `floor` 操作，在 Python 2 和 Python 3 中是一致的。

## 异常与抛出

 Python 3 中使用 `as` 作为关键词。捕获异常的语法由 `except exc, var` 改为 `except exc as var`。使用语法`except (exc1, exc2) as var`可以同时捕获多种类别的异常。Python 2.6已经支持这两种语法。

此外:<br>

1. 在2.x时代，所有类型的对象都是可以被直接抛出的； 而在3.x时代，只有继承自BaseException的对象才可以被抛出。
2. 在2.x中`raise`语句使用逗号将抛出对象类型和参数分开；而3.x中取消了这种奇葩的写法，直接调用构造函数抛出对象即可。

## `range()` 取代`xrange()`

在 Python 3 中，`range()`是像`xrange()`那样实现的，以至于一个专门的`xrange()`函数不再存在。在 Python 3 中使用`xrange()`会抛出命名异常。

## 进制数表示

八进制数必须写成0o777，原来的形式0777不能用了；二进制必须写成0b111。

新增了一个`bin()`函数用于将一个整数转换成二进制字串。

## 不等运算符

Python 3 中去掉了`<>`, 只有`!=`一种写法。

## 去掉了`repr`表达式\`\`

Python 2 中反引号\`\`相当于`repr`函数的作用；<br>Python 3 中去掉了这种写法，只允许使用repr函数。

## 模块改名

| Old Module Name | New Module Name |
| --------------- | --------------- |
| _winreg         | winreg          |
| ConfigParser    | configparser    |
| copy_reg        | copyreg         |
| Queue           | queue           |
| SocketServer    | socketserver    |
| repr            | reprlib         |

`StringIO`模块现在被合并到新的`io`模组内。<br>`new`, `md5`, `gopherlib`等模块被删除。<br>
`httplib`, `BaseHTTPServer`, `CGIHTTPServer`, `SimpleHTTPServer`, `Cookie`, `cookielib`被合并到`http`包内。<br>
取消了`exec`语句，只剩下`exec()`函数。

## 数据类型

Python 3 去除了`long`类型，现在只有一种整型`int`，但它的行为就像 Python 2 版本的`long`；<br>新增了`bytes`类型，对应于 Python 2 版本中的八位串，定义一个`bytes`变量的方法如下:

```python
>>> b = b'china'
>>> type(b)
<type 'bytes'>
```

`str` 对象和 `bytes` 对象可以使用`.encode()` (str -> bytes) 或`.decode()` (bytes -> str) 方法相互转化。

```python
>>> s = b.decode()
>>> s
'china'
>>> b1 = s.encode()
>>> b1
b'china'
```

`dict`类型的的`.keys()`、`.items`和`.values()`方法返回迭代器，而之前的`iterkeys()`等函数都被废弃。同时去掉的还有`dict.has_key()`，用`in`替代它吧 。

## 打开文件

原：

```python
file( ..... )
或
open(.....)
```

改为只能用：

```python
open(.....)
```

Python 3 中`input()`替代了原`raw_input()`，其接收任意性输入，将所有输入默认为字符串处理，并返回字符串类型。

## `map`、`filter`和`reduce`

这三个函数号称是函数式编程的代表。<br>在 Python 2 中，它们都是内置函数 (built-in function)。<br>在 Python 3 中，它们从函数变成了类；其次它们的返回结果也从当初的列表变成了一个可迭代的对象，可以使用`next()`函数来进行手工迭代。

# Python 解释器

## 环境变量

| Variable      | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| PYTHONPATH    | Python搜索路径，默认import的模块都会从PYTHONPATH中寻找       |
| PYTHONSTARTUP | Python启动后，先执行PYTHONSTARTUP环境变量指定的文件中的代码  |
| PYTHONCASEOK  | 加入PYTHONCASEOK的环境变量，会使Python导入模块时不区分大小写 |
| PYTHONHOME    | 另一种模块搜索路径，通常内嵌于PYTHONSTARTUP或PYTHONPATH目录中，使得两个模块库更容易切换 |

## 脚本式编程

在Linux/Unix系统中，你可以在脚本顶部添加以下命令让Python脚本可以像SHELL脚本一样可直接执行：

```python
#! /usr/bin/env python3
```

然后修改脚本权限，使其有执行权限：

```bash
$ chmod +x hello.py
```

执行以下命令：

```bash
./hello.py
```

即可直接运行脚本。

# Python3 基础语法

## 编码

默认情况下， Python 3 源码文件以 UTF-8 编码， 所有字符串都是 unicode 字符串， 即:

```
#_*_ coding:utf-8 _*_
```

当然你也可以为源码文件指定不同的编码：

```
# -*- coding: cp-1252 -*-
```

上述定义允许在源文件中使用 Windows-1252 字符集中的字符编码， 对应适合语言为保加利亚语、白罗斯语、马其顿语、俄语、塞尔维亚语。

## 标识符

```
第一个字符必须是字母表中字母(a-z,A-Z)或下划线(_)。
标识符的其他的部分由字母、数字和下划线组成。
标识符对大小写敏感。
```

## 保留字

保留字即关键字， 我们不能把它们用作任何标识符名称。<br>Python 的标准库提供了一个 keyword 模块， 可以输出当前版本的所有关键字：

```python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

## 注释

单行注释：以 \# 开头；<br>多行注释：多个 \#，或者 ''' ，或者 """。

```python
#!/usr/bin/env python3
# 第一个注释
# 第二个注释
'''
第三注释
第四注释
'''
"""
第五注释
第六注释
"""
print ("Hello, Python!")
```

以下实例我们可以输出函数的注释：

```python
def a():
'''这是文档字符串'''
pass
print(a.__doc__)
```

输出结果为：

```
这是文档字符串
```

## 行与缩进

### 缩进

Python最具特色的就是使用缩进来表示代码块，不需要使用大括号 {} 。<br>
缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。

### 空行

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。<br>类和函数入口之间也用一行空行分隔，以突出函数入口的开始。<br>空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

## 多行语句

Python 通常是一行写完一条语句。

### 一条语句跨多行

但如果语句很长，我们可以使用反斜杠(\\)来实现多行语句，例如：

```python
total = item_one + \
        item_two + \
        item_three
```

在 []、{}、或 () 中的多行语句，不需要使用反斜杠(\\)，例如：

```python
total = ['item_one', 'item_two', 'item_three',
         'item_four', 'item_five']
```

### 一行写多条语句

Python可以在同一行中使用多条语句，语句之间使用分号(;)分割，例如：

```python
#!/usr/bin/env python3
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

使用脚本执行以上代码，输出结果为：

```
runoob
```

使用交互式命令行执行，输出结果为：

```
>>> import sys; x = 'runoob'; sys.stdout.write(x + '\n')
runoob
7
```

此处的 7 表示字符数。

### 多个语句构成代码组

缩进相同的一组语句构成一个代码块，我们称之代码组。<br>
像 `if`、`while`、`def`和`class`这样的复合语句，首行以关键字开始，以冒号(:)结束，该行之后的一行或多行代码构成代码组。我们将首行及后面的代码组称为一个子句(clause)。

如下实例：

```python
if expression1 :
	suite1
elif expression2 :
	suite2
else :
	suite3
```

## 数字与字符串

### 数字 (Number)

Python中数字有四种类型：**整数 (int)**、**布尔型 (bool)**、**浮点数 (float)** 和**复数 (complex)**。

| Number Type      | Description/Example                                          |
| ---------------- | ------------------------------------------------------------ |
| `int` (整数)     | 如: 1。只有一种整数类型`int`，表示为长整型，没有python2中的`long` |
| `bool` (布尔型)  | 如：True。                                                   |
| `float` (浮点数) | 如：1.23、3.1E-2。                                           |
| `complex` (复数) | 如：1 + 2j、1.1 - 2.2j。                                     |

### 字符串 (String)

Python中单引号和双引号使用完全相同。<br>使用三引号 ( ''' 或 """ )可以指定一个多行字符串。<br>反斜杠(\\)可以用来转义，使用 r 可以让反斜杠不发生转义。<br>字符串可以用 \+ 运算符连接在一起，用 \* 运算符重复。<br>Python中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。<br>Python中没有单独的字符类型，一个字符就是长度为1的字符串。<br>Python中字符串不可以发生改变。

字符串的截取语法格式如下：

```python
字符串变量[头下标:尾下标:步长]
```

## 输入与输出

### `input` 输入

Python3 仅保留了`input()`函数，其接收任意任性输入，将所有输入默认为字符串处理，并返回字符串类型。

执行下面的程序在按回车键后就会等待用户输入：

```python
#!/usr/bin/env python3
input("\n\n按下 enter 键后退出。")
```

以上代码中 ，`'\n\n'`在结果输出前会输出两个新的空行。<br>一旦用户按下 <kbd>Enter</kbd> 键时，程序将退出。

### `print` 输出

`print` 默认输出是换行的 (即`end='\n'`)，若要实现不换行需在变量末尾加上 `end=' '`，实例：

```python
#!/usr/bin/env python3
x="a"
y="b"
# 换行输出
print(x)
print(y)
print('---------')
# 不换行输出
print(x, end=' ')
print(y, end=' ')
print()
```

以上实例执行结果为：

```
a b
---------
a b
```

通过命令 `help(print)` 我们知道这个方法里第二个为缺省参数 `sep=' '`。这里表示我们使用空格作为分隔符。

```python
>>> help(print)
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
```

所以在打印 `dict` 类的使用, 可以这样写 (使用冒号作为分隔符):

```python
>>> def getPairs(dict):
...	for k,v in dict.items() :
...		print(k,v,sep=':')
...
```

测试代码：

```python
>>> getPairs({x:x**3 for x in (1,2,3,4)})
1:1
2:8
3:27
4:64
```

## `import` 与 `from ... import`

在 Python 用 `import` 或者 `from ... import` 来导入相应的模块。

| Description                                                  |
| ------------------------------------------------------------ |
| 将整个模块(somemodule)导入： `import somemodule`             |
| 从某个模块中导入某个函数： `from somemodule import somefunction` |
| 从某个模块中导入多个函数：` from somemodule import firstfunc, secondfunc, thirdfunc` |
| 将某个模块中的全部函数导入： `from somemodule import *`      |

## `help()` 函数

调用 Python 的 help() 函数可以打印输出一个函数的文档字符串，按下`:q`即退出说明文档。

```python
# 如下实例，查看 max 内置函数的参数列表和规范的文档
>>> help(max)
Help on built-in function max in module builtins:

max(...)
    max(iterable, *[, default=obj, key=func]) -> value
    max(arg1, arg2, *args, *[, key=func]) -> value
    
    With a single iterable argument, return its biggest item. The
    default keyword-only argument specifies an object to return if
    the provided iterable is empty.
    With two or more arguments, return the largest argument.
(END)
```

若仅想得到文档字符串：

```python
>>> print(max.__doc__) # 注意，doc的前后分别是两个下划线
max(iterable, *[, default=obj, key=func]) -> value
max(arg1, arg2, *args, *[, key=func]) -> value

With a single iterable argument, return its biggest item. The
default keyword-only argument specifies an object to return if
the provided iterable is empty.
With two or more arguments, return the largest argument.
```

在 print 打印的时候双引号与单引号都可以当做定界符使用，且可以嵌套。<br>被嵌套的会被解释成为标点符号，反之亦然。

## 命令行参数

很多程序可以执行一些操作来查看一些基本信息，Python可以使用-h参数查看各参数帮助信息：

```bash
$ python -h
usage: python [option] ... [-c cmd | -m mod | file | -] [arg] ...
Options and arguments (and corresponding environment variables):
-b     : issue warnings about str(bytes_instance), str(bytearray_instance)
         and comparing bytes/bytearray with str. (-bb: issue errors)
-B     : don't write .pyc files on import; also PYTHONDONTWRITEBYTECODE=x
-c cmd : program passed in as string (terminates option list)
-d     : debug output from parser; also PYTHONDEBUG=x
-E     : ignore PYTHON* environment variables (such as PYTHONPATH)
-h     : print this help message and exit (also --help)
-i     : inspect interactively after running script; forces a prompt even
         if stdin does not appear to be a terminal; also PYTHONINSPECT=x
-I     : isolate Python from the user's environment (implies -E and -s)
-m mod : run library module as a script (terminates option list)
-O     : remove assert and __debug__-dependent statements; add .opt-1 before
         .pyc extension; also PYTHONOPTIMIZE=x
-OO    : do -O changes and also discard docstrings; add .opt-2 before
         .pyc extension
-q     : don't print version and copyright messages on interactive startup
-s     : don't add user site directory to sys.path; also PYTHONNOUSERSITE
-S     : don't imply 'import site' on initialization
-u     : force the stdout and stderr streams to be unbuffered;
         this option has no effect on stdin; also PYTHONUNBUFFERED=x
-v     : verbose (trace import statements); also PYTHONVERBOSE=x
         can be supplied multiple times to increase verbosity
-V     : print the Python version number and exit (also --version)
         when given twice, print more information about the build
-W arg : warning control; arg is action:message:category:module:lineno
         also PYTHONWARNINGS=arg
-x     : skip first line of source, allowing use of non-Unix forms of #!cmd
-X opt : set implementation-specific option
--check-hash-based-pycs always|default|never:
    control how Python invalidates hash-based .pyc files
file   : program read from script file
-      : program read from stdin (default; interactive mode if a tty)
arg ...: arguments passed to program in sys.argv[1:]

Other environment variables:
PYTHONSTARTUP: file executed on interactive startup (no default)
PYTHONPATH   : ':'-separated list of directories prefixed to the
               default module search path.  The result is sys.path.
PYTHONHOME   : alternate <prefix> directory (or <prefix>:<exec_prefix>).
               The default module search path uses <prefix>/lib/pythonX.X.
PYTHONCASEOK : ignore case in 'import' statements (Windows).
PYTHONIOENCODING: Encoding[:errors] used for stdin/stdout/stderr.
PYTHONFAULTHANDLER: dump the Python traceback on fatal errors.
PYTHONHASHSEED: if this variable is set to 'random', a random value is used
   to seed the hashes of str and bytes objects.  It can also be set to an
   integer in the range [0,4294967295] to get hash values with a
   predictable seed.
PYTHONMALLOC: set the Python memory allocators and/or install debug hooks
   on Python memory allocators. Use PYTHONMALLOC=debug to install debug
   hooks.
PYTHONCOERCECLOCALE: if this variable is set to 0, it disables the locale
   coercion behavior. Use PYTHONCOERCECLOCALE=warn to request display of
   locale coercion and locale compatibility warnings on stderr.
PYTHONBREAKPOINT: if this variable is set to 0, it disables the default
   debugger. It can be set to the callable of your debugger of choice.
PYTHONDEVMODE: enable the development mode.
PYTHONPYCACHEPREFIX: root directory for bytecode cache (pyc) files.
```

## `if __name__ == '__main__'：`的作用

一个 python 文件通常有两种使用方法:<br>第一是作为脚本直接执行；<br>第二是`import `到其他的 python 脚本中被调用 (模块重用) 执行。

`if __name__ == '__main__': `的作用就是控制这两种情况执行代码的过程。<br>在 `if __name__ == '__main__': `下的的代码只有在第一种情况下 (即文件作为脚本直接执行) 才会被执行，<br>而`import`到其他脚本中是不会被执行的。

# Python 3 基本数据类型





















































# Python 获取命令行参数

## 利用`sys.argv`

Python 中可以用 `sys` 的 `sys.argv` 来获取命令行参数：

```
sys.argv 是命令行参数列表。
len(sys.argv) 是命令行参数个数

注：sys.argv[0] 表示代码本身文件路径，所以参数从1开始
```

### 实例1

创建test.py 文件，代码如下：

```python
#!/usr/bin/env python3
import sys
print ('参数个数为:', len(sys.argv), '个参数。')
print ('参数列表:', str(sys.argv))
```

执行以上代码，输出结果为：

```bash
$ python3 test.py arg1 arg2 arg3
参数个数为: 4 个参数。
参数列表: ['test.py', 'arg1', 'arg2', 'arg3']
```

### 实例2

创建sample.py 文件，代码如下：

```python
#!/usr/bin/env python  
#_*_ coding:utf-8 _*_  
import sys   

HELP = ''' 
This program prints files to the standard output.  
Any number of files can be specified.  
Options include:  
  --version : Prints the version number  
  --help    : Display this help
'''  

def readfile(filename): #定义readfile函数，从文件中读出文件内容   
  '''''''''Print a file to the standard output.'''  
  f = file(filename)   
  while True:   
    line = f.readline()   
    if len(line) == 0:   
      break  
    print line, # notice comma 分别输出每行内容   
  f.close() 

# Script starts from here  
print sys.argv  

if len(sys.argv) < 2:   
  print 'No action specified.'  
  sys.exit()   

if sys.argv[1].startswith('--'):   
  option = sys.argv[1][2:]   
  # fetch sys.argv[1] but without the first two characters   
  if option == 'version': #当命令行参数为-- version，显示版本号   
    print 'Version 1.2'  
  elif option == 'help': #当命令行参数为--help时，显示相关帮助内容   
    print HELP
  else:   
    print 'Unknown option.'  
  sys.exit()   
else:   
  for filename in sys.argv[1:]: #当参数为文件名时，传入readfile，读出其内容   
    readfile(filename)
```

在与sample.py同一目录下，新建1个记事本文件test.txt, 其内容为: `hello python!`。<br>验证sample.py，如下：

```bash
C:\Users\91135\Desktop>python sample.py
 ['sample.py']
No action specified.

C:\Users\91135\Desktop>python sample.py --help
['sample.py', '--help']
This program prints files to the standard output.
 Any number of files can be specified.
 Options include:
  --version : Prints the version number
  --help    : Display this help
 
C:\Users\91135\Desktop>python sample.py --version
 ['sample.py', '--version']
Version 1.2

C:\Users\91135\Desktop>python sample.py --ok
 ['sample.py', '--ok']
Unknown option.

C:\Users\91135\Desktop>python sample.py test.txt
 ['sample.py', 'test.txt']
hello python!
```

## 利用`getopt`模块

`getopt`模块是专门处理命令行参数的模块，用于获取命令行选项和参数，即`sys.argv`。命令行选项使得程序的参数更加灵活。支持短选项模式 (-) 和长选项模式 (--)。该模块提供了两个方法及一个异常处理来解析命令行参数。

### `getopt.getopt` 方法

`getopt.getopt `方法用于解析命令行参数列表，语法格式如下：

```python
getopt.getopt(args, options[, long_options])
```

方法参数说明:

```
args        : 要解析的命令行参数列表。
options     : 以字符串的格式定义，后的冒号(:)表示该选项必须有附加参数，不带冒号表示该选项不附加参数。
long_options: 以列表的格式定义，后的等号(=)表示如果设置该选项则必须有附加参数，否则就不附加参数。

该方法返回值由两个元素组成: 
第一个是 (option, value) 元组的列表。 
第二个是参数列表，包含那些没有'-'或'--'的参数。
```

### `getopt.gnu_getopt` 方法

另外一个方法是 ’getopt.gnu_getopt‘，这里不多做介绍。

### 异常处理`except getopt.GetoptError`

在没有找到参数列表，或选项的需要的参数为空时会触发该异常。<br>异常的参数是一个字符串，表示错误的原因。<br>属性 `msg` 和 `opt` 为相关选项的错误信息。

### 实例

假定我们创建这样一个脚本，可以通过命令行向脚本文件传递两个文件名，同时我们通过另外一个选项查看脚本的使用。脚本使用方法如下：

```bash
usage: test.py -i <inputfile> -o <outputfile>
```

创建test.py 文件，代码如下所示：

```python
#!/usr/bin/env python3
import sys, getopt

def main(argv):
    inputfile = ''
    outputfile = ''
    try:
        opts, args = getopt.getopt(argv,"hi:o:",["ifile=","ofile="])
    except getopt.GetoptError:
        print ('test.py -i <inputfile> -o <outputfile>')
        sys.exit(2)
    for opt, arg in opts:
        if opt == '-h':
        	print ('test.py -i <inputfile> -o <outputfile>')
            sys.exit()
        elif opt in ("-i", "--ifile"):
            inputfile = arg
        elif opt in ("-o", "--ofile"):
            outputfile = arg
        print ('Input File is: ', inputfile)
        print ('Output File is: ', outputfile)

if __name__ == "__main__":
	main(sys.argv[1:])
```

```
Note:
	"hi:o:"             -> '-'型参数有: -h, -i(必须带附加参数), -o(必须带附加参数)
	["ifile=","ofile="] -> '--'型参数有: --ifile(必须带附加参数), --ofile(必须带附加参数)
```

执行以上代码，输出结果为：

```bash
$ python3 test.py -h
usage: test.py -i <inputfile> -o <outputfile>
$ python3 test.py -i inputfile -o outputfile
Input File is: inputfile
Output File is: outputfile
```

