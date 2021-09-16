## 基本语法

### SET命令



### 运算符

#### 算数运算符

|运算符| 描述 |示例|
| ----| ---- | ---- |
| `+` | 两个操作数相加 | `1 + 2`的结果为：`3` |
| `-`  | 从第一个减去第二个操作数 | `2 - 1`的结果为：`1` |
| `*`|  两个操作数的乘法| `2 * 3`的结果为：`6`|
|`/`| 分母除以分子 |`3 / 2`的结果为：`1.5`|
|`%`| 模运算符，整数/浮点除法后的余数| `3 % 2`的结果为：`1`|

**示例：**

```bat
@echo off
SET /A a=1
SET /A b=2
SET /A c=%a% + %b%
echo %c%
```

#### 关系运算符

| 运算符 | 描述 | 示例 |
| ------ | ---- | ---- |
|EQU|测试两个对象之间的相等性|`2 EQU 2`的结果为：真|
|NEQ|测试两个对象之间的不相等性|`3 NEQ 2`的结果为：真|
|LSS|检查左对象是否小于右操作数|`2 LSS 3` 的结果为：真|
|LEQ|检查左对象是否小于或等于右操作数|`2 LEQ 3`的结果为：真|
|GTR|检查左对象是否大于右操作数|`3 GTR 2` 的结果为：真|
|GEQ|检查左对象是否大于或等于右操作数|`3 GEQ 2` 的结果为：真|

**示例**

```bat
@echo off 
SET /A a=5 
SET /A b=10 
if %a% EQU %b% echo A is equal to than B 
if %a% NEQ %b% echo A is not equal to than B 
if %a% LSS %b% echo A is less than B 
if %a% LEQ %b% echo A is less than or equal B
if %a% GTR %b% echo A is greater than B 
if %a% GEQ %b% echo A is greater than or equal to B

```

结果

```
A is not equal to than B
A is less than B
A is less than or equal B
```



#### 逻辑运算符

|运算符|描述|
|---------|-------|
|NOT|逻辑“非”|



**示例（NOT）**

```bat
@echo off
SET /A a=5
IF NOT %a%==6 echo "A is not equal to 6"
```

结果

```
"A is not equal to 6""
```

> 有的教程上说还有`AND`、`OR`运算运算符，但在Win10系统中测试，表示`运算符不存在`。



**示例**

```bat
@echo off 
set /a var= 1 "&" 1
echo %var%
set /a var= 1 "|" 1
echo %var%
set /a var= 1 "^" 1
echo %var%
```

结果

```
>run.bat
1
1
0
```



### for循环

#### for循环的基本形态

Windows bat脚本的for语句基本形态如下：

- 在CMD窗口中：

  ```bat
	for %I in (command1) do command2
  ```

- 在脚本中

  ```bat
  for %%I in (command1) do command2
  ```

**示例：**

```bat
@echo off
for %%I in (1 5 15 20) do (
  echo file-%%I.png
)
```

结果：

```cmd
>run.bat
file-1.png
file-5.png
file-15.png
file-20.png
```



#### for循环中的变量延迟

设置本地为延迟环境变量扩展。

**变量识别过程**

在cmd执行命令前会对脚本进行预处理，其中有一个过程是**变量识别过程**，在这个过程中，如果有两个%括起来的如%value%类似这样的变量，就会对其进行识别，并且查找这个变量对应的值，再而将值替换掉这个变量，这个替换值的过程，就叫做变量扩展。

```bat
@echo off 
setlocal enabledelayedexpansion
set /A b=0
for /l %%i in (1,5,277) do (
  copy "c:\src\file-%%i.png" c:\dst\file-!b!.png
  set /A b+=1
)
```

> 延迟的变量需要用`!!`引用。

> **SETLOCAL**
>
> 开始批处理文件中环境改动的本地化操作。在执行 SETLOCAL 之后所做的环境改动只限于批处理文件。要还原原先的设置，必须执行 ENDLOCAL。 达到批处理文件结尾时，对于该批处理文件的每个尚未执行的 SETLOCAL 命令，都会有一个隐含的 ENDLOCAL 被执行。

## 参考文献

[1]. [易百教程：《批处理教程》](https://www.yiibai.com/batch_script)

[2]. [脚本之家：cmd SETLOCAL使用介绍](https://www.jb51.net/article/36043.htm)

