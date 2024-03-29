# cksum

cksumは、ファイルのチェックサムとサイズを出力する。

~~~
$ echo hello > hello
$ cksum hello
3015617425 6 hello
~~~

順序は、チェックサム、サイズ、ファイルパスの順序だ。

cksumはチェックサムとしてCRC32を計算するが、これは今日のファイルの衝突判定としては甚だ不十分だ。

hash+sumという名前で似たようなPOSIX非標準のコマンドがいくつかある。

かつてはMD5というハッシュアルゴリズムがよく使われていたが、MD5は現在では意図的な衝突が容易になってしまった。

SHA-1もよく使われているが、SHA-1の意図的な衝突が実証されたので現実的になってしまった。

この2021年においてはSHA-2を使うのがいいだろう。sha256sumはSHA-256を計算するのに使える。

~~~
$ sha256sum hello 
5891b5b522d5df086d0ff0b110fbd9d21bb4fc7163af34d08286a2e846f6be03  hello
~~~

出力は、ハッシュ値、空白文字、バイナリモードならば'*'、テキストモードならば' '、ファイル名だ。

<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/cksum.html>
