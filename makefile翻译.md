---
title: CNU Make速成课程，面向专业软件开发者 
date: 2020-07-28 18:17:16
tags:
- C/C++
---

翻译自[ahockersten](https://github.com/ahockersten/makefile_tutorial)

为什么任何人想要学习make？现在已经很老了，并且现在有很多构建系统了，以更好的方式做着同一件事。Make有一些非常重要的东西使得脱颖而出：

* 通用的 - 每个人(在Mac或者Linux等)已经预装make了
* 出名的 - 许多人知道如何阅读和写makefiles，如果你遇到困难，会有大量的帮助
* 稳定的 - 尽管make仍在开发和改进中，但基本格式多年来未曾改变。 将来不太可能发布新版本的make并迫使您对构建脚本进行重大更改

所以，为什么你不想使用make呢？

* 糟糕的语法 - 你一旦驾驭了，它并不糟糕，但是不可否认的是，make不会以人们期望的那样工作
* 困难去跟踪出了什么错 - 确信你可以让make去输出冗杂的调试信息，但是调试makefile仍然是一件极端沮丧的事情
* 非常困难去做一些事。make被设计用来构建和依赖检查，尽管这样做很好，但是处理条件编译之类的内容可能很快就会变得复杂

所以，也许你查看了替代方案，之后一些考虑决定硬着头皮去使用make构建你的项目，但是你不知道如何开始。这是make的一个小小的怪癖，任何人从头创建makefile很少见。在这篇博客中，我会带领你从一个非常简单的例子到复杂，改进一个makefile文件，该文件具有少量自定义功能，在任何项目中都应该有用。

这是一个属于这个项目的git仓库，你可以下载结果并且测试它们，以及探索makefile如何演变。你可以在[这里](https://github.com/ahockersten/makefile_tutorial)找到它。

让我们开始写一个尽可能简单的makefile，这个makefile仅产生一个二进制文件从单一的C输入文件。

```makefile
a.out: main.c
	gcc main.c
```

Makefile由构成目标文件的规则声明组成。这个makefile仅包含一个目标，`a.out`二进制文件。在`:`的左侧，我们列出了输出的目标(outpus)(这些默认被make认为是文件，尽管他们可以是其他东西)。在右侧我们列出output的依赖。在此之后应该是产生输出的命令列表。这行用一个TAB字符缩进。这是make语法必须要求的。如果我们使用空格来代替，我们会得到一个错误的信息，解释是"缺乏分割符(missing separator)"。

我们可以看到make迷雾中第一个重要的组成部分。当你运行了`make`之后，make会比较outputs文件与inputs文件的修改日期，并且当且仅当inputs文件比outputs文件更新时，他会运行与目标有关的命令。

现在，让我们看一个稍微复杂一点的例子：

```makefile
program: main.o
	gcc -o program main.o
main.o : main.c
	gcc -c main.c	
```

这里，我们分开了目标文件的汇编和二进制文件的链接(被重命名为program)。因为`main.o`是`program`的以来，make知道首先应运行`main.o`目标的规则以创建该文件，在尝试构建`program`之前。

让我们使用make的一些功能使它更加通用：

```makefile
program: main.o
	gcc -o $@ $^
%.o: %.c
	gcc -c $^
```

上面的代码出现了三个新概念，第一个是使用一个通用规则(generic)。`%.o: %.c`意思是："产生同名的 '.o'文件，你需要一个'.c'文件(注意，这里产生的.o文件只是上面所依赖的.o文件，并非当前目录下的所有.c文件编译生成.o文件)，并在下面的构建中概述了这一流程"。 `$^`是一个特殊的变量，其意思是“目标的输入(可能包含多个)”(在上面这段代码中第一句，`$^`代表着"main.o")。`$@`意思是“目标的输出(可能包含多个)”，在上面这段代码program这一句是”program“。

现在如果我们增加多个文件，我们可以简单的把它们加入到`program`的依赖中。

```makefile
program: main.o extra.o
	gcc -o $@ $^
%.o: %.c
	gcc -c $^
```

我们假设我们有大量的目标文件，持续的在makefile中手动跟踪它们既容易出错又乏味。于是我们可以使用下面这种方法:

```makefile
C_FILES = $(wildcard *.c)
O_FILES = $(C_FILES:.c=.o)

program: $(O_FILES)
	gcc -o $@ $^
%.o: %.c
	gcc -c $^
```

我们在上面的代码中介绍了两种变量。`C_FILES`是所有的C源码输入文件，`O_FILES`是对应的目标文件。`$(wildcard *.c)`要求make扩展此变量，所以它包含这个目录下所有C文件的名字，所以在我们的例子中使用`$(C_FILES)`和写`main.c extra.c`是相同的。

通过对`C_FILES`中每个空格分隔的元素进行模式替换来生成`O_FILES`。替换任意以”.c“文件结尾为”.o“文件结尾。为什么不对`O_FILES`使用`$(wildcard ...)`而略过创建`C_FILES`变量呢？因为`$(wildcard ...)`只会列出真实存在的文件的列表，当我们第一次运行我们的makefile，没有目标文件存在。换句话说，也就是`program`没有依赖，也就意味着没有目标会被构建，因此`program`也不会被正确的构建。

`O_FILES`的构建使用make的`$(patsubst pattern,replacement,$(var))`函数的速记语法。因此，`O_FILES`的声明等同于`OFILES=$(patsubst %.c,%.o,$(C_FILES))`,通常我发现速记语法更清晰，但是有时可能明确的使用`$(patsubst ...)`函数是更合适的。

让我们以这个例子为示，使其更加完整

```makefile
C_FILES = $(wildcard *.c)
O_FILES = $(C_FILES:.c=.o)
.PHONY: all clean
.DEFAULT: all
all: program
program: $(O_FILES)
	gcc -o $@ $^
%.o: %.c
	gcc -c $^
clean:
	-rm -f $(O_FILES)
	-rm -f program
```

首先，我添加了两个伪目标，`all`和`clean`，这些用作用户的简写，所以你可以运行`make all`和`make clean`代替告诉make哪一个文件应该被生成。我然后增加了两个特殊的目标，`.PHONY`和`.DEFAULT`。`.PHONY`

告诉make，”这个目标不是一个真正的文件“，没有了这行，如果你创建了一个文件名为"all"或者"clean"，make可能产生未预期的运行行为。`.DEFAULT`告诉make如果用户没有指明其他的目标，我们应该构建`.DEFAULT`列出来的目标。换句话说，运行`make`现在意味着与运行`make clean`是同一件事(这行在文件中不是严格必须的，默认，make会构建列出来的第一个的实际目标，在本例中是`all`)。

注：我在测试中`.DEFAULT`无效

最后，我增加了一个`clean`目标，这个`clean`目标的意义是删除任何由其他make目标产生的文件。如果你仔细阅读，你会注意到我放置了一个`-`在`rm`命令前。这告诉make去忽略在运行这条命令中的任何的错误，并且继续构建过程。显然，没有目标文件不是一个错误(意思是它们此前没有被构建)，所以如果`program`文件存在，不应该阻止我们删除`program`文件。

至此，我们达到了”最低限度“的nakefile。但是在我认为这是个好例子前，我还有一些改进。

首先，我想让源文件在它们自己的目录，我不也不想用目标文件污染我们的makefie所在的目录。

```makefile
C_FILES = $(wildcard src/*.c)
O_FILES = $(C_FILES:src/%.c=build/%.o)

.PHONY: all clean
.DEFAULT: all

all: program

program: $(O_FILES)
	gcc -o $@ $^

build:
	@mkdir -p build

build/%.o: src/%.c | build
	gcc -c $< -o $@

clean:
	-rm -f $(O_FILES)
	-rm -f program
	-rm -rf build
```

所以，”.c“文件现在在”src“目录了，目标文件在”build“目录，如果build目录不存在，它要被创建。我增加了一个`@`在mkdir命令前，为的是使它不输出到屏幕。make会在运行前输出任何不以`@`开头的命令。

你会注意到在”build“依赖前我使用了`|`。这意味着它是一个"order-only dependency"。这会确保目录的存在，但是不会看它的时间戳来决定是否需要重新构建。因为当目录里的文件发生改变时，目录的时间戳也改变了。我们需要确保文件不会进行不必要的重构。

注：当我们第一步创建了build目录之后，再在build中存放新创建出的文件，当我们放入文件时，目录的时间戳发生改变，在本例中，如果我们不用`|`符号时，每次`make`时，都需要重新构建一次`build/%.o`。用了`|`符号后，仅在没有build目录或者`src/%.c`发生改变时，才会进行构建`build/%.o`。

我也使用了`$<`代替了`$^`。`$<`仅扩展列表中的第一项依赖。我们在构建过程中不想传递`build`目录到gcc命令中。

我认为这个例子很完整了，如果你遵循上面的结构，修改它来构建其他类型的文件应该很容易了。有一件很流行的事情，是让你的构建脚本产生更好的输出。所以让我们扩展我们的例子做到这一点吧，利用这个机会学到更多make中的概念。

```makefile
# VERBOSE变量可以在make中预定义，也可以在命令行传参，例如`make all VERBOSE=YES`
CC_VERBOSE = $(CC)
# CC_NO_VERBOSE相当于在编译时，只输出一句提示信息"building $(target)"，而不输出gcc命令的信息
CC_NO_VERBOSE = @echo "Building $@..."; $(CC)

ifeq ($(VERBOSE),YES)
  V_CC = $(CC_VERBOSE)
  AT := 
else
  V_CC = $(CC_NO_VERBOSE)
  AT := @
endif

C_FILES = $(wildcard src/*.c)
O_FILES = $(C_FILES:src/%.c=build/%.o)

.PHONY: all clean
.DEFAULT: all

all: program

program: $(O_FILES)
	$(V_CC) -o $@ $^

build:
	$(AT)mkdir -p build

build/%.o: src/%.c | build
	$(V_CC) -c $< -o $@

clean:
	@echo Removing object files
	$(AT)-rm -f $(O_FILES)
	@echo Removing application
	$(AT)-rm -f program
	@echo Removing build directory
	$(AT)-rm -rf build
```

注：$(CC)是make中内置变量，指向gcc。

上面的代码做了什么呢？当你调用make时如果`verbose`设置为`YES`，然后它将输出运行的那些命令的所有信息。如果不是设置为`YES`，这些信息会被隐藏。为了做到这一点，我介绍几个新的变量。首先，我将`@`的所有相关使用替换为`$(AT)`。`$(AT)`仅在非冗杂模式下被设置为`@`。这一点我使用了操作符`:=`。在make中`:=`和`=`的区别有一点复杂并且导致许多bug。`:=`强制所有的变量右值立即展开，而使用`=`确保它们在最后可能的时候展开。可能用一个小例子来阐明它更容易：

```makefile
FOO := $(BAR)
BAR = bar
output: input
	$(FOO) hello world
```

运行`make output`等同于运行`hello world`，因为当`$(FOO)`被设置时`$(BAR)`没有被定义。`$(FOO)`因此会被设置为空字符串。

```makefile
FOO = $(BAR)
BAR = bar
output: input
	$(FOO) hello world
```

这里运行`make output`等同于运行`bar hello world`。当`$(FOO)`被设置时`$(BAR)`仍然未被定义。然而，当`$(FOO)`在扩展`output`时是可用的，它会被扩展为`$(BAR)`,然后会被扩展为”bar"。

注意在makefile例子中，尽可能使用`=`来代替`:=`。我在这里使用`:=`为了阐述`：=`的使用。

这总结了make的重要功能，简短但是希望相当完整的内容。仍然有许多东西学要学习，在下一篇博客文章中，我将从真实的应用程序中借用一个`makefile`，并了解如何对其进行改进。

