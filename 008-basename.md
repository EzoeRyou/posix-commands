# basename

basenameは、パス名のうち非ディレクトリー部分を返す。

非ディレクトリー部分というのはファイル名のことだ。

~~~
$ basename /aaa/bbb/ccc
ccc
$ basename /usr/bin/basename
basename
~~~

basenameには対になるdirnameがあり、こちらはパス名のディレクトリー部分を返す。POSIXでは対になるようにdirnameの挙動に変更が加えられたそうだ。

basenameは1979年には存在したそうなのだが、初めて同梱されたUNIXは1982年のSYSTEM IIIということになっており、またBSDは4.4BSDから同梱したそうでこれは1993年になる。この辺の時系列は筆者の世代にはもうよくわからない。



<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/basename.html>
