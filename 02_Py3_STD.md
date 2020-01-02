[toc]

# 14 Python 3 输入和输出

## 14.1 `input()`输入

Python 3 仅保留了`input()`函数，它可接收任意任性输入，将所有输入默认为字符串处理，并返回字符串类型。

执行下面的程序在按回车键后就会等待用户输入：

```python
#!/usr/bin/env python3
input("\n\n按下 enter 键后退出。")
```

以上代码中 ，`'\n\n'`在结果输出前会输出两个新的空行。一旦用户按下 <kbd>Enter</kbd> 键时，程序将退出。

## 14.2 `print()`输出

Python 中有两种**输出值**的方式: **表达式语句**和**`print()`函数**。第三种方式是使用**文件对象**的**`write()`方法**，标准输出文件可以用`sys.stdout`引用。

* 若希望输出的形式更加多样，可以使用`str.format()`函数来格式化输出值 (见**Section 4.5.3**)。

* 若希望将输出的值转成字符串，可以使用`repr()`或`str()`函数来实现。
  * **`str()`**: 函数返回一个用户易读的表达形式。 
  * **`repr()`**: 产生一个解释器易读的表达形式。

```python
>>> s = 'Hello, Runoob'
>>> str(s)
'Hello, Runoob'
>>> repr(s)
"'Hello, Runoob'"
>>> str(1/7)
'0.14285714285714285'
>>> x = 10 * 3.25; y = 200 * 200
>>> s = 'x 的值为： ' + repr(x) + ',  y 的值为：' + repr(y) + '.'
>>> print(s)
x 的值为： 32.5,  y 的值为：40000.
>>> hello = 'hello, runoob\n'; hellos = repr(hello); print(hellos)  
'hello, runoob\n'                       # repr()函数可以转义字符串中的特殊字符  
>>> repr((x, y, ('Google', 'Runoob')))  # repr()的参数可以是 Python 的任何对象
"(32.5, 40000, ('Google', 'Runoob'))"
```

两种方式输出一个平方与立方的表：

```python
>>> for x in range(1, 11):
...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ') 
...     # 注意前一行 'end' 的使用
...     print(repr(x*x*x).rjust(4))  # 使用rjust()方法将字符串靠右,并在左边填充空格
...
>>> for x in range(1, 11):
...     print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))
...
```

## 14.3 读和写文件

### 14.3.1 打开文件`open()`函数

利用`open()`函数将会返回一个`file`对象，基本语法格式如下: 

```python
open(filename, mode='r', encoding=None)
```

- `filename`：包含了你要访问的文件名称的字符串值。
- `mode`：决定了打开文件的模式：只读，写入，追加等。该参数是非强制的，默认文件访问模式为只读。
- `encoding`：一般使用`utf-8`

不同模式打开文件的完全列表：

| Mode | Description                                                  |
| ---- | ------------------------------------------------------------ |
| t    | (默认) 文本模式                                              |
| b    | 二进制模式                                                   |
| +    | 打开一个文件进行更新(可读可写)。                             |
| r    | (默认) 以只读方式打开文件。文件的指针将会放在文件的开头。    |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，则创建新文件。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，则创建新文件进行写入。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，则创建新文件。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，则创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，则创建新文件。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，则创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，则创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，则创建新文件用于读写。 |

下图很好的总结了这几种模式：

<left>
    <img src="./images/0015.png" alt="0015.png" style="zoom: 50%;">
</left>

|    Mode    |  r   |  r+  |  w   |  w+  |  a   |  a+  |
| :--------: | :--: | :--: | :--: | :--: | :--: | :--: |
|     读     |  +   |  +   |      |  +   |      |  +   |
|     写     |      |  +   |  +   |  +   |  +   |  +   |
|    创建    |      |      |  +   |  +   |  +   |  +   |
|    覆盖    |      |      |  +   |  +   |      |      |
| 指针在开始 |  +   |  +   |  +   |  +   |      |      |
| 指针在结尾 |      |      |      |      |  +   |  +   |

### 14.3.2 文件对象的方法

