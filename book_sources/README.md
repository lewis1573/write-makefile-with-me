# 跟我一起写 Makefile ( Markdown 重制版 )


## 简介

`“`《跟我一起写Makefile》是 [陈皓](http://coolshell.cn/haoel)发表在其CSDN博客上的系列文章。该系列文章翻译整理自 [GNU Make Manual](https://www.gnu.org/software/make/manual/)，一直受到读者的推荐，是很多人学习Makefile的首选文档。最早的时候网络上流传的PDF版本多为祝冬华整理的版本。这个版本的排版一般，代码部分没有做任何语法高亮。`”`

因此[seisman](https://seisman.info)使用Sphinx重新制作了一个更精美的PDF版本，同时制作了一个[网页在线版本](https://seisman.github.io/how-to-write-makefile/)，并将所有源文件开源在[how-to-write-makefile](https://github.com/seisman/how-to-write-makefile)。

[seisman](https://seisman.info)的在线版本不支持中文搜索，排版效果也不太友好，代码中的 Tab 全部转为了空格，因为makefile对于空格和 Tab 有严格的区分，所以直接拷贝上面的代码无法直接运行，会对初学者造成困扰，PDF 因为格式本身的缺陷也会丢失 Tab ，所以制作了这个更精美且对初学者友好的网页在线版。将[seisman](https://seisman.info)的 rst 源文件，转换为通用的，专注于内容的 Markdown 文件，并且使用 GitBook 制作了这个在线版本。

## 关于 Tab 及空格

为了解释这个问题，先简单说明下 makefile 的基本语法：

``` makefile
test.o:test.c
	gcc -c test.c test.o
```

第一行代码表示要将 test.c 制作成 test.o ，第二行代码是所谓的命令，说明了怎么制作，即调用 gcc 加上这几个参数制作。需要关注的是 gcc 命令前面有一点空白，这个空白必须使用一个 Tab 而不是一个或几个空格，我保证，我书写的时候，这里也是 Tab 而不是空格，但是你现在去选中这段空白看看，很有可能是四个空格，这是因为我制作本书的工具GitBook在将 Makedown 文件转换为 html 的时候将 Tab 替换为空格了，遗憾的是，不只是GitBook这么做，很多转换工具都是这样的，因为 Tab 在转换 html 显示的时候难以处理，所以你会看到 makefile 官方的在线文档里，这个空白竟然也是空格！可以想象假如一个初学者，不知道这个规则，准备从官方文档复制两句简单代码去运行一下，结果报了一个错（这个错也不会说是少一个 Tab 或者多了空格），会不会开始怀疑人生呢？ Tab和空格造成的另一个困惑就是几乎所有的编辑器都不显示空格和 Tab ，对他们的显示都是一样的空白，看着一行有空白的代码，谁也无法判断空白是 Tab 还是空格。

针对这个问题 GNU make 打了个补丁， 从3.82版本（发布于2010年）开始，可以使用其他的命令前缀来替换 Tab ，比如在 makefile 的一开始定义 `.RECIPEPREFIX = >` ，那么后面所有的命令前缀将使用 `>` ， 而不再是 Tab ，直到 `.RECIPEPREFIX` 被赋给其他值，所以一般情况一个makefile 只需要在一开始的时候定义一次 `.RECIPEPREFIX` 即可。上面的例子使用新的格式书写将会是下面的样子：

``` makefile
.RECIPEPREFIX = >
test.o:test.c
>  gcc -c test.c test.o
```

这里需要特别说明的是，第三行的 `>` 前面不能有空格，后面可以有，多余的空格将会被忽略，第一行`.RECIPEPREFIX`定义的时候 `>` 的前后都可以有空格，都将会被忽略。

为了解决 Tab 和空格引起的这些问题，这个版本中所有的例子都采用上面这种新规则，使用自定义命令前缀。为了保证每一段例子都可以单独运行，在每个例子都定义了前缀，不过正如上文所说，在实际的 makefile 中，只需要定义一次即可。

关于 makefile 的 Tab 前缀，这里作一些补充说明，这个规则是原始 make 的作者 Stuart Feldman 定义的，他本人明确表示过，这是个坏主意，但是为了不影响已有用户，就没有更改，直到后来对数以万计的人造成了灾难， 他本人在软件工程的讲座中使用了这个例子（作为反例）。（[信息来源](https://retrocomputing.stackexchange.com/questions/20292/why-does-make-only-accept-tab-indentation)）

在 GUN make 更新12年后的今天，我认为是时候摒弃 Tab 作为命令的前缀这个坏主意了。

