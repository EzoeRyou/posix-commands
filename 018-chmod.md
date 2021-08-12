# chmod

chmodは、ファイルのモードを変更する。

chmodはchange modeの略だ。さらにいうと、変更するのはmode bitsであり、一般的にはパーミッション(permission)と呼ばれている。少なくとも筆者はモードという名称を日常的に使っていないし、口頭で「ファイルのモードを変更する」などと言って同業者に通じるとは思えないのだが、この辺の名称の歴史はよくわからない。


モードというのは、r, w, xのことで、それぞれ、read, write, executeを意味する。実はモードにはファイルとディレクトリーがある。ファイルに対するモードはそのまま、読み、書き、実行でいいのだが、ディレクトリーに対するモードは少し変わっている。

実験用にディレクトリーとファイルを作ろう。

~~~
$ mkdir dir
$ touch file
$ ls -l
total 4
drwxrwxr-x 2 ezoe ezoe 4096 Aug 12 09:50 dir
-rw-rw-r-- 1 ezoe ezoe    0 Aug 12 09:50 file
~~~

ディレクトリーdirの`drwxrwxr-x`に注目しよう。最初のdはディレクトリーを意味する。残りの9文字は9ビットのモードを表現したものだ。最初の3文字はuserであるファイル所有者のモードであり、次の3文字はグループ(group)のモードであり、最後の3文字はその他(other)のモードだ。

chmodの引数では、ユーザー、グループ、その他はそれぞれu,g,oで指定する。すべてを指定したい場合はaを使う。省略した場合は+-ではaと同じ扱いになる。

~~~
# ユーザー
$ chmod u+r
# グループ
$ chmod g+r
# ユーザーとグループ
$ chmod ug+r
~~~

+でモードを付与し、-で除去する。モードは、r, w, xだ。

作成したディレクトリーは所有者に対してrwxのモードビットがすべて立っている。rを除去してみよう。

~~~
$ chmod -r dir
$ cd dir
dir$ ls
ls: cannot open directory '.': Permission denied
dir$ echo hello > file
dir$ cat file
hello
~~~

ディレクトリーからrを外すと、ディレクトリー内のファイルの列挙ができなくなる。ディレクトリー内にファイルを作ったり読んだりすることには問題がない

ディレクトリーにrを戻してからwを除去してみよう。

~~~
$ chmod +r dir
$ chmod -w dir
$ cd dir
dir$ ls
file
dir$ cat file
hello
dir$ echo hello > hello
bash: hello: Permission denied
dir$ mv file foo
mv: cannot move 'file' to 'foo': Permission denied
dir$ rm file
rm: cannot remove 'file': Permission denied
~~~

ディレクトリーからwを外すと、ディレクトリー内にファイルを作成することはできないし、既存のファイルの名前を変更、削除することもできなくなる。

xはなんだろうか。ディレクトリーは実行するものではない。ディレクトリーに対するxはディレクトリー内のファイル情報へのアクセスを制御している。xのないディレクトリーはカレントディレクトリーにすることができなくなる。ディレクトリー内のファイルへのアクセスもできない。

~~~
$ chmod +w dir
# xを除去
$ chdmo -x dir
$ cd dir
bash: cd: dir: Permission denied
$ cat dir/file
cat: dir/file: Permission denied
~~~

このような特性から、ディレクトリーに対するxはsearchと呼ばれることもある。xがないと検索ができなくなるのだ。

ファイルのモードはわかりやすい。rはreadでwはwriteだ。

~~~
$ echo hello > file
$ cat file
hello
# rを除去
$ chmod -r file
# 読めない
$ cat file
cat: file: Permission denied
# wを除去
$ chmod -w file
# 書けない
$ echo hello > file
bash: file: Permission denied
~~~

xは実行だ。

~~~
$ cat file
#! /bin/bash

echo hello
# 実行できない
$ ./file
bash: ./file: Permission denied
# xを付与
$ chmod +x file
# 実行できる
$ ./file
hello
~~~

<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/chmod.html>