本节中剩下的例子假设已经创建了一个称为`f`的文件对象。

1. **`f.read([size])`**函数：读取一定数目的数据，然后作为字符串或字节对象返回。`size`是一个可选的数字类型的参数，当`size`被忽略了或者为负时该文件的所有内容都将被读取并且返回。

   ```python
   #!/usr/bin/env python3
   f = open("/tmp/foo.txt", "r")  # 打开一个文件
   str = f.read()  # 读取整个文件
   print(str) # 输出读取内容
   f.close()  # 关闭打开的文件
   ```

2. **`f.readline([size])`**函数：从文件中读取单独的一行，换行符为`'\n'`。若`f.readline()`返回一个空字符串, 则说明已经读取到最后一行。

3. **`f.readlines([size])`**函数：将返回该文件中包含的所有行(直到结束符`EOF`)，若碰到结束符`EOF`则返回空字符串。若设置可选参数`size`，则读取指定长度的字节, 并且将这些字节按行分割。

   若文件"runoob.txt"的内容如下：

   ```
   1:www.runoob.com
   2:www.runoob.com
   3:www.runoob.com
   4:www.runoob.com
   5:www.runoob.com
   ```

   循环读取文件的内容：

   ```python
   #!/usr/bin/env python3
   fo = open("runoob.txt", "r")  # 打开文件
   print("文件名为: ", fo.name)  # 输出文件名
   
   for line in fo.readlines():             # 依次读取每行  
       line = line.strip()                 # 去掉每行头尾空白  
       print ("读取的数据为: %s" % (line))  # 输出每行内容
    
   fo.close()  # 关闭文件
   ```

4. 另一种方式是**迭代一个文件对象**然后**读取每行**：

   ```python
   #!/usr/bin/env python3
   f = open("/tmp/foo.txt", "r")  # 打开一个文件
   for line in f:  # 迭代一个文件对象然后读取每行
       print(line, end='')
   f.close()  # 关闭打开的文件
   ```

5. **`f.write(string)`**函数：将`string`写入文件中，返回写入的字符数。若想写入非字符串，则需先进行转换。

6. **`f.writelines(list)`**函数：向文件写入一个序列字符串列表，若要换行则需自行加入每行的换行符。

   ```python
   #!/usr/bin/env python3
   fo = open("test.txt", "w")  # 打开文件
   print("Filename is: ", fo.name)  # 输出文件名
   seq = ["菜鸟教程 1\n", "菜鸟教程 2"]  # 字符串列表 (含换行符)
   fo.writelines( seq )  # 写入文件
   fo.close()  # 关闭文件
   ```

7. **`f.tell()`**函数：返回文件对象当前所处的位置，即从文件开头开始算起的字节数。

8. **`f.seek(offset, from_what)`**函数：可以使用该函数改变文件当前的位置。

   `from_what`的值，若是 0 表示开头 (默认)；1 表示当前位置； 2 表示文件的结尾，例如：

   - `seek(x,0)`： 从文件首行首字符开始往后移动`x`个字符
   - `seek(x,1)`： 表示从当前位置往后移动`x`个字符
   - `seek(-x,2)`：表示从文件的结尾往前移动`x`个字符

   ```python
   >>> f = open('/tmp/foo.txt', 'rb+')
   >>> f.write(b'0123456789abcdef')
   16
   >>> f.seek(5)     # 移动到文件的第六个字节
   5
   >>> f.read(1)
   b'5'
   >>> f.seek(-3, 2) # 移动到文件的倒数第三字节
   13
   >>> f.read(1)
   b'd'
   ```

9. **`f.truncate(size)`**函数：用于从文件的首行首字节开始截断，截断文件为`size`个字节，无`size`则表示从文件开头开始截断到当前位置；截断之后后面的所有字节被删除。实例：

   ```python
   #!/usr/bin/env python3
   fo = open("runoob.txt", "r+", encoding="utf-8")
   print ("文件名: ", fo.name)
   
   fo.seek(36)
   fo.truncate()  # 从第36个字节以后的内容全部删除了
   
   fo.seek(0,0)
   line = fo.readlines()
   print("读取行: %s" % (line))
   
   fo.truncate(10)  # 截取10个字节
   fo.seek(0,0)
   str = fo.read()
   print("读取数据: %s" % (str))
   
   fo.close() # 关闭文件
   ```

   以上实例输出结果为：

   ```python
   文件名:  runoob.txt
   读取行: ['1:www.runoob.com\n', '2:www.runoob.com\n', '3:']
   读取数据: 1:www.runo
   ```

