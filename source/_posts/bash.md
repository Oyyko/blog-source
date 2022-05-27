---
title: BASH脚本
date: 2022-05-27
tag: 
- bash
category: bash
mathjax: true
---
BASH 条件判断
<!--more-->
## test 和 [

内置命令 test 根据表达式expr 求值的结果返回 0（真）或 1（假）。也可以使用方括号：test expr 和 [ expr ] 是等价的。 可以用 $? 检查返回值；可以使用 && 和 || 操作返回值；也可以用本技巧后面介绍的各种条件结构测试返回值。

[ian@pinguino ~]$ test 3 -gt 4 && echo True || echo false

false

[ian@pinguino ~]$ [ "abc" != "def" ];echo $?

0

[ian@pinguino ~]$ test -d "$HOME" ;echo $?

0

在清单 1 的第一个示例中，-gt 操作符对两个字符值之间执行算术比较。在第二个示例中，用 [ ] 的形式比较两个字符串不相等。在最后一个示例中，测试 HOME 变量的值，用单目操作符 -d 检查它是不是目录。


可以用 -eq、 -ne、-lt、 -le、 -gt 或 -ge 比较算术值，它们分别表示等于、不等于、小于、小于等于、大于、大于等于。


可以分别用操作符 =、 !=、< 和 > 比较字符串是否相等、不相等或者第一个字符串的排序在第二个字符串的前面或后面。单目操作符 -z 测试 null 字符串，如果字符串非空 -n 返回 True（或者根本没有操作符）。


说明：shell 也用 < 和 > 操作符进行重定向，所以必须用 \< 或 \> 加以转义。清单 2 显示了字符串测试的更多示例。检查它们是否如您预期的一样。

## 一些常见的文件测试 操作符 特征

-d 目录

-e 存在（也可以用 -a）

-f 普通文件

-h 符号连接（也可以用 -L）

-p 命名管道

-r 可读

-s 非空

-S 套接字

-w 可写

-N 从上次读取之后已经做过修改

## 除了上面的单目测试，还可以使用表 2 所示的双目操作符比较两个文件：

表 2. 测试一对文件 操作符 为 True 的情况

-nt 测试 file1 是否比 file2 更新。修改日期将用于这次和下次比较。

-ot 测试 file1 是否比 file2 旧。

-ef 测试 file1 是不是 file2 的硬链接。


## 清单 5. 分配和测试算术表达式

[ian@pinguino ~]$ let x=2 y=2**3 z=y*3;echo $? $x $y $z

0 2 8 24

[ian@pinguino ~]$ (( w=(y/x) + ( (~ ++x) & 0x0f ) )); echo $? $x $y $w

0 3 8 16

[ian@pinguino ~]$ (( w=(y/x) + ( (~ ++x) & 0x0f ) )); echo $? $x $y $w

0 4 8 13

