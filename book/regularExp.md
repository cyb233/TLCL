---
layout: book
title: 正则表达式 
---

In the next few chapters, we are going to look at tools used to manipulate text. As we
have seen, text data plays an important role on all Unix-like systems, such as Linux. But
before we can fully appreciate all of the features offered by these tools, we have to first
examine a technology that is frequently associated with the most sophisticated uses of
these tools — regular expressions.

接下来的几章中，我们将会看一下一些用来操作文本的工具。正如我们所见到的，在类似于Unix的
操作系统中，比如Linux中，文本数据起着举足轻重的作用。但是在我们能完全理解这些工具提供的
所有功能之前，我们不得不先看看，经常与这些工具的高级使用相关联的一门技术——正则表达式。

As we have navigated the many features and facilities offered by the command line, we
have encountered some truly arcane shell features and commands, such as shell
expansion and quoting, keyboard shortcuts, and command history, not to mention the vi
editor. Regular expressions continue this “tradition” and may be (arguably) the most
arcane feature of them all. This is not to suggest that the time it takes to learn about them
is not worth the effort. Quite the contrary. A good understanding will enable us to
perform amazing feats, though their full value may not be immediately apparent.
What Are Regular Expressions?

我们已经浏览了许多由命令行提供的功能和工具，我们遇到了一些真正神秘的shell功能和命令，
比如shell展开和引用，键盘快捷键，和命令历史，更不用说vi编辑器了。正则表达式延续了
这种“传统”，而且有可能（备受争议地）是其中最神秘的功能。这并不是说花费时间来学习它们
是不值得的，而是恰恰相反。虽然它们的全部价值可能不能立即显现，但是较强理解这些功能
使我们能够表演令人惊奇的技艺。什么是正则表达式？

Simply put, regular expressions are symbolic notations used to identify patterns in text.
In some ways, they resemble the shell’s wildcard method of matching file and pathnames,
but on a much grander scale. Regular expressions are supported by many command line
tools and by most programming languages to facilitate the solution of text manipulation
problems. However, to further confuse things, not all regular expressions are the same;
they vary slightly from tool to tool and from programming language to language. For our
discussion, we will limit ourselves to regular expressions as described in the POSIX
standard (which will cover most of the command line tools), as opposed to many
programming languages (most notably Perl), which use slightly larger and richer sets of
notations.

简而言之，正则表达式是一种符号表示法，被用来识别文本模式。在某种程度上，它们与匹配
文件和路径名的shell通配符比较相似，但其规模更庞大。许多命令行工具和大多数的编程语言
都支持正则表达式，以此来帮助解决文本操作问题。然而，并不是所有的正则表达式都是一样的，
这就进一步混淆了事情；不同工具以及不同语言之间的正则表达式都略有差异。我们将会限定
POSIX标准中描述的正则表达式（其包括了大多数的命令行工具），供我们讨论，
与许多编程语言（最著名的Perl语言）相反，它们使用了更多和更丰富的符号集。

### grep

The main program we will use to work with regular expressions is our old pal, grep.
The name “grep” is actually derived from the phrase “global regular expression print,” so
we can see that grep has something to do with regular expressions. In essence, grep
searches text files for the occurrence of a specified regular expression and outputs any
line containing a match to standard output.

我们将使用的主要程序是老朋友，grep程序，来和正则表达式一起合作。实际上，“grep”这个名字
来自于短语“global regular expression print”，所以我们能看出grep程序和正则表达式有关联。
本质上，grep程序会在文本文件中查找一个指定的正则表达式，并把匹配行输出到标准输出。

So far, we have used grep with fixed strings, like so:

到目前为止，我们已经使用grep程序查找了固定的字符串，就像这样:

<div class="code"><pre>
<tt>[me@linuxbox ~]$ ls /usr/bin | grep zip </tt>
</pre></div>

This will list all the files in the /usr/bin directory whose names contain the substring
“zip”.

这个命令会列出，位于目录/usr/bin中，文件名中包含子字符串“zip”的所有文件。

The grep program accepts options and arguments this way:

这个grep程序以这样的方式来接受选项和参数：

grep [options] regex [file...]

where regex is a regular expression.

这里的regx是指一个正则表达式。