10. **`f.close()`**函数：在文本文件中 (那些打开文件的模式下没有`b`的)，只会相对于文件起始位置进行定位。当处理完一个文件后，调用`f.close()`来关闭文件并释放系统资源，若尝试再调用该文件则会抛出异常。

11. 当处理一个文件对象时，**使用`with`关键字**是非常好的方式。在结束后，它会帮你正确的关闭文件。而且写起来也比`try - finally`语句块要简短：

    ```python
    >>> with open('/tmp/foo.txt', 'r') as f:
    ...     read_data = f.read()
    >>> f.closed
    True
    ```

实例1：(检索指定路径下后缀是".py"的所有文件)

```python
#!/usr/bin/env python3
import os
import os.path

ls = []  # save full paths of all '.py' files found

def get_appoint_file(path, ls):  # path: full path specified
    file_list = os.listdir(path) # list all file names in the specified path
    try:
        for tmp in file_list:  # iterate for all file names
            path_tmp = os.path.join(path, tmp)  # assemble full path of one file 
            if os.path.isdir(path_tmp):  # if the file is still a folder
                get_appoint_file(path_tmp, ls)  # invoke the subroutine again
            elif path_tmp[path_tmp.rfind('.') + 1:].upper() == 'PY':  # if suffix is '.py'
                ls.append(path_tmp)  # found one python file
    except PermissionError:  # if there is no permission to visit the file
        pass  # continue to search for next file

def main():
    while True:
        path = input('Please type an absolute location:').strip()  # remove whitespaces
        if os.path.isdir(path):
            break

    get_appoint_file(path, ls)
    print(ls)       # print all '.py' file full paths
    print(len(ls))  # print the number of found '.py' files

main()
```

实例2：(搜索并替换一指定文本文件中的字符或单词)

```python
#!/usr/bin/env python3

def file_replace(file_name, rep_word, new_word):
    f_read = open(file_name)

    content = []
    count = 0
    for eachline in f_read:
        if rep_word in eachline:
            count = count + eachline.count(rep_word)
            eachline = eachline.replace(rep_word, new_word)
        content.append(eachline)

    message = '\nFile %s contains %s [%s]' \
              '\nDo you what to replace all [%s] by [%s]？' \
              '\n[YES/NO]：'
    decide = input(message % (file_name, count, rep_word, rep_word, new_word))

    if decide in ['YES', 'Yes', 'yes']:
        f_write = open(file_name, 'w')
        f_write.writelines(content)
        f_write.close()

    f_read.close()

if __name__ == "__main__":
    file_name = input('Please Type a Text File：')
    rep_word = input('Please Type a Character/Word to be Replaced：')
    new_word = input('Please Type a New Character/Word：')
    file_replace(file_name, rep_word, new_word)
```

### 14.3.3 使用`pickle`模块

Python 中的`pickle`模块实现了基本的数据序列化和反序列化操作。通过`pickle`模块的序列化能够将程序中运行的对象信息保存到文件中并永久存储。通过`pickle`模块的反序列化，能够从文件中创建上一次程序保存的对象。基本接口：

```python
pickle.dump(obj, file, [,protocol=None])  # 将对象的pickled表示写入数据文件
pickle.dumps(obj [,protocol=None])        # 将对象的pickled表示作为bytes对象返回

obj     : 要封装的对象
file    : 必须以二进制可写模式即"wb"打开
protocol: 使用的协议：0,1,2,3,4,5,-1 (若为负数,则选用最高)

>>> pickle.HIGHEST_PROTOCOL # 可用的最高协议的版本 (Python3.8中为5)
5
>>> pickle.DEFAULT_PROTOCOL # 默认的协议版本 (Python3.8中默认为4)
4
>>> pickle.format_version  # 当前格式版本
'4.0'
>>> pickle.compatible_formats  # 兼容的格式版本
['1.0', '1.1', '1.2', '1.3', '2.0', '3.0', '4.0', '5.0']
>>> 
>>> with open(filename, 'wb') as file:
...   pickle.dump(data, file)
...
>>> p_str = pickle.dumps(data); print(p_str)
```

