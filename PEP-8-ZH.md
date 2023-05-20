
本文是 [PEP 8](https://www.python.org/dev/peps/pep-0008/) 的中文翻译，基于 MarkDown 语法。

## §1 简介

本文档为主 Python 发行版中构成标准库的 Python 代码提供编码规范。请参见伴随的信息 PEP，其中描述了 [Python C实现中C代码的样式指南](https://peps.python.org/pep-0007 "PEP 7 – Style Guide for C Code")。

本文档和 [PEP 257](https://peps.python.org/pep-0257 "PEP 257 – Docstring Conventions")（文档字符串约定）改编自 Guido 的原始 Python 样式指南文章，其中还加入了 Barry 的样式指南[^2]的一些内容。

随着语言本身的变化，本样式指南将随着识别出额外约定并使过时的约定失效而不断演变。

许多项目都有自己的编码样式指南。如果出现任何冲突，对于该项目，应以特定于该项目的指南为优先。

## §2 何时忽略本指南

Guido 的一个关键洞见之一是，代码被阅读的次数比编写的次数多得多。本文档提供的指南旨在提高代码的可读性，并使其在 Python 代码的广泛范围内保持一致。正如 [PEP 20](https://peps.python.org/pep-0020 "PEP 20 – The Zen of Python") 所说，“可读性至关重要”。

一篇样式指南是关于一致性的约定。遵守该样式指南保持一致性很重要。遵守项目内的一致性更为重要。遵守一个模块或功能内的一致性是最重要的。

不过，也要知道何时可以不一致——有时样式指南的建议并不适用。当有疑虑时，请运用你自己的判断力。看看其他例子，并决定哪个看起来更好。随时向他人提问！

特别是：不要为了遵守本 PEP 而破坏向后兼容性！

其他一些忽略样式准则的充分理由：

1. 应用该指南会使代码难以阅读，即使是习惯于阅读遵循此 PEP 的代码的人。
2. 为了与周围的代码保持一致，这些代码也违反了这些规则（可能出于历史原因），尽管这也是清理别人犯下的错误的机会（按照真正的 XP 风格）。
3. 由于所讨论的代码早于准则的引入，因此没有其他理由修改该代码。
4. 当代码需要与不支持样式指南推荐功能的旧版 Python 保持兼容性时。

## §3 代码布局

### §3.1 缩进

每个缩进级别使用4个空格。

换行应该使用垂直对齐方式，使用 Python 的括号、方括号和大括号的隐式换行，或者使用悬挂缩进[^1]。在使用悬挂缩进时，应该考虑以下几点；第一行不应该有参数，并且需要使用进一步的缩进来清晰地区分为换行：

```python
# Correct:

# Aligned with opening delimiter.
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Add 4 spaces (an extra level of indentation) to distinguish arguments from the rest.
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

```python
# Wrong:

# Arguments on first line forbidden when not using vertical alignment.
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Further indentation required as indentation is not distinguishable.
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

对于使用悬挂缩进的连续行，4空格规则不一定需要遵守。比如：

```python
# Hanging indents *may* be indented to other than 4 spaces.
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```

当 `if`-语句的条件部分足够长，需要跨越多行编写时，值得注意的是，使用两个字符的关键字（即 `if`），再加上一个空格和一个左括号会为后续的多行条件语句创建一个自然的4个空格缩进。这可能会产生与嵌套在 `if`-语句内部的缩进代码块产生视觉冲突，后者也会自然缩进到4个空格。本 PEP 没有对如何（或是否）进一步从视觉上区分这样的条件行和 `if`-语句内部嵌套的代码块表达明确的立场。在这种情况下，可接受的选项包括但不限于：

```python
# No extra indentation.
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# Add a comment, which will provide some distinction in editors
# supporting syntax highlighting.
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# Add some extra indentation on the conditional continuation line.
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

（也请参见下面关于在二元操作符之前还是之后换行的讨论。）

多行构造的右括弧/方括号/花括号可以在列表最后一行的第一个非空白字符下对齐，如下所示：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```

或者它可以与开始多行结构的行的第一个字符对齐，例如：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```


### §3.2 使用 tab 还是空格？

空格是首选的缩进方式。

tab 应仅用于与已经用制表符缩进的代码保持一致。

Python 不允许混合使用 tab 和空格进行缩进。

### §3.3 一行代码最大的长度

行最多为79个字符。

对于没有太多结构限制的长文本块（例如文档字符串或注释），行长应该限制在72个字符以内。

限制所需的编辑器窗口宽度可以使得打开多个文件并排显示成为可能，并且当使用代码审查工具将两个版本呈现在相邻的列中时非常有效。

大多数工具中的默认换行方式会破坏代码的视觉结构，使其更难理解。这些限制被选择为避免在将窗口宽度设置为80的编辑器中换行，即使工具在换行时在最后一列放置一个标记符号。有些基于 Web 的工具可能根本不提供动态换行。

有些团队更喜欢更长的行长度。对于由能够就此问题达成协议的团队独家或主要维护的代码，可以将行长度限制增加到99个字符，但是注释和文档字符串仍应该在72个字符处换行。

Python 标准库比较保守，要求将行限制在79个字符以内（文档字符串/注释为72个字符）。

换行时的首选方式是使用 Python 中括号、方括号和大括号内的隐式换行来实现。可以通过将表达式放在括号中来将长行分为多行。与使用反斜杠进行换行相比，这种方法更加优先。

有时候仍然可能需要使用反斜杠。例如，在 Python 3.10 之前，长而多个 `with`-语句无法使用隐式换行，因此对于这种情况使用反斜杠是可以接受的：

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

（请参见前面有关多行 `if`-语句的讨论，以了解有关这些多行 `with`-语句缩进的更多想法。）

另一个需要注意缩进的情况是 `assert`-语句。

请确保适当缩进续行。

### §3.4 换行应该在二元运算符的前还是后？

几十年来，推荐的样式一直是在二元运算符后进行换行。但这种做法可能会影响可读性，因为运算符往往会散布到屏幕上的不同列，并且每个运算符都会移动到其操作数之前的上一行。在这种情况下，需要额外的工作量来判断哪些项目是相加的，哪些项目是相减的：

```python
# Wrong:
# operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)
```

为了解决此可读性问题，数学家和他们的出版商采用了相反的惯例。Donald Knuth 在他的 _Computers and Typesetting_ 系列中解释了传统的规则：“尽管段落内的公式总是在二元运算和关系符后换行，但显示公式总是在二元运算符之前换行”[^3]。

遵循这样的数学的惯例会使代码更具可读性：

```python
# Correct:
# easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

在 Python 代码中，可以在二元运算符之前或之后进行换行，只要在本地保持样式一致即可。对于新代码，建议采用 Knuth 的样式。

### §3.5 空行

用两个空行包围顶级函数和类定义。

类内的方法定义由单个空白行包围。

可以（适度地）使用额外的空行来分隔相关函数组。在一堆相关的单行函数之间可以省略空行（例如，一组虚拟实现）。

在函数中使用空白行以分隔逻辑部分。

Python 接受 ctrl-L （即^ L）换页字符作为空格；许多工具将这些字符视为页面分隔符，因此您可以使用它们来分隔您的文件中不同部分的页面。请注意，某些编辑器和基于 Web 的代码查看器可能不会将 ctrl-L 识别为换页，而是在其位置显示另一个标志符号。

### §3.6 源文件的编码

核心 Python 发行版中的代码应始终使用 UTF-8，并且不应具有编码声明。

在标准库中，非 UTF-8 编码应仅用于测试目的。请适度使用非 ASCII 字符，最好仅用于表示地点和人名。如果将非 ASCII 字符用作数据，请避免使用带噪声的 Unicode 字符（如z̯̯͡a̧͎̺l̡͓̫g̹̲o̡̼̘和字节顺序标记）。

Python 标准库中的所有标识符都必须使用仅 ASCII 标识符，并在可能的情况下应使用英文单词（在许多情况下，使用缩写和技术术语而不是英文）。

鼓励全球受众的开源项目采用类似的政策。

### §3.7 模块的导入

- import 通常一行一个：

    ```python
    # Correct:
    import os
    import sys
    ```

    ```python
    # Wrong:
    import sys, os
    ```

    不过，这样也是可以的：

    ```python
    # Correct:
    from subprocess import Popen, PIPE
    ```

- import 总是放在文件的顶部，紧随任何模块注释和文档字符串之后，同时在全局变量和常量之前。

    导入应按以下顺序分组：

    1. 标准库导入。
    2. 相关第三方库。
    3. 本地应用/库的某个具体模块/方法的导入。

    您应该在每组导入之间放置一个空白行。

- 推荐使用绝对导入，这样可读性更好，并且如果导入系统配置不正确（例如，当包内的目录位于 `sys.path` 上时），它们往往表现更好（或至少提供更好的错误消息）：

    ```python
    import mypkg.sibling
    from mypkg import sibling
    from mypkg.sibling import example
    ```

    然而，显式的相对导入是绝对导入的可接受替代方法，特别是当处理复杂的包布局时，使用绝对导入会过于冗长。：

    ```python
    from . import sibling
    from .sibling import example
    ```

    标准库代码应避免复杂的包布局，并始终使用绝对导入。

- 从包含类的模块中导入类时，通常可以这样拼写：

    ```python
    from myclass import MyClass
    from foo.bar.yourclass import YourClass
    ```

    如果此拼写引起本地名称冲突，则应明确拼写它们：

    ```python
    import myclass
    import foo.bar.yourclass
    ```

    然后使用 `myclass.MyClass` 和 `foo.bar.yourclass.YourClass`。

- 应避免通配符导入（`from <module> import *`），因为它们使名称空间中存在哪些名称不清楚，这会使读者和许多自动化工具感到困惑。有一种合理的用例是通配符导入，即将内部接口作为公共 API 的一部分重新发布（例如，使用可选加速器模块的定义覆盖接口的纯 Python 实现，并且不能预先知道将覆盖哪些定义）。

    以这种方式重新发布名称时，仍适用以下关于公共和内部接口的指南。

### §3.8 如何使用 dunders？

模块级 “dunders”（即名称前后有两个下划线）如 `__all__`，`__author__`，`__version__` 等等。他们通常被放置在文档字符串之后，但在任何导入语句（除了 `__future__`）之前。Python规定未来导入必须出现在除文档字符串外的任何其他代码之前：

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

## §4 字符串中的引号

在 Python 中，单引号和双引号的字符串是相同的。这个 PEP 没有对此进行建议。选择一条规则并坚持遵循它。但是，当字符串包含单引号或双引号字符时，请使用另一个字符以避免在字符串中使用反斜杠。这会提高可读性。

对于三引号字符串，请始终使用双引号字符以与 [PEP 257](https://peps.python.org/pep-0257 "PEP 257 – Docstring Conventions") 中的文档字符串约定一致。

## §5 表达式和语句中的空格

### §5.1 小毛病

在以下情况下，请避免使用多余的空格：

- 紧靠在括号，中括号或大括号内：

    ```python
    # Correct:
    spam(ham[1], {eggs: 2})
     ```

    ```python
    # Wrong:
    spam( ham[ 1 ], { eggs: 2 } )
    ```

- 在尾随逗号和后面的右括号之间：

    ```python
    # Correct:
    foo = (0,)
    ```

    ```python
    # Wrong:
    bar = (0, )
    ```

- 在逗号，分号或冒号之前：

    ```python
    # Correct:
    foo = (0,)
    ```

    ```python
    # Wrong:
    bar = (0, )
    ```

- 然而，在切片中，冒号的作用类似于二元运算符，并且两侧应该有相等的间距（将其视为优先级最低的运算符）。在扩展切片中，两个冒号必须应用相同数量的间距。例外情况：当省略切片参数时，空格也被省略：

    ```python
    # Correct:
    ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
    ham[lower:upper], ham[lower:upper:], ham[lower::step]
    ham[lower+offset : upper+offset]
    ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
    ham[lower + offset : upper + offset]
    ```

    ```python
    # Wrong:
    ham[lower + offset:upper + offset]
    ham[1: 9], ham[1 :9], ham[1:9 :3]
    ham[lower : : step]
    ham[ : upper]
    ```

- 紧接在包含函数调用的参数列表的开括号之前：

    ```python
    # Correct:
    spam(1)
    ```

    ```python
    # Wrong:
    spam (1)
    ```

- 紧接在切片或索引目的的开括号之前:

    ```python
    # Correct:
    dct['key'] = lst[index]
    ```

    ```python
    # Wrong:
    dct ['key'] = lst [index]
    ```

- 赋值（或其他）运算符周围使用多个空格以使其与另一个对齐:

    ```python
    # Correct:
    x = 1
    y = 2
    long_variable = 3
    ```

    ```python
    # Wrong:
    x             = 1
    y             = 2
    long_variable = 3
    ```

### §5.2 本章其他的建议

- 避免任何地方都有尾随空格。因为它通常是不可见的，所以会造成困惑：例如，后面跟着一个空格和换行符的反斜杠不算作换行标记。一些编辑器不保留它，许多项目（如CPython本身）都有预提交钩子来拒绝它。

- 总是在这些二元运算符的两侧各加上一个空格：赋值（`=`），增量赋值（`+=`，`-=`等），比较（`==`，`<`，`>`，`！=`，`<>` ，`<=`，`>=`，`in`，`not in`，`is`，`is not`），布尔运算（`and`，`or`，`not`）。

- 如果使用具有不同优先级的运算符，请考虑在具有最低优先级的运算符周围添加空格（多个优先级时）。请自行判断；但是，永远不要使用超过一个空格，并且始终在二元运算符的两侧具有相同数量的空格：

    ```python
    # Correct:
    i = i + 1
    submitted += 1
    x = x*2 - 1
    hypot2 = x*x + y*y
    c = (a+b) * (a-b)
    ```

    ```python
    # Wrong:
    i=i+1
    submitted +=1
    x = x * 2 - 1
    hypot2 = x * x + y * y
    c = (a + b) * (a - b)
    ```

- 函数注释应遵循冒号的常规规则，并且如果存在 `->` 箭头，则始终在其周围加上空格。（有关函数注释的更多信息，请参见下面的“函数注释”）：

    ```python
    # Correct:
    def munge(input: AnyStr): ...
    def munge() -> PosInt: ...
    ```

    ```python
    # Wrong:
    def munge(input:AnyStr): ...
    def munge()->PosInt: ...
    ```

- 在用于指示关键字参数或未注释的函数参数的默认值时，请不要在 `=` 符号周围使用空格：

    ```python
    # Correct:
    def complex(real, imag=0.0):
        return magic(r=real, i=imag)
    ```

    ```python
    # Wrong:
    def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
    ```

    但是，当将参数注释与默认值组合时，请在 `=` 符号周围使用空格：

    ```python
    # Correct:
    def munge(sep: AnyStr = None): ...
    def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
    ```

    ```python
    # Wrong:
    def munge(input: AnyStr=None): ...
    def munge(input: AnyStr, limit = 1000): ...
    ```

- 通常不建议使用复合语句（同一行多个语句）：

    ```python
    # Correct:
    if foo == 'blah':
        do_blah_thing()
    do_one()
    do_two()
    do_three()
    ```

    ```python
    # Wrong:
    if foo == 'blah': do_blah_thing()
    do_one(); do_two(); do_three()
    ```

- 虽然有时可以将 `if`/`for`/`while` 与一个小的主体放在同一行上，但永远不要对于多条子句的语句这样做。同时也避免折叠这样的长行！

    ```python
    # Wrong:
    if foo == 'blah': do_blah_thing()
    for x in lst: total += x
    while t < 10: t = delay()
    ```

    绝对错了：

    ```python
    # Wrong:
    if foo == 'blah': do_blah_thing()
    else: do_non_blah_thing()

    try: something()
    finally: cleanup()

    do_one(); do_two(); do_three(long, argument,
                                 list, like, this)

    if foo == 'blah': one(); two(); three()
    ```

## §6 什么时候在行尾使用逗号

尾随逗号通常是可选的，但是在创建一个元素的元组时必须使用。为了清晰起见，建议将后者用（从技术上来说是多余的）括号括起来：

```python
# Correct:
FILES = ('setup.cfg',)
```

```python
# Wrong:
FILES = 'setup.cfg',
```

当尾随逗号是多余的时候，它们通常在使用版本控制系统时很有帮助，因为预计会随时间推移扩展值、参数或导入的项列表。该模式是将每个值（等）单独放在一行上，始终添加一个尾随逗号，并在下一行上添加关闭括号/括号/大括号。然而，在与闭合定界符相同的行上有一个尾随逗号是没有意义的（单例元组的情况除外）：

```python
# Correct:
FILES = [
    'setup.cfg',
    'tox.ini',
    ]
initialize(FILES,
           error=True,
           )
```

```python
# Wrong:
FILES = ['setup.cfg', 'tox.ini',]
initialize(FILES, error=True,)
```

## §7 注释

与代码相矛盾的注释比没有注释更糟糕。当代码发生变化时，始终优先更新注释！

注释应该是完整的句子。第一个单词应该大写，除非它是以小写字母开头的标识符（永远不要改变标识符的大小写！）。

块注释通常由一个或多个段落组成，每个句子以句号结尾。

在多个语句的注释中，在句子结束的句点后使用一到两个空格，但在最后一句之后除外。

确保您的注释对使用您所写的语言的其他人来说是清晰易懂的。

来自非英语国家的 Python 开发者：请使用英语编写您的注释，除非您十二分确定代码永远不会被不会说您的语言的人读到。

### §7.1 块注释（通常注释）

块注释通常适用于其后面的一些（或所有）代码，并且与该代码缩进到相同的级别。块注释的每行都以`#`和一个空格开头（除非它是注释内缩进的文本）。

块注释中的段落由一行包含单个`#`的线分隔开。

### §7.2 内联注释

请谨慎使用行内注释。

行内注释是与语句在同一行的注释。行内注释应该与语句至少相隔两个空格。它们应该以`#`和一个空格开头。

如果行内注释仅表示显而易见的内容，则是不必要且分散注意力的。不要这样做：

```python
x = x + 1                 # Increment x
```

但是有时候这样是有用的：

```python
x = x + 1                 # Compensate for border
```

### §7.3 文档字符串

有关编写良好文档字符串的约定（又称“docstrings”）已在 [PEP 257](https://peps.python.org/pep-0257 "PEP 257 – Docstring Conventions") 中得到了规定。

- 为所有公共模块、函数、类和方法编写文档字符串。非公共方法不需要文档字符串，但是应该有一条注释描述该方法的作用。此注释应出现在 `def` 行之后。

- [PEP 257](https://peps.python.org/pep-0257 "PEP 257 – Docstring Conventions") 描述了良好的文档字符串约定。请注意，最重要的是，多行文档字符串结束的 `"""` 应该位于单独的一行上：

    ```python
    """Return a foobang

    Optional plotz says to frobnicate the bizbaz first.
    """
    ```

- 对于一行的文档字符串，结尾的`"""`应在同一行：

    ```python
    """Return an ex-parrot."""
    ```

## §8 命名方法

Python 库的命名约定有点混乱，因此我们永远不会完全保持一致。尽管如此，以下是目前推荐的命名标准。新模块和包（包括第三方框架）应按照这些标准编写，但是对于已存在的库具有不同风格的情况，更偏向于内部一致性。

### §8.1 首要的命名原则

对于用户而言，作为 API 公共部分可见的名称的命名方法应反映它的用法而不是它如何实现。
  
### §8.2 描述型的命名样式

现在有很多不同的命名样式。不讨论从函数的名字中是否能看出函数的用途是什么，人们总是可以准确说出它们正在使用的命名样式是什么。

下列是最通常使用的命名样式：

- `b`（单个小写字母）
- `B`（单个大写字母）
- `lowercase`（小写）
- `lower_case_with_underscores`（小写带下划线）
- `UPPERCASE`（大写）
- `UPPER_CASE_WITH_UNDERSCORES`（大写带下划线）
- `CapitalizedWords`（或 CapWords 或 CamelCase——因其字母的隆起外观而得名[^4]）。 这有时也被称为 StudlyCaps。
   注意：在 CapWords 中使用首字母缩写词时，请使用首字母缩写词的所有字母大写。因此，HTTPServerError 比 HttpServerError 看起来更舒服，也更好。
- `mixedCase`（与上文不同的是，首字母小写！）
- `Capitalized_Words_With_Underscores`（丑!）

还有一种使用短的唯一前缀将相关名称组合在一起的样式。这在 Python 中使用不多，但是为了完整起见也提一下：例如，`os.stat()`函数返回一个元组，该元组的项目传统上具有诸如 `st_mode`，`st_size`，`st_mtime` 等名称。（这样做是为了强调与 POSIX 系统调用结构的字段的对应关系，这有助于程序员熟悉该结构。）

X11 库将前导 X 用于其所有公共功能。在 Python 中，通常认为这种风格是不必要的，因为属性和方法名称是以对象为前缀，并且函数名称是以模块名称为前缀。

此外，以下使用前导或尾随下划线的特殊形式得到了认可（通常可以与任何大小写约定组合）：

- `_single_leading_underscore`：“内部使用”, 级别较弱。例如，`from M import *` 不会导入名称以下划线开头的对象。
- `_single_trailing_underscore_`：约定用于避免与 Python 关键字冲突，例如：

    ```python
    tkinter.Toplevel(master, class_='ClassName')
    ```

- `__double_leading_underscore`：命名类属性时，调用此名称修饰（在类 FooBar 中，`__boo` 变为 `_FooBar__boo`；请参见下文）。

- `__double_leading_and_trailing_underscore__`：位于用户控制的名称空间中的“魔法”对象或属性。例如 `__init__`，`__import__` 或者 `__file__` 请勿发明此类名称；仅按文档使用它们。
  
### §8.3 惯例上的命名约定

#### §8.3.1 需要避免的名字

不要将字符“l”（小写字母L），“O”（大写字母o）或“ I”（大写字母i）用作单个字符变量名称。

在某些字体中，这些字符与数字1和0没有区别。当你尝试用“l”时，请改用“ L”。

#### §8.3.2 ASCII 兼容性

标准库中使用的标识符必须与 [PEP 3131](https://peps.python.org/pep-3131 "PEP 3131 – Supporting Non-ASCII Identifiers") 的 [policy section](https://peps.python.org/pep-3131#policy-specification "PEP 3131 – Supporting Non-ASCII Identifiers § Policy Specification") 所述的 ASCII 兼容 。

#### §8.3.3 包和模块名

模块应使用简短的全小写名称。如果模块名称可以提高可读性，则可以在模块名称中使用下划线。尽管不鼓励使用下划线，但 Python 包也应使用短小写全名。

当用 C 或 C++ 编写的扩展模块具有随附的 Python 模块提供更高级别（例如，更面向对象）的接口时，这个 C/C++ 模块应有下划线（例如`_socket`）。

#### §8.3.4 类名

类名通常使用 CapWords 约定样式。

当接口作为可调用主要文档化和使用时，可以改用函数的命名约定。

请注意，内置名称有单独的约定：大多数内置名称都是单词（或两个单词连接在一起），仅在异常名称和内置常量中使用 CapWords 约定。

#### §8.3.5 类型变量名

在 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 中引入的类型变量的名称通常应使用 CapWords 的短名称样式，如 `T`，`AnyStr`，`Num`。建议将用于声明协变或逆变行为的变量后缀分别添加 `_co` 或 `_contra`：

```python
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

#### §8.3.6 “异常”名

因为异常应该是类，所以在这里适用类命名约定样式。但是，您应该在异常名称上使用后缀`Error`（如果异常实际上是一个错误）。

#### §8.3.7 全局变量名

（希望这些变量仅用于一个模块内部。）这些约定与函数的约定大致相同。

设计供 `from M import *` 使用的模块应该使用 `__all__` 机制来防止导出全局变量，或者使用将这些全局变量前缀下划线的旧约定（你可能想要这样做，以表明这些全局变量是“模块非公共”的）。

#### §8.3.8 函数和变量名

函数名称应小写，必要时用下划线分隔单词以提高可读性。

变量名称遵循与函数名称相同的约定。

mixedCase 仅允许在已经存在该样式的情况下使用（例如threading.py），以保持向后兼容性。

#### §8.3.9 函数和方法的参数名

始终将`self`用作实例方法的第一个参数。

始终将`cls`用作类方法的第一个参数。

如果函数参数的名称与保留关键字冲突，通常最好附加单个下划线而不是使用缩写或拼写错误。因此，`class_` 比 `clss` 更好。（也许更好的方法是通过使用同义词来避免这样的冲突。）

#### §8.3.10 方法名和变量实例

使用函数命名规则：小写字母，必要时用下划线分隔单词，以提高可读性。

仅对非公共方法和实例变量使用一个前划线。

为避免名称与子类冲突，请使用两个前导下划线来调用 Python 的名称处理规则。

Python 用类名来修饰这些名称：如果类 Foo 具有名为 `__a` 的属性，则 `Foo.__ a` 不能访问它。（坚持的用户仍然可以通过调用 `Foo._Foo__a` 来获得访问权限。）通常，双引号下划线仅应用于避免名称与设计为子类的类中的属性发生冲突。

注意：关于 ` __name` 的使用存在一些争议（请参见下文）。

#### §8.3.11 常数

常量通常在模块级别定义，并以所有大写字母书写，并用下划线分隔单词。例子有 `MAX_OVERFLOW` 和 `TOTAL`。

#### §8.3.12 继承设计的命名方法

始终决定一个类的方法和实例变量（统称“属性”）是公有还是非公有。如果不确定，选择非公有；将一个公有属性变为非公有更容易。

公有属性是您希望与您的类没有关系的客户端使用的属性，并承诺避免向后不兼容的更改。非公有属性则不打算由第三方使用；您不能保证非公有属性不会发生更改，甚至被删除。

在此我们不使用“private”一词，因为在 Python 中没有真正的私有属性（除非进行通常是不必要的工作）。

另一类属性是“子类 API”的一部分（在其他语言中通常称为“protected”）。某些类旨在被继承，无论是扩展还是修改类的行为方面。当设计这样的类时，请仔细考虑哪些属性是公共的，哪些是子类 API 的一部分，哪些是仅供基类使用的属性。

考虑到这些，以下是 Python 的指南：

- 公共属性不应该有前导下划线。

- 如果您的公共属性名称与保留关键字冲突，请在属性名称末尾附加一个下划线。这比缩写或错误拼写更好。（但是，尽管有此规则，`cls` 是任何已知为类的变量或参数的首选拼写，尤其是类方法的第一个参数。）

    注1：有关类方法的参数名称建议，请参见上面的参数名称建议。

- 对于简单的公共数据属性，最好只公开属性名称，而不是使用复杂的访问器/修改器方法。请记住，在Python中提供了一条易于扩展的道路，如果您发现简单的数据属性需要增加功能性行为，则可以使用属性将功能实现隐藏在简单的数据属性访问语法之后。

    注1：尽量使功能行为无副作用，虽然缓存等副作用通常很好。

    注2：避免使用计算量较大的操作来进行属性；属性符号使调用者认为访问（相对）便宜。

- 如果您的类旨在被子类化，并且有一些您不希望子类使用的属性，请考虑使用双前导下划线和无尾随下划线来命名它们。这将调用 Python 的名称修饰算法，其中类的名称会混淆到属性名称中。这有助于避免属性名称冲突，如果子类意外包含具有相同名称的属性。

    注1：请注意，名称混淆仅使用简单的类名，因此如果子类同时选择相同的类名和属性名，仍然可能会出现名称冲突。

    注2：名称混淆可能会使一些用途，例如调试和`__getattr__()`不太方便。但是名称混淆算法有很好的文档记录，易于手动执行。

    注3：并非每个人都喜欢名称混淆。请尝试平衡避免意外名称冲突的需求与高级调用者的潜在使用。

### §8.4 公共和内部接口名

你只需要保证公共接口具有向后兼容性。因此，重要的是用户能够清楚地区分公共接口和内部接口。

已经文档化(Documented)的接口都应被视为公共接口，除非文档明确声明它们是临时接口或内部接口，而这两种接口不受通常的向后兼容性保证。否则。所有未记录的接口都应假定为内部接口。

为了更方便自我检查，模块应使用`__all__`属性在其公共 API 中显式声明名称。将`__all__`设置为空时，表示该模块没有公共 API。

即使适当地设置了`__all__`，内部接口（包，模块，类，函数，属性或其他名称）仍应使用单个下划线作为前缀。

对于任何的接口，当它包含名称空间（包，模块或类），则该接口也被视为内部接口。

作为 import 的名称，应始终以实现的细节为命名尺度。除非其他模块是被包含模块的 API 中明确文档化的一部分，否则其他模块不得依赖对此类导入名称的间接访问例如 `os.path` 或某个包从子模块中用于体现功能的 `__init__` 模块。

## §9 编程建议

- 应该以不妨碍 Python 其他实现（PyPy，Jython，IronPython，Cython，Psyco等）执行的方式编写代码。

    例如，对于形式为 `a += b` 或 `a = a + b` 的语句，请不要依赖 CPython 对原位字符串连接的有效实现。即使在 CPython 中，这种优化也很脆弱（仅适用于某些类型），并且在不使用引用计数的实现中根本不存在这种优化。在库的性能敏感部分中，应改用 `''.join()` 形式。这将确保在各种实现中串联行为都发生在线性时间内。

- 与单例（如 None）的比较应该始终使用 `is` 或 `is not` 进行，永远不要使用相等 `==` 运算符进行。

    另外，当您要表示 `if x is not None` 时，要当心写 `if x`，例如，在测试默认为 None 的变量或参数是否为其他值时，这个其他值可能会返回布尔 false 的类型（例如容器）！

- 使用 `is not` 运算符，而不是 `not ... is`。虽然两个表达式在功能上都相同，但前者更为易读：

    ```python
    # Correct:
    if foo is not None:
    ```

    ```python
    # Wrong:
    if not foo is None:
    ```

- 当编写基于富比较的排序操作，最好是实现所有六个操作：(`__eq__`，`__ne__`，`__lt__`，`__le__`，`__gt__`，`__ge__`)，而不是依赖其他代码仅执行特定比较。

    为了最大程度地减少工作量， `functools.total_ordering()` 装饰器提供了一种生成缺少的比较方法的工具。

    [PEP 207](https://peps.python.org/pep-0207 "PEP 207 – Rich Comparisons") 表明，交换律在 Python 中是默认假设存在的。因此，解释器可以将 `y > x` 与 `x < y` 交换，`y >= x` 与 `x <= y` 交换，并且可以交换 `x == y` 和 `x！= y` 的参数。`sort()` 和 `min()` 操作是被保证基于 `<` 运算符的，同时 `max()` 函数使用 `>` 运算符。但是，最好写完整所有六个操作，以避免混淆。

- 始终使用 `def` 语句而不是将 lambda 表达式直接绑定到标识符的赋值语句

    ```python
    # Correct:
    def f(x): return 2*x
    ```

    ```python
    # Wrong:
    f = lambda x: 2*x
    ```

    第一种形式意味着生成的函数对象的名称是特定的 `f`，而不是通用的 `<lambda>`。这对于回溯和一般的字符串表示更有用。使用赋值语句消除了 lambda 表达式可以提供的唯一好处，即它可以嵌入到较大表达式中的限制。

- 从 `Exception` 而不是 `BaseException` 派生异常。从 `BaseException` 直接继承的异常通常是不应该被捕获的异常。

    设计异常层次结构基于代码_捕获_异常所需的区别而不是引发异常的位置。旨在以编程方式回答“出了什么问题？”而不仅仅是陈述“发生了某个问题”（请参阅 [PEP 3151](https://peps.python.org/pep-3151 "PEP 3151 – Reworking the OS and IO exception hierarchy")，以获取内置异常层次结构的学习示例）。

    此处适用类命名约定，尽管如果异常是错误，则应将后缀“Error”添加到异常类中。用于非本地流控制或其他形式的信号传递的非错误异常不需要特殊的后缀。

- 适当使用异常链接。应使用 `raise X from Y` 来表示显式替换而不会丢失原始的问题回溯。

    在有意替换内部异常时（使用`raise X from None`），请确保将相关详细信息转移到新异常中（例如，在将 KeyError 转换为 AttributeError 时保留属性名称，或在新异常消息中嵌入原始异常的文本）。

- 捕获异常时，请尽可能提及特定的异常，而不要使用空壳的`except:` 子句:

    ```python
    try:
        import platform_specific_module
    except ImportError:
        platform_specific_module = None
    ```

    一个裸露的 `except:` 子句将捕获 SystemExit 和 KeyboardInterrupt 异常，使使用 ctrl-C 更难以中断程序，并可能掩盖其他问题。如果您想捕获所有表示程序错误的异常，请使用 `except Exception:`（裸露的 except 等效于 `except BaseException:`）。

    一个好的经验法则是将裸露异常子句的使用限制为两种情况：

    1. 如果异常处理程序将打印输出或记录回溯；这意味着至少用户会意识到发生了错误。
    2. 如果代码需要做一些清理工作，但是之后让异常通过 `raise` 向上传播。`try...finally` 可能是处理这种情况的更好方法。

- 在捕获操作系统错误时，优先使用 Python 3.3 引入的显式异常层次结构，而不是对 `errno` 值进行内省。

- 此外，对于所有 `try`/`except` 子句，请将 `try` 子句限制为所需的绝对最小数量的代码。同样，这避免了掩盖错误：

    ```python
    # Correct:
    try:
        value = collection[key]
    except KeyError:
        return key_not_found(key)
    else:
        return handle_value(value)
    ```

    ```python
    # Wrong:
    try:
        # Too broad!
        return handle_value(collection[key])
    except KeyError:
        # Will also catch KeyError raised by handle_value()
        return key_not_found(key)
    ```

- 当资源位于本地(对某段代码来说）时，请使用 `with` 语句以确保在使用后迅速，可靠地对其进行清理。`try`/`finally` 语句也是可以接受的。

- 每当他们执行除获取和释放资源以外的其他操作时，都应通过单独的函数或方法来调用上下文管理器：

    ```python
    # Correct:
    with conn.begin_transaction():
        do_stuff_in_transaction(conn)
    ```

    ```python
    # Wrong:
    with conn:
        do_stuff_in_transaction(conn)
    ```

    错误的示例除了在 transaction 后关闭了链接，并没有提供任何信息来指定 `__enter__` 和 `__exit__`方法在做什么。明确的给出这类信息在这种情形下很关键。

- 在 `return`-语句中，风格应保持一致。函数中的所有 `return`-语句应该返回一个表达式，或者都不返回。如果任何 `return`-语句返回一个表达式，则不返回任何值的任何 `return`-语句都应将其显式声明为 `return None`，并且在函数末尾（如果可以执行到）应存在一个显式 `return`-语句：

    ```python
    # Correct:

    def foo(x):
        if x >= 0:
            return math.sqrt(x)
        else:
            return None

    def bar(x):
        if x < 0:
            return None
        return math.sqrt(x)
    ```

    ```python
    # Wrong:

    def foo(x):
        if x >= 0:
            return math.sqrt(x)

    def bar(x):
        if x < 0:
            return
        return math.sqrt(x)
    ```

- 使用 `''.startswith()` 和 `''.endswith()`而不是字符串切片来检查前缀或后缀。

    `startswith()` 和 `endswith()` 更干净，更不容易出错：

    ```python
    # Correct:
    if foo.startswith('bar'):
    ```

    ```python
    # Wrong:
    if foo[:3] == 'bar':
    ```

- 对象类型比较应始终使用 `isinstance()` 而不是直接比较类型：

    ```python
    # Correct:
    if isinstance(obj, int):
    ```

    ```python
    # Wrong:
    if type(obj) is type(1):
    ```

- 对于序列（字符串，列表，元组），请在使用前假定空序列是否存在：

    ```python
    # Correct:
    if not seq:
    if seq:
    ```

    ```python
    # Wrong:
    if len(seq):
    if not len(seq):
    ```

- 不要写依赖大量尾随空格的字符串文字。这种尾随的空格在视觉上是无法区分的，因此一些编辑器（或最近的，reindent.py）会对其进行修剪。

- 不要使用 `==` 将布尔值与 True 或 False 进行比较。

    ```python
    # Correct:
    if greeting:
    ```

    ```python
    # Wrong:
    if greeting == True:
    ```

     更糟：

    ```python
    # Wrong:
    if greeting is True:
    ```

- 在 `try...finally` 的 finally 语句中使用流程控制语句 `return`/`break`/`continue`，其中流程控制语句会跳出 finally 语句块的范围是不被鼓励的。这是因为这种语句将隐式取消通过 finally 语句块传播的任何活动异常：

    ```python
    # Wrong:
    def foo():
        try:
            1 / 0
        finally:
            return 42
    ```

### §9.1 函数注释

随着对 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 的接受，函数注释的样式规则正在改变。

- 为了向前兼容，Python 3 代码中的函数注释最好应使用 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 语法。（上一部分中对注释有一些格式建议。）

- 不再鼓励使用本 PEP 中先前建议的注释样式进行实验。

- 但是，在 stdlib 之外， 现在鼓励在 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 规则之内进行实验。例如，使用 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 样式类型注释为大型第三方库或应用程序标记，你需要关注添加这些注释的难易程度，并观察它们的存在是否增加了代码的可理解性。

- Python 标准库在采用此类注释时应保持保守，但允许将其用于新代码和大型重构。

- 对于想要不同使用函数注释的代码，建议添加以下形式的注释：

    ```python
    # type: ignore
    ```

     在文件顶部附近；这告诉类型检查器忽略所有注释。（在 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 中可以找到更多的防止类型检查报问题的更细致的方法。）

- 像 linter 一样，类型检查器是可选的独立工具。默认情况下，Python 解释器不应由于类型检查而输出任何消息，也不应基于注释更改其行为。

- 不想使用类型检查器的用户可以随意忽略它们。但是，第三方库软件包的用户可能希望对那些软件包运行类型检查器。为此，[PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints") 建议使用存根文件：`.pyi` 文件优先于相应的 `.py` 文件被类型检查器读取。存根文件可以与库一起分发，或通过 typeshed repo[^5] 单独分发（获得库作者的允许）。

### §9.2 变量注释

[PEP 526](https://peps.python.org/pep-0526 "PEP 526 – Syntax for Variable Annotations") 引入了变量注释。针对它们的样式建议与上述函数注释类似：

- 模块级变量，类和实例变量以及局部变量的注释应在冒号后面有一个空格。

- 冒号前不应有空格。

- 如果一个赋值语句有右边部分，则等号两侧应该各有一个空格。

    ```python
    # Correct:

    code: int

    class Point:
        coords: Tuple[int, int]
        label: str = '<unknown>'
    ```

    ```python
    # Wrong:

    code:int  # No space after colon
    code : int  # Space before colon

    class Test:
        result: int=0  # No spaces around equality sign
    ```

- 尽管 [PEP 526](https://peps.python.org/pep-0526 "PEP 526 – Syntax for Variable Annotations") 已被 Python 3.6 接受，但变量注释语法是所有版本的 Python 上存根文件的首选语法（有关详细信息，请参见 [PEP 484](https://peps.python.org/pep-0484 "PEP 484 – Type Hints")）。

脚注：

[^1]: 悬挂缩进是一种类型设置样式，其中段落中除第一行外的所有行均缩进。在 Python 上下文中，该术语用于描述一种样式，其中带括号的语句的左括号是该行的最后一个非空白字符，其后的行会缩进，直到右括号为止。

## 引用

[^2]: Barry's GNU Mailman style guide http://barry.warsaw.us/software/STYLEGUIDE.txt
[^3]: Donald Knuth's The TeXBook, pages 195 and 196.
[^4]: http://www.wikipedia.com/wiki/CamelCase
[^5]: Typeshed repo https://github.com/python/typeshed

## 版权

该文档已进入公共领域。

来源： https://github.com/python/peps/blob/master/pep-0008.txt
