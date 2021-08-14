# comm

commは、2つのファイルの共通の行を選択もしくは除外する。

commはだいぶクセのあるコマンドだ。まずファイルは現在のcollating sequenceでソートされていなければならない。つまり、ファイルのソートとcommコマンドを使う際に別のLC_COLLATEを使うと正常に動かない。

例を見てみよう。

~~~
$ cat | sort > file1
cat
dog 
monkey
rabbit
$ cat | sort > file2
cat 
dog
mouse
rabbit
$ comm file1 file2
		cat
		dog
monkey
	mouse
		rabbit
~~~

`comm file1 file2`のデフォルトの挙動は、file1のみ存在する行はそのまま出力、file2にのみ存在する行は先頭にタブ文字1つを出力した上で出力、file1, file2に共通して存在する行は先頭にタブ文字2つを出力したうえで出力する。

さきほどの例に注釈をいれると、

~~~
		cat         # タブ文字2つなので共通行
		dog         # タブ文字2つなので共通行
monkey              # タブ文字がないのでfile1にのみ存在する行
	mouse           # タブ文字が1つなのでfile2にのみ存在する行
		rabbit      # タブ文字2つなので共通行

~~~

となる。

commには-1, -2, -3という引数があるが、これはややわかりにくい否定的な意味を持っている。

+ -1は、file1にのみ存在する行を出力しない
+ -2は、file2にのみ存在する行を出力しない
+ -3は、file1, file2に共通する行を出力しない

したがって、file1, file2に共通する行だけを出力したい場合、-1と-2を指定する。

~~~
$ comm -12 file1 file2
cat
dog
rabbit
~~~

見ての通り、-1, -2を両方とも指定した場合は、先頭にタブ文字2つを出力しない。

引数の組み合わせと出力をわかりやすく書くと、

+ file1ユニーク行、file2ユニーク行、共通行の順番がある
+ 3つ出力する場合、1つ目はタブ文字なし、2つ目はタブ文字1つ、3つ目はタブ文字2つ
+ 2つ出力する場合、この順番の最初にくるものはタブ文字なし、次にくるものはタブ文字1つ
+ 1つ出力する場合、タブ文字なし

となる。

何に使えるかというと、2つのログファイルを比較したい場合に使えるだろう。例えば正常に動作しているときのdmesgのログdmesg_normalと、問題が発生したときのdmesgのログdmesg_errorがあるとすると、

~~~
# 先頭のタイムスタンプを除去した上でソートする
$ cat dmesg_normal | cut -d" " -f2- | sort > sorted_dmesg_normal
$ cat dmesg_error | cut -d" " -f2- | sort > sorted_dmesg_error
# dmesg_errorファイルだけに存在する行を出力する
$ comm -13 dmesg_normal dmesg_error
~~~

こうすることで問題が発生したときのdmesgに存在する行のみを出力することができる。

<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/comm.html>