有了`pickle`这个对象，就能对`file`以读取的形式打开:

```python
x = pickle.load(file)  # 从数据文件中读取转换被封装的对象并返回为Python的数据结构
x = pickle.loads(obj)  # 从字节对象中读取转换被封装的对象并返回为Python的数据结构

file: 必须以二进制可读模式即"rb"打开

>>> with open(filename, 'r') as file:
...   data = pickle.load(file)
...
>>> mes = pickle.loads(p_str); print(mes)
```

使用`pickle`模块可能出现三种异常：

1. `PickleError`：封装和拆封时出现的异常类，继承自`Exception`

2. `PicklingError`: 遇到不可封装的对象时出现的异常，继承自`PickleError`

3. `UnPicklingError`: 拆封对象过程中出现的异常，继承自`PickleError`

实例1：

```python
#!/usr/bin/env python3
import pickle

# 原始数据对象1 (字典)
data1 = {'a': [1, 2.0, 3, 4+6j],
         'b': ('string', u'Unicode string'),
         'c': None}
# 原始数据对象2 (递归引用)
selfref_list = [1, 2, 3]
selfref_list.append(selfref_list)

# 使用pickle模块将数据对象保存到文件
output = open('data.pkl', 'wb')

# Pickle dictionary using protocol 0.
pickle.dump(data1, output)

# Pickle the list using the highest protocol available.
pickle.dump(selfref_list, output, -1)

# 关闭文件
output.close()
```

实例2：

```python
#!/usr/bin/env python3
import pprint, pickle

# 使用pickle模块从文件中重构Python对象
pkl_file = open('data.pkl', 'rb')

data1 = pickle.load(pkl_file)
pprint.pprint(data1)

data2 = pickle.load(pkl_file)
pprint.pprint(data2)

pkl_file.close()
```



# 15 Python 3 OS 文件/目录方法

Python 中的**`os`**模块提供了非常丰富的方法用来处理文件和目录。常用的方法如下表所示：

