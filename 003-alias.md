# alias

aliasはエイリアスの定義と表示をする。

シェルがコマンドを実行する前にエイリアスは設定した文字列に置換される。

aliasコマンドを引数なしで実行すると、現在のエイリアス一覧を出力する。

~~~
$ alias
alias ls='ls --color=auto'
~~~

この場合、"ls"コマンドとは、実際には"ls --color-auto"となる。

引数`alias-name=string`を与えると、以降、シェルはalias-nameをstringに置換してコマンドを実行する

~~~
$ alias how-many-bash='ps -e | grep bash | wc -l'
$ how-many-bash
5
~~~

これは現在bashという名前を含むプロセスがいくつあるかを数える便利なエイリアスだ。

aliasは永続しないので、通常は.bashrcなどに書いておくのがおすすめだ。

歴史的経緯をたどると、aliasはもともとKornShellの機能だった。alias自体はシェルの組み込みコマンドとして実装されている場合がほとんどだ。何しろ単純な機能なので、あまり歴史的に面白い話はない。




<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/alias.html>
