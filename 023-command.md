# command

commandは、単純なコマンドを実行する。

~~~
$ command echo hello
hello
~~~

`command echo hello`は`echo hello`を実行する。

commandコマンドは一見すると無価値なコマンドのように思える。コマンドを実行したければそのまま実行すればいいのではないか、と思うかもしれない。しかしcommandはとても重要なのだ。

commandはシェル関数を呼び出さない。

~~~
$ shell-function
shell-function: command not found
$ function shell-function { echo shell function; }
$ shell-function 
shell function
$ command shell-function
shell-function: command not found
~~~

この挙動は重要だ。というのも、既存のコマンドと同名のシェル関数を書いた場合、シェル関数の中からもとのコマンドを普通に実行することはできないからだ。

例えばechoが呼び出されるたびになにか追加で処理をしたいとする。そこでシェル関数echoを書くとしよう。

~~~bash
function echo {
    # echoが呼び出されるたびに追加でする処理
    # 元のechoを呼び出す・・・つもり
    echo $@
}
~~~

残念ながらこれは正しく動かない。echoはシェル関数として定義されてしまったので、このシェル関数は自分自身を呼び出す再帰になり、無限ループに陥る。こういう場合、commandを使う。

~~~bash
function echo {
    # echoが呼び出されるたびに追加でする処理
    # 正しく元のechoを呼び出す
    command echo $@
}
~~~

commandには引数-pがある。これはPATHのデフォルト値でコマンドを探す。PATH変数が破壊されていても標準のコマンドを安全に実行することができる。

~~~
$ PATH=""
$ date
bash: date: No such file or directory
$ command -p date
Sat Aug 14 11:20:21 PM JST 2021
~~~

PATH変数が破壊されたのになぜcommandコマンドは見つけることができるのかというと、commandはシェルの組み込みコマンドだからだ。

commandにはコマンドが実際にどのように実行されるのか、どのように解釈されるのかということを知る機能もある。

引数-vをつけると、コマンドがどのようにシェルに使われるかを出力する。

例えば、bashの場合は以下のようになる。

~~~
$ command -v cat
/usr/bin/cat
$ command -v command
command
$ command -v ls
alias ls='ls --color=auto'
~~~

この環境では、catは`/usr/bin/cat`にあるファイルが実行される。commandはシェルの組み込みコマンドなのでcommandとして扱われる。lsはエイリアスされているので、エイリアスが表示される。

引数-Vをつけると、シェルが名前をどのように解釈するかを出力する。

bashの場合は以下のようになる。

~~~
command -V cat
cat is /usr/bin/cat
$ command -V command
command is a shell builtin
$ command -V ls
ls is aliased to `ls --color=auto'
~~~



<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/command.html>