1. [os.access(path, mode)](https://www.runoob.com/python3/python3-os-access.html): 用于检验权限模式
   - **path** -- 要用来检测是否有访问权限的路径。 
   - **mode** -- 默认为`os.F_OK`
     - **os.F_OK:** 作为access()的mode参数，测试path是否存在。
     - **os.R_OK:** 包含在access()的mode参数中 ， 测试path是否可读。 
     - **os.W_OK** 包含在access()的mode参数中 ， 测试path是否可写。
     - **os.X_OK** 包含在access()的mode参数中 ，测试path是否可执行。
2. [os.chdir(path)](https://www.runoob.com/python3/python3-os-chdir.html): 用于改变当前工作目录到指定的路径。如果允许访问返回`True`, 否则返回`False`。
3. [os.chflags(path, flags)](https://www.runoob.com/python3/python3-os-chflags.html): 设置路径的标记为数字标记，多个标记可使用`OR`组合起来。只支持在Unix下使用。 
   - **flags** -- 可以是以下值： 
     - **stat.UF_NODUMP:**  非转储文件
     - **stat.UF_IMMUTABLE:**  文件是只读的
     - **stat.UF_APPEND:**  文件只能追加内容 
     - **stat.UF_NOUNLINK:**  文件不可删除
     - **stat.UF_OPAQUE:**  目录不透明，需要通过联合堆栈查看
     - **stat.SF_ARCHIVED:**  可存档文件(超级用户可设) 
     - **stat.SF_IMMUTABLE:**  文件是只读的(超级用户可设)
     - **stat.SF_APPEND:**  文件只能追加内容(超级用户可设) 
     - **stat.SF_NOUNLINK:**  文件不可删除(超级用户可设) 
     - **stat.SF_SNAPSHOT:**  快照文件(超级用户可设)
4. [os.chmod(path, mode)](https://www.runoob.com/python3/python3-os-chmod.html): 用于更改文件或目录的权限。
5. 
6. 
7. 
8. 
9. 
10. 





| No.  | Method & Description                                         |      |
| :--: | ------------------------------------------------------------ | ---- |
|  4   | [os.chmod(path, mode)](https://www.runoob.com/python3/python3-os-chmod.html) 更改权限 |      |
|  5   | [os.chown(path, uid, gid)](https://www.runoob.com/python3/python3-os-chown.html) 更改文件所有者 |      |
|  6   | [os.chroot(path)](https://www.runoob.com/python3/python3-os-chroot.html) 改变当前进程的根目录 |      |
|  7   | [os.close(fd)](https://www.runoob.com/python3/python3-os-close.html) 关闭文件描述符 fd |      |
|  8   | [os.closerange(fd_low, fd_high)](https://www.runoob.com/python3/python3-os-closerange.html) 关闭所有文件描述符，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略 |      |
|  9   | [os.dup(fd)](https://www.runoob.com/python3/python3-os-dup.html) 复制文件描述符 fd |      |
|  10  | [os.dup2(fd, fd2)](https://www.runoob.com/python3/python3-os-dup2.html) 将一个文件描述符 fd 复制到另一个 fd2 |      |
|  11  | [os.fchdir(fd)](https://www.runoob.com/python3/python3-os-fchdir.html) 通过文件描述符改变当前工作目录 |      |
|  12  | [os.fchmod(fd, mode)](https://www.runoob.com/python3/python3-os-fchmod.html) 改变一个文件的访问权限，该文件由参数fd指定，参数mode是Unix下的文件访问权限。 |      |
|  13  | [os.fchown(fd, uid, gid)](https://www.runoob.com/python3/python3-os-fchown.html) 修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符fd指定。 |      |
|  14  | [os.fdatasync(fd)](https://www.runoob.com/python3/python3-os-fdatasync.html) 强制将文件写入磁盘，该文件由文件描述符fd指定，但是不强制更新文件的状态信息。 |      |
|  15  | [os.fdopen(fd[, mode[, bufsize\]])](https://www.runoob.com/python3/python3-os-fdopen.html) 通过文件描述符 fd 创建一个文件对象，并返回这个文件对象 |      |
|  16  | [os.fpathconf(fd, name)](https://www.runoob.com/python3/python3-os-fpathconf.html) 返回一个打开的文件的系统配置信息。name为检索的系统配置的值，它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1, Unix 95, Unix 98, 和其它）。 |      |
|  17  | [os.fstat(fd)](https://www.runoob.com/python3/python3-os-fstat.html) 返回文件描述符fd的状态，像stat()。 |      |
|  18  | [os.fstatvfs(fd)](https://www.runoob.com/python3/python3-os-fstatvfs.html) 返回包含文件描述符fd的文件的文件系统的信息，Python 3.3 相等于 statvfs()。 |      |
|  19  | [os.fsync(fd)](https://www.runoob.com/python3/python3-os-fsync.html) 强制将文件描述符为fd的文件写入硬盘。 |      |
|  20  | [os.ftruncate(fd, length)](https://www.runoob.com/python3/python3-os-ftruncate.html) 裁剪文件描述符fd对应的文件, 所以它最大不能超过文件大小。 |      |
|  21  | [os.getcwd()](https://www.runoob.com/python3/python3-os-getcwd.html) 返回当前工作目录 |      |
|  22  | [os.getcwdu()](https://www.runoob.com/python3/python3-os-getcwdu.html) 返回一个当前工作目录的Unicode对象 |      |
|  23  | [os.isatty(fd)](https://www.runoob.com/python3/python3-os-isatty.html) 如果文件描述符fd是打开的，同时与tty(-like)设备相连，则返回true, 否则False。 |      |
|  24  | [os.lchflags(path, flags)](https://www.runoob.com/python3/python3-os-lchflags.html) 设置路径的标记为数字标记，类似 chflags()，但是没有软链接 |      |
|  25  | [os.lchmod(path, mode)](https://www.runoob.com/python3/python3-os-lchmod.html) 修改连接文件权限 |      |
|  26  | [os.lchown(path, uid, gid)](https://www.runoob.com/python3/python3-os-lchown.html) 更改文件所有者，类似 chown，但是不追踪链接。 |      |
|  27  | [os.link(src, dst)](https://www.runoob.com/python3/python3-os-link.html) 创建硬链接，名为参数 dst，指向参数 src |      |
|  28  | [os.listdir(path)](https://www.runoob.com/python3/python3-os-listdir.html) 返回path指定的文件夹包含的文件或文件夹的名字的列表。 |      |
|  29  | [os.lseek(fd, pos, how)](https://www.runoob.com/python3/python3-os-lseek.html) 设置文件描述符 fd当前位置为pos, how方式修改: SEEK_SET 或者 0 设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算; os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效 |      |
|  30  | [os.lstat(path)](https://www.runoob.com/python3/python3-os-lstat.html) 像stat(),但是没有软链接 |      |
|  31  | [os.major(device)](https://www.runoob.com/python3/python3-os-major.html) 从原始的设备号中提取设备major号码 (使用stat中的st_dev或者st_rdev field)。 |      |
|  32  | [os.makedev(major, minor)](https://www.runoob.com/python3/python3-os-makedev.html) 以major和minor设备号组成一个原始设备号 |      |
|  33  | [os.makedirs(path[, mode\])](https://www.runoob.com/python3/python3-os-makedirs.html) 递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹。 |      |
|  34  | [os.minor(device)](https://www.runoob.com/python3/python3-os-minor.html) 从原始的设备号中提取设备minor号码 (使用stat中的st_dev或者st_rdev field )。 |      |
|  35  | [os.mkdir(path[, mode\])](https://www.runoob.com/python3/python3-os-mkdir.html) 以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。 |      |
|  36  | [os.mkfifo(path[, mode\])](https://www.runoob.com/python3/python3-os-mkfifo.html) 创建命名管道，mode 为数字，默认为 0666 (八进制) |      |
|  37  | [os.mknod(filename[, mode=0600, device\])](https://www.runoob.com/python3/python3-os-mknod.html) 创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。 |      |
|  38  | [os.open(file, flags[, mode\])](https://www.runoob.com/python3/python3-os-open.html) 打开一个文件，并且设置需要的打开选项，mode参数是可选的 |      |
|  39  | [os.openpty()](https://www.runoob.com/python3/python3-os-openpty.html) 打开一个新的伪终端对。返回 pty 和 tty的文件描述符。 |      |
|  40  | [os.pathconf(path, name)](https://www.runoob.com/python3/python3-os-pathconf.html)  返回相关文件的系统配置信息。 |      |
|  41  | [os.pipe()](https://www.runoob.com/python3/python3-os-pipe.html) 创建一个管道. 返回一对文件描述符(r, w) 分别为读和写 |      |
|  42  | [os.popen(command[, mode[, bufsize\]])](https://www.runoob.com/python3/python3-os-popen.html) 从一个 command 打开一个管道 |      |
|  43  | [os.read(fd, n)](https://www.runoob.com/python3/python3-os-read.html) 从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。 |      |
|  44  | [os.readlink(path)](https://www.runoob.com/python3/python3-os-readlink.html) 返回软链接所指向的文件 |      |
|  45  | [os.remove(path)](https://www.runoob.com/python3/python3-os-remove.html) 删除路径为path的文件。如果path 是一个文件夹，将抛出OSError; 查看下面的rmdir()删除一个 directory。 |      |
|  46  | [os.removedirs(path)](https://www.runoob.com/python3/python3-os-removedirs.html) 递归删除目录。 |      |
|  47  | [os.rename(src, dst)](https://www.runoob.com/python3/python3-os-rename.html) 重命名文件或目录，从 src 到 dst |      |
|  48  | [os.renames(old, new)](https://www.runoob.com/python3/python3-os-renames.html) 递归地对目录进行更名，也可以对文件进行更名。 |      |
|  49  | [os.rmdir(path)](https://www.runoob.com/python3/python3-os-rmdir.html) 删除path指定的空目录，如果目录非空，则抛出一个OSError异常。 |      |
|  50  | [os.stat(path)](https://www.runoob.com/python3/python3-os-stat.html) 获取path指定的路径的信息，功能等同于C API中的stat()系统调用。 |      |
|  51  | [os.stat_float_times([newvalue\])](https://www.runoob.com/python3/python3-os-stat_float_times.html) 决定stat_result是否以float对象显示时间戳 |      |
|  52  | [os.statvfs(path)](https://www.runoob.com/python3/python3-os-statvfs.html) 获取指定路径的文件系统统计信息 |      |
|  53  | [os.symlink(src, dst)](https://www.runoob.com/python3/python3-os-symlink.html) 创建一个软链接 |      |
|  54  | [os.tcgetpgrp(fd)](https://www.runoob.com/python3/python3-os-tcgetpgrp.html) 返回与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组 |      |
|  55  | [os.tcsetpgrp(fd, pg)](https://www.runoob.com/python3/python3-os-tcsetpgrp.html) 设置与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组为pg。 |      |
|  56  | os.tempnam([dir[, prefix]]) **Python3 中已删除。**返回唯一的路径名用于创建临时文件。 |      |
|  57  | os.tmpfile() **Python3 中已删除。**返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。 |      |
|  58  | os.tmpnam() **Python3 中已删除。**为创建一个临时文件返回一个唯一的路径 |      |
|  59  | [os.ttyname(fd)](https://www.runoob.com/python3/python3-os-ttyname.html) 返回一个字符串，它表示与文件描述符fd 关联的终端设备。如果fd 没有与终端设备关联，则引发一个异常。 |      |
|  60  | [os.unlink(path)](https://www.runoob.com/python3/python3-os-unlink.html) 删除文件路径 |      |
|  61  | [os.utime(path, times)](https://www.runoob.com/python3/python3-os-utime.html) 返回指定的path文件的访问和修改的时间。 |      |
|  62  | [os.walk(top[, topdown=True[, onerror=None[, followlinks=False\]]])](https://www.runoob.com/python3/python3-os-walk.html) 输出在文件夹中的文件名通过在树中游走，向上或者向下。 |      |
|  63  | [os.write(fd, str)](https://www.runoob.com/python3/python3-os-write.html) 写入字符串到文件描述符 fd中. 返回实际写入的字符串长度 |      |
|  64  | [os.path 模块](https://www.runoob.com/python3/python3-os-path.html) 获取文件的属性信息。 |      |



# 18 Python3 标准库





# 20 Python 获取命令行参数

## 20.1 利用`sys.argv`

Python 中可以用 `sys` 的 `sys.argv` 来获取命令行参数：

```
sys.argv 是命令行参数列表。
len(sys.argv) 是命令行参数个数

注：sys.argv[0] 表示代码本身文件路径，所以参数从1开始
```

### 20.1.1 实例1

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

### 20.1.2 实例2

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

## 20.2 利用`getopt`模块

`getopt`模块是专门处理命令行参数的模块，用于获取命令行选项和参数，即`sys.argv`。命令行选项使得程序的参数更加灵活。支持短选项模式 (-) 和长选项模式 (--)。该模块提供了两个方法及一个异常处理来解析命令行参数。

### 20.2.1 `getopt.getopt` 方法

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

### 20.2.2 `getopt.gnu_getopt` 方法

另外一个方法是 ’getopt.gnu_getopt‘，这里不多做介绍。

### 20.2.3 异常处理`except getopt.GetoptError`

在没有找到参数列表，或选项的需要的参数为空时会触发该异常。<br>异常的参数是一个字符串，表示错误的原因。<br>属性 `msg` 和 `opt` 为相关选项的错误信息。

### 20.2.4 实例

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

