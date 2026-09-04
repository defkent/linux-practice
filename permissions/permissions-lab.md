Linuxパーミッション検証
目的

Linuxのファイル権限を変更し、所有者・グループ・その他のユーザーにどの権限が適用されるか確認する。

実行環境
WSL2
Ubuntu
実行ユーザー: kent
検証ユーザー: testuser
実施内容
1. ファイルを作成

実行コマンド:

touch sample.txt
ls -l sample.txt

初期状態:

-rw-r--r-- 1 kent kent 0 Sep 2 22:20 sample.txt

初期状態の権限は644だった。

2. 権限を640に変更

実行コマンド:

chmod 640 sample.txt
ls -l sample.txt

実行結果:

-rw-r----- 1 kent kent 20 Sep 3 21:56 sample.txt
3. testuserから読み取りを試す

testuserがkentグループに所属していない状態では、読み取りが拒否された。

実行結果:

Permission denied
4. testuserをkentグループに追加

実行コマンド:

sudo usermod -aG kent testuser
id testuser

実行結果:

uid=1001(testuser) gid=1001(testuser) groups=1001(testuser),1000(kent)
5. 権限640で読み書きを確認

kentグループに追加した後、testuserはファイルを読み取れた。

一方、書き込みを試すとPermission deniedになった。

6. 権限を660に変更

実行コマンド:

chmod 660 sample.txt

testuserからファイルへの追記に成功した。

実行結果:

permission practice
written by testuser
7. 権限を640に戻す

実行コマンド:

chmod 640 sample.txt
ls -l sample.txt

最終結果:

-rw-r----- 1 kent kent 40 Sep 3 22:06 sample.txt
分かったこと
chmodで所有者・グループ・その他の権限を変更できる。
640では、グループメンバーは読み取れるが書き込めない。
660では、グループメンバーも読み書きできる。
コマンド実行後はls -lで設定結果を確認することが重要である。