Here is a list of the commonly used grep options:

这是一个常用的grep选项列表：

<p>
<table class="multi" cellpadding="10" border="1" width="%100">
<caption class="cap">Table20-1: grep Options </caption>
<tr>
<th class="title">Option</th>
<th class="title">Description</th>
</tr>
<tr>
<td valign="top" width="20%">-i</td>
<td valign="top">Ignore case. Do not distinguish between upper and lower case
characters. May also be specified --ignore-case.</td>
</tr>
<tr>
<td valign="top">-v</td>
<td valign="top">Invert match. Normally, grep prints lines that contain a match.
This option causes grep to print every line that does not contain a
match. May also be specified --invert-match. </td>
</tr>
<tr>
<td valign="top">-c</td>
<td valign="top">Print the number of matches (or non-matches if the -v option is
also specified) instead of the lines themselves. May also be specified --count.  </td>
</tr>
<tr>
<td valign="top">-l</td>
<td valign="top">Print the name of each file that contains a match instead of the lines
themselves. May also be specified --files-with-matches.  </td>
</tr>
<tr>
<td valign="top">-L</td>
<td valign="top">Like the -l option, but print only the names of files that do not
contain matches. May also be specified --files-without-match.
</td>
</tr>
<tr>
<td valign="top">-n</td>
<td valign="top">Prefix each matching line with the number of the line within the
file. May also be specified --line-number.  </td>
</tr>
<tr>
<td valign="top">-h</td>
<td valign="top">For multi-file searches, suppress the output of filenames. May also
be specified --no-filename. </td>
</tr>
</table>
</p>

<p>
<table class="multi" cellpadding="10" border="1" width="%100">
<caption class="cap">表20-1: grep 选项</caption>
<tr>
<th class="title">选项</th>
<th class="title">描述</th>
</tr>
<tr>
<td valign="top" width="20%">-i</td>
<td valign="top">忽略大小写。不会区分大小写字符。也可用--ignore-case来指定。 </td>
</tr>
<tr>
<td valign="top">-v</td>
<td valign="top">不匹配。通常，grep程序会打印包含匹配项的文本行。这个选项导致grep程序
只会不包含匹配项的文本行。也可用--invert-match来指定。 </td>
</tr>
<tr>
<td valign="top">-c</td>
<td valign="top">打印匹配的数量（或者是不匹配的数目，若指定了-v选项），而不是文本行本身。
也可用--count选项来指定。 </tr>
<tr>
<td valign="top">-l</td>
<td valign="top">打印包含匹配项的文件名，而不是文本行本身，也可用--files-with-matches选项来指定。</td>
</tr>
<tr>
<td valign="top">-L</td>
<td valign="top">相似于-l选项，但是只是打印不包含匹配项的文件名。也可用--files-without-match来指定。</td>
</tr>
<tr>
<td valign="top">-n</td>
<td valign="top">在每个匹配行之前打印出其位于文件中的相应行号。也可用--line-number选项来指定。</td>
</tr>
<tr>
<td valign="top">-h</td>
<td valign="top">应用于多文件搜索，不输出文件名。也可用--no-filename选项来指定。 </td>
</tr>
</table>
</p>

In order to more fully explore grep, let’s create some text files to search:

为了更好的探究grep程序，让我们创建一些文本文件来搜寻：

<div class="code"><pre>
<tt>[me@linuxbox ~]$ ls /bin > dirlist-bin.txt
[me@linuxbox ~]$ ls /usr/bin > dirlist-usr-bin.txt
[me@linuxbox ~]$ ls /sbin > dirlist-sbin.txt
[me@linuxbox ~]$ ls /usr/sbin > dirlist-usr-sbin.txt
[me@linuxbox ~]$ ls dirlist\*.txt
dirlist-bin.txt     dirlist-sbin.txt    dirlist-usr-sbin.txt
dirlist-usr-bin.txt </tt>
</pre></div>

We can perform a simple search of our list of files like this:

我们能够对我们的文件列表执行简单的搜索，像这样：

<div class="code"><pre>
<tt>[me@linuxbox ~]$ grep bzip dirlist\*.txt
dirlist-bin.txt:bzip2
dirlist-bin.txt:bzip2recover </tt>
</pre></div>
