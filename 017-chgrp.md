# chgrp

chgrpはファイルのグループを変更する。

UNIXではファイルには所有者(owner)、グループ(group)、その他(other)という概念があり、それぞれにパーミッションが設定されている。パーミッションとはファイルに操作できる権限を設定する。

からのファイルを作ってそのファイルの情報を見てみよう。

~~~
$ touch file
$ ls -l
total 0
-rw-rw-r-- 1 ezoe ezoe 0 Jul  2 04:08 file
~~~

`ls -l`はlong listing formatでファイル情報を表示する。これは以下のように分解できる。

~~~
-               種類
rw-             オーナーのパーミッション
rw-             グループのパーミッション
r--             その他のパーミッション
1               ハードリンク数
ezoe            所有者
ezoe            グループ
0               サイズ
Jul  2 04:08    変更日時
file            名前
~~~

筆者の使っているシステムでは、ユーザーはユーザー名と同じグループに所属している。

POSIXを離れてGNU/Linuxを考えると、グループの一覧は`/etc/group`に書かれている。

~~~
$ cat /etc/group
~~~

現在のユーザーのグループの一覧は`groups`コマンドで取得できる。

~~~
$ groups
ezoe adm cdrom sudo dip plugdev lpadmin sambashare
~~~

筆者はUbuntuを利用しているので、このグループはDebianの流儀に従っているのだが、話のついでに解説すると、ezoeはユーザー名と同じグループで、ユーザーを作るときにユーザーがデフォルトで所属するグループだ。admはシステムモニタリングを可能とするグループで、/var/log以下のログファイルへのアクセスを可能にする。なぜadmなのかというと、歴史的に/var/logは/usr/admで、それが/var/admとなり、今の/var/logになったからだ。cdromは光学ドライブへのアクセスを可能にするグループ。sudoはsudoコマンドの実行を可能にするグループ。dipはダイアルアップツールの仕様を可能にするグループ、plugdevはリムーバブルデバイスのマウントとアンマウントを可能にするグループ、lpadminはプリンターの制御を可能にするグループ、sambashareはSMBプロトコルの実装であるsambaが共有するディレクトリへのアクセスを可能にするグループだ。

ある操作に必要なファイルにあるグループが設定されているので、rootを除いてはそのグループに所属するユーザーしか操作できない。

先程作ったファイルのグループを変えてみよう。

~~~
$ chgrp adm file
$ ls -l
-rw-rw-r-- 1 ezoe adm 0 Jul  2 04:08 file
~~~

これでfileのグループはadmになった。このファイルはその他のユーザーにはリード権限しか与えられていなかったものが、admグループに所属するユーザーはライト権限も持つようになった。

ここまで解説してパーミッションを解説しないのは片手落ちだが、chmodの項で解説する。

<https://pubs.opengroup.org/onlinepubs/9699919799/utilities/chgrp.html>