清单 6. 使用 [[ 复合命令

[ian@pinguino ~]$ [[ ( -d "$HOME" ) && ( -w "$HOME" ) ]] &&

> echo "home is a writable directory"

home is a writable directory


在使用 = 或 != 操作符时，复合命令 [[ 还能在字符串上进行模式匹配。匹配的方式就像清单 7 所示的通配符匹配。


清单 7. 用 [[ 进行通配符测试

[ian@pinguino ~]$ [[ "abc def .d,x--" == a[abc]*\ ?d* ]]; echo $?

0

[ian@pinguino ~]$ [[ "abc def c" == a[abc]*\ ?d* ]]; echo $?

1

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* ]]; echo $?

1


甚至还可以在 [[ 复合命令内执行算术测试，但是千万要小心。除非在 (( 复合命令内，否则 < 和 > 操作符会把操作数当成字符串比较并在当前排序序列中测试它们的顺序。清单 8 用一些示例演示了这一点。


清单 8. 用 [[ 包含算术测试

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* || (( 3 > 2 )) ]]; echo $?

0

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* || 3 -gt 2 ]]; echo $?

0

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* || 3 > 2 ]]; echo $?

0

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* || a > 2 ]]; echo $?

0

[ian@pinguino ~]$ [[ "abc def d,x" == a[abc]*\ ?d* || a -gt 2 ]]; echo $?

-bash: a: unbound variable

条件测试


虽然使用以上的测试和 &&、 || 控制操作符能实现许多编程，但 bash 还包含了更熟悉的 “if, then, else” 和 case 结构。学习完这些之后，将学习循环结构，这样您的工具箱将真正得到扩展。


If、then、else 语句


bash 的 if 命令是个复合命令，它测试一个测试或命令（$?）的返回值，并根据返回值为 True（0）或 False（不为 0）进行分支。虽然上面的测试只返回 0 或 1 值，但命令可能返回其他值。请参阅 LPI exam 102 prep: Shells, scripting, programming, and compiling 教程学习这方面的更多内容。


Bash 中的 if 命令有一个 then 子句，子句中包含测试或命令返回 0 时要执行的命令列表，可以有一个或多个可选的 elif 子句，每个子句可执行附加的测试和一个 then 子句，子句中又带有相关的命令列表，最后是可选的 else 子句及命令列表，在前面的测试或 elif 子句中的所有测试都不为真的时候执行，最后使用 fi 标记表示该结构结束。


使用迄今为止学到的东西，现在能够构建简单的计算器来计算算术表达式，如清单 9 所示：


清单 9. 用 if、then、else 计算表达式

[ian@pinguino ~]$ function mycalc ()

> {

> local x

> if [ $# -lt 1 ]; then

> echo "This function evaluates arithmetic for you if you give it some"

> elif (( $* )); then

> let x="$*"

> echo "$* = $x"

> else

> echo "$* = 0 or is not an arithmetic expression"

> fi

> }

[ian@pinguino ~]$ mycalc 3 + 4

3 + 4 = 7

[ian@pinguino ~]$ mycalc 3 + 4**3

3 + 4**3 = 67

[ian@pinguino ~]$ mycalc 3 + (4**3 /2)

-bash: syntax error near unexpected token `('

[ian@pinguino ~]$ mycalc 3 + "(4**3 /2)"

3 + (4**3 /2) = 35

[ian@pinguino ~]$ mycalc xyz

xyz = 0 or is not an arithmetic expression

[ian@pinguino ~]$ mycalc xyz + 3 + "(4**3 /2)" + abc

xyz + 3 + (4**3 /2) + abc = 35


这个计算器利用 local 语句将 x 声明为局部变量，只能在 mycalc 函数的范围内使用。let 函数具有几个可用的选项，可以执行与它密切关联的 declare 函数。请参考 bash 手册或使用 help let 获得更多信息。


如清单 9 所示，需要确保在表达式使用 shell 元字符 —— 例如(、)、*、> 和 < 时 —— 正确地对表达式转义。无论如何，现在有了一个非常方便的小计算器，可以像 shell 那样进行算术计算。


在清单 9 中可能注意到 else 子句和最后的两个示例。可以看到，把 xyz 传递给 mycalc 并没有错误，但计算结果为 0。这个函数还不够灵巧，不能区分最后使用的示例中的字符值，所以不能警告用户。可以使用字符串模式匹配测试（例如

[[ ! ("$*" == *[a-zA-Z]* ]]

， 或使用适合自己范围的形式）消除包含字母表字符的表达式，但是这会妨碍在输入中使用 16 进制标记，因为使用 16 进制标记时可能要用 0x0f 表示 15。实际上，shell 允许的基数最高为 64（使用 base#value 标记），所以可以在输入中加入 _ 和 @ 合法地使用任何字母表字符。8 进制和 16 进制使用常用的标记方式，开头为 0 表示八进制，开头为 0x 或 0X 表示 16 进制。清单 10 显示了一些示例。


清单 10. 用不同的基数进行计算

[ian@pinguino ~]$ mycalc 015

015 = 13

[ian@pinguino ~]$ mycalc 0xff

0xff = 255

[ian@pinguino ~]$ mycalc 29#37

29#37 = 94

[ian@pinguino ~]$ mycalc 64#1az

64#1az = 4771

[ian@pinguino ~]$ mycalc 64#1azA

64#1azA = 305380

[ian@pinguino ~]$ mycalc 64#1azA_@

64#1azA_@ = 1250840574

[ian@pinguino ~]$ mycalc 64#1az*64**3 + 64#A_@

64#1az*64**3 + 64#A_@ = 1250840574


对输入进行的额外处理超出了本技巧的范围，所以请小心使用这个计算器。


elif 语句非常方便。它允许简化缩进，从而有助于脚本编写。在清单 11 中可能会对 type 命令在 mycalc 函数中的输出感到惊讶。


清单 11. Type mycalc

[ian@pinguino ~]$ type mycalc

mycalc is a function

mycalc ()

{

local x;

if [ $# -lt 1 ]; then

echo "This function evaluates arithmetic for you if you give it some";

else

if (( $* )); then

let x="$*";

echo "$* = $x";

else

echo "$* = 0 or is not an arithmetic expression";

fi;

fi

}


当 然，也可以只用 $(( 表达式 )) 和 echo 命令进行 shell 算术运算，如清单 12 所示。这样就不必学习关于函数或测试的任何内容，但是请注意 shell 不会解释元字符，例如 *，因此元字符不能在 (( 表达式 )) 或 [[ 表达式 ]] 中那样正常发挥作用。


清单 12. 在 shell 中用 echo 和 $(( )) 直接进行计算

[ian@pinguino ~]$ echo $((3 + (4**3 /2)))
