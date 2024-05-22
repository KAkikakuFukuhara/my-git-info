# Git Tutorial

- [1. git に関してと本稿に関して](#1-git-に関してと本稿に関して)
- [2. git インストールと初期設定](#2-git-インストールと初期設定)
  - [2.1. git インストール](#21-git-インストール)
  - [2.2. git 初期設定](#22-git-初期設定)
- [3. git履歴の初期化](#3-git履歴の初期化)
- [4. ファイルの追加と履歴の保存](#4-ファイルの追加と履歴の保存)
  - [4.1. ファイルの追加](#41-ファイルの追加)
  - [4.2. ファイルの変更](#42-ファイルの変更)
  - [4.3. ディレクトリの追加](#43-ディレクトリの追加)
  - [4.4. 指定したファイルだけ今回のコミットに含める](#44-指定したファイルだけ今回のコミットに含める)
  - [4.5. 特定のファイルを追跡対象にしない(.gitignore)](#45-特定のファイルを追跡対象にしないgitignore)
    - [4.5.1. 特定の拡張子のファイルを追跡対象にしない](#451-特定の拡張子のファイルを追跡対象にしない)
    - [4.5.2. 例外の追加](#452-例外の追加)
    - [4.5.3. ディレクトリまるごと](#453-ディレクトリまるごと)
- [5. 履歴の表示と差分の表示](#5-履歴の表示と差分の表示)
  - [5.1. 履歴の表示](#51-履歴の表示)
  - [5.2. 差分の表示](#52-差分の表示)
    - [5.2.1. ある時点Ａからある時点Ｂの差分](#521-ある時点ａからある時点ｂの差分)
  - [5.3. 実際の運用に関して](#53-実際の運用に関して)
- [6. ブランチの作成と切り替え](#6-ブランチの作成と切り替え)
  - [6.1. ブランチの説明とブランチの追加そして削除](#61-ブランチの説明とブランチの追加そして削除)
  - [6.2. ブランチの切り替え](#62-ブランチの切り替え)
  - [6.3. ブランチの切り替え（深堀り）](#63-ブランチの切り替え深堀り)
  - [6.4. ブランチの切り替え（さらに深堀り）](#64-ブランチの切り替えさらに深堀り)
- [7. マージ](#7-マージ)
  - [7.1. マージの基礎](#71-マージの基礎)
  - [7.2. コンフリクト](#72-コンフリクト)
- [8. コミットの巻き戻し](#8-コミットの巻き戻し)
  - [8.1. 巻き戻しの基礎](#81-巻き戻しの基礎)
  - [8.2. 巻き戻し(git reset オプション)](#82-巻き戻しgit-reset-オプション)
- [9. ブランチの強制削除](#9-ブランチの強制削除)
- [10. Tips](#10-tips)
  - [10.1. コミット後の修正（コメントを修正したい or ファイルの追加忘れ or ファイルの変更点の保存忘れ）](#101-コミット後の修正コメントを修正したい-or-ファイルの追加忘れ-or-ファイルの変更点の保存忘れ)

## 1. git に関してと本稿に関して

 　`git`は普遍的に使われているファイルのバージョン管理ツールである。ファイルのバージョン管理ツールは誰が何時どのようにファイルを変更したかを確認することができる。コレとは別に、web上でファイルのバージョン管理ができる`github`というサービスがある。このサービスはしばしば、`git`と連携して使用されることがあるが、別物のツールである点を理解して欲しい。
 　本稿では`github`と連携していない状態での`git`の使い方に関して記載する。単純にソフトウェアを`github`からダウンロードしてきて使うだけなら、本稿で解説するような内容は必要ないが、`git`を用いてバージョン管理を実際に行っていく場合は必ず必要となる内容であると筆者は考えている。
 　本稿の読み方としては、実際に`git`を操作しながら使い方を覚えていくスタイルを採用している。記載されている内容を読むだけではなく、実際に読者のPC上で操作を実行しながら使い方を覚えていただきたい。
 　なお本稿で使用されているPCのOSは`Ubuntu 18.04 LTS`である。Windows や Mac では勝手が違う可能性があるが、それらは考慮しないこととする。

## 2. git インストールと初期設定

### 2.1. git インストール
 　`git`アプリのインストールは以下のように行う。  
```bash
sudo apt update # 全てのパッケージの最新情報を取得
sudo apt install git
```
### 2.2. git 初期設定

 　gitを活用するには名前とメールアドレスを設定する必要がある。あとから変えるので適当でよい。  

```bash
git config --global user.name "HOGE FUGA"
git config --global user.email "HOGE@FUGA.test0527.com"
```

## 3. git履歴の初期化

 　ディレクトリをgitの追跡対象とするには`git init`コマンドを用いる必要がある。以下のように新しくディレクトリを作成して、そのディレクトリをgitの追跡対象にしてみよう。  

```bash
$ mkdir git-tutorial-free-test
$ cd git-tutorial-free-test/
$ ls -a
. ..

$ git init
Initialized empty Git repository in /home/fukuhara/workspace/test/git-tutorial-free-test/.git/
$ ls -a
.  ..  .git
```
 　git の追跡対象の情報は`.git`に保存される。

## 4. ファイルの追加と履歴の保存

 　git の現在の状態は以下のように`git status`で確認できる。
```bash
$ git status
ブランチ master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```
 　まだ何も追跡していないので `No commits yet`と出る。ココでいるコミットとはコミットオブジェクトのことで、これはグループ単位の変更履歴を指す。詳細は後述する。  

### 4.1. ファイルの追加

 　それでは、以下のように新しいファイルを作成してgitの状態を見てみる。`echo "aaaa" >> test.txt` は test.txtが存在しない場合は作成してテキスト"aaaa"を記述するbashコマンドで、既に存在している場合は、最終行にテキストを追加する。  
```bash
$ echo "aaaa" >> test.txt
$ ls -a
.  ..  .git test.txt
$ git status
ブランチ master

No commits yet

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test.txt

nothing added to commit but untracked files present (use "git add" to track)
```
 　上記を実行すると、追跡されていないファイルとして先程追加したtest.txtが表示されているのが分かるだろう。  
　このファイルを追跡対象として登録するには以下のように`git add`コマンドを利用する。  
```bash
$ git add test.txt 
$ git status
ブランチ master

No commits yet

コミット予定の変更点:
  (use "git rm --cached <file>..." to unstage)

	new file:   test.txt
```
 　次はコミット予定の変更点という表記が見られる。登録されたファイルは実はまだコミットオブジェクトとして保存されていない。この`git add`はコミットオブジェクトの候補として登録するコマンドである。一般的に`git add`のこのような処理を**ステージ**と呼び、ステージされたファイルが置かれている場所を**ステージングエリア**と呼ぶ。  

 　次にコミットを行う。コミットはステージングエリアのファイル群をコミットオブジェクトとしてひとまとめにしてリポジトリに保存する作業である。リポジトリとは変更履歴を保存する場所の概念であり、木構造のような構造をしている。  
 　コミットは以下のように`git commit`コマンドを用いる。ここでオプションとして`-m`と文字列を用いているが、これはコミットオブジェクトに対してコメントを記入するオプションである。コミットにはコメントは必須であるが、このオプションを実行しない場合は強制的にエディタが起動してしまう。エディタの操作方法の説明はしたくないのでこのオプションを使用している。  

```bash
$ git commit -m "first commit"
[master (root-commit) 44b500f] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt

$ git status
ブランチ master
nothing to commit, working tree clean
```
 　これでコミットが完了し変更履歴がリポジトリへ保存された。この `git add + git commit`の流れは作業フォルダからステージングエリアに変更したファイル群を登録して、リポジトリに変更履歴としてまとめて保存するというようになっている。簡略化すると`作業ディレクトリ→ステージング→リポジトリ'という流れとなる。図が見たい人は Google検索で "git コミット　ステージング　画像" で検索してほしい。  

### 4.2. ファイルの変更

 　さて先程はファイルの追加に関して記載した。次にファイルの変更の場合を以下に記載する。  

```bash
$ echo "bbbb" >> test.txt
$ ls -a
.  ..  .git  test.txt
$ cat test.txt
aaaa
bbbb
$ git status
ブランチ master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
 　ファイルの変更を行った際にはファイルの追加の際には`new file: test.txt`であった表記が `modified:  test.txt` という表記に変わっている。  

　これを`git add + commit`する方法は追加の時と同じである。
```bash
$ git add test.txt
$ git status
ブランチ master
コミット予定の変更点:
  (use "git reset HEAD <file>..." to unstage)

	modified:   test.txt

$ git commit -m "[Update] test.txt"
[master 2a94b90] [Update] test.txt
 1 file changed, 1 insertion(+)
$ git status
ブランチ master
nothing to commit, working tree clean
```
 　これで変更に関しての処理が完了した。

### 4.3. ディレクトリの追加

 　次にディレクトリ毎追加を行う。追加の方法は以下のように対象にディレクトリを指定するだけで、コマンドは同じである。
```bash
$ ls -a
.  ..  .git  test.txt
$ mkdir hoge_dir
$ echo "cccc" >> hoge_dir/test2.txt
$ echo "dddd" >> hoge_dir/test3.txt
$ ls -a
.  ..  .git  hoge_dir  test.txt
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	hoge_dir/

nothing added to commit but untracked files present (use "git add" to track)
$ git add hoge_dir
$ git status
ブランチ master
コミット予定の変更点:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hoge_dir/test2.txt
	new file:   hoge_dir/test3.txt

$ git commit -m "[Add] hoge_dir"
[master 1ea6ace] [Add] hoge_dir
 2 files changed, 2 insertions(+)
 create mode 100644 hoge_dir/test2.txt
 create mode 100644 hoge_dir/test3.txt
$ git status
ブランチ master
nothing to commit, working tree clean
```

### 4.4. 指定したファイルだけ今回のコミットに含める

 　あるファイルに関してはコミットをしたいが、あるファイルはコミットしたくないような状況が発生することがある。そのような場合は以下のように特定のファイルだけコミットすることが可能である。

```bash
$ echo "eeee" >> test4.txt
$ echo "ffff" >> test5.txt
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test4.txt
	test5.txt

nothing added to commit but untracked files present (use "git add" to track)
$ git add test4.txt 
$ git status
ブランチ master
コミット予定の変更点:
  (use "git reset HEAD <file>..." to unstage)

	new file:   test4.txt

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test5.txt

$ git commit -m "[Add] test4.txt"
[master 575b2b5] [Add] test4.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test4.txt
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test5.txt

nothing added to commit but untracked files present (use "git add" to track)
```
 　当然その後、以下のように残して置いたファイルを追加したりするのは自由である。
```bash
$ git add test5.txt
$ git commit -m "[Add] test5.txt"
[master e1475de] [Add] test5.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test5.txt
$ git status
ブランチ master
nothing to commit, working tree clean
```
### 4.5. 特定のファイルを追跡対象にしない(.gitignore)

 　gitはデフォルトの状態だと全てのファイルを追跡対象に含めることができる（筆者の知識上は。例外可能性有り）。一方で画像処理なんかをしていると大量の画像ファイルを扱う時があり、それらは追跡対象に含んでほしくないことがある。そういった時は`.gitignore`を設定する。  

#### 4.5.1. 特定の拡張子のファイルを追跡対象にしない

 　まず複数の画像ファイルを用意する。`touch HOGE`はHOGEという名称の空ファイルを作成するbashコマンドである。
```bash
$ touch test1.jpg
$ touch test2.jpg
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test1.jpg
	test2.jpg

nothing added to commit but untracked files present (use "git add" to track)
```
 　`.gitignore`というファイルに除外したい拡張子のファイルを正規表現で追加。先程'追跡されていないファイル'として表記されていたjpgファイルが表示されなくなる。  
```bash
$ echo "*.jpg" >> .gitignore
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```
 　`git add`の対象をカレントディレクトリにしても追加されていないことが分かる。そしていつもどおりコミットする。  
```bash
$ git add ./
$ git status
ブランチ master
コミット予定の変更点:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .gitignore

$ git commit -m "[Add] .gitignore"
[master 71f617c] [Add] .gitignore
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore

```

#### 4.5.2. 例外の追加

 　先程はワイルドカードを用いてjpgファイルを全て無視するようにした。しかし特定のファイルは追跡対象にしたい場合がある（例えばサンプルデータとか）。そのような場合は以下のようにエクスクラメーションマーク（ビックリマーク）を除外したいファイルパスの手前に置いて、.gitignoreに追記する。（echoコマンドの表記が少し異なるが`!`の追記に必要なためである[^1](https://qiita.com/anqooqie/items/785f46a8cc5f10ba7abb))

```bash
$ echo "$(echo '!test1.jpg')" >> .gitignore
$ git status
ブランチ master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   .gitignore

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test1.jpg

no changes added to commit (use "git add" and/or "git commit -a")
$ git add ./
$ git status
ブランチ master
コミット予定の変更点:
  (use "git reset HEAD <file>..." to unstage)

	modified:   .gitignore
	new file:   test1.jpg

$ git commit -m "[Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加"
[master bef3708] [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
 2 files changed, 1 insertion(+)
 create mode 100644 test1.jpg
```

 　これで例外を設定できた。  

#### 4.5.3. ディレクトリまるごと

 　ファイルや正規表現以外にもディレクトリもまるごと無視するように指定できる。  
 以下のようにディレクトリパスを.gitignoreに含めるとまるごと無視するようになる。  

```bash
$ mkdir fuga_dir
$ touch fuga_dir/test1.png
$ touch fuga_dir/test2.png
$ git status
ブランチ master
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	fuga_dir/

nothing added to commit but untracked files present (use "git add" to track)
$ echo "fuga_dir" >> .gitignore
$ git status
ブランチ master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
$ git add ./.gitignore 
$ git commit -m "[Update] add do not include 'fuga' into .gitignore." 
[master 7517781] [Update] add do not include 'fuga' into .gitignore.
 1 file changed, 1 insertion(+)
```

 　.gitignoreのより詳細な設定に関しては以下に記載しておく。実際に運用している際に困ったことがあれば以下を確認することや自分で調べること。
 - [https://www-creators.com/archives/1662](https://www-creators.com/archives/1662)

## 5. 履歴の表示と差分の表示

 　gitは履歴を管理するツールであるため、履歴の表示や差分が取れる。  

### 5.1. 履歴の表示 

 履歴の表示は以下のような`git log`コマンドを使うと履歴が表示できる。  

```bash
$ git log
commit 7517781dc1c19592aa62cd7f0f163d6e89f55525 (HEAD -> master)
Author: Kenta Fukuhara <105337464+KAkikakuFukuhara@users.noreply.github.com>
Date:   Wed May 22 11:36:21 2024 +0900

    [Update] add do not include 'fuga' into .gitignore.

commit bef3708de917d44038cfe94feaf3f359f35c3b6e
Author: Kenta Fukuhara <105337464+KAkikakuFukuhara@users.noreply.github.com>
Date:   Wed May 22 11:26:05 2024 +0900

    [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加

commit 71f617cad72ebc7dccf58efaf5bafebf8a3ad880
Author: Kenta Fukuhara <105337464+KAkikakuFukuhara@users.noreply.github.com>
Date:   Wed May 22 11:20:48 2024 +0900

    [Add] .gitignore

commit e1475de0474aa00466cca11810d27787a3340bb2
Author: Kenta Fukuhara <105337464+KAkikakuFukuhara@users.noreply.github.com>
Date:   Wed May 22 11:03:47 2024 +0900

    [Add] test5.txt
...
```
 　上記を使うと`git config`で設定した名前とメールアドレスに基づいて誰が何時どのような変更をしたのかを確認することができる。  
 　なおどのような変更かあったかだけをを確認したい場合は以下のオプションで省略することができる。  
```bash
$ git log --oneline
7517781 (HEAD -> master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
e1475de [Add] test5.txt
575b2b5 [Add] test4.txt
1ea6ace [Add] hoge_dir
2a94b90 [Update] test.txt
44b500f first commit
```
 　ここで1列目に表示されている数字はコミットハッシュと呼ばれる値でコミットオブジェクトに毎に一意の値を取る。そして２列目にコメントがあるわけであるが、(HEAD -> master) と表記されている行がある。これは'master'がブランチ名、`HEAD`が今自分がいるリポジトリの位置を表記している。ここの話に関しては後述する'ブランチの作成と取り替え'の章で説明を行う。とりえあず`HEAD`が現在いる位置を表していると理解していればいい。  

### 5.2. 差分の表示  

 　履歴を確認した後は差分を表示する。  
 以下のコマンドで今のHEADの位置のコミットオブジェクトでどのような変更が行われたかが表示される。  
```bash
$ git diff HEAD~1
diff --git a/.gitignore b/.gitignore
index 33c8d42..489b0dd 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1,2 +1,3 @@
 *.jpg
 !test1.jpg
+fuga_dir
```
 　今回は 'ディレクトリをまとめて無視する'の節で行われた.gitignore に fuga_dir を無視するように追加した変更を見ることができた。  
 　`HEAD~N` の N の数字を大きくすることで以下のようにさらに過去の差分を掘ることができる。  
```bash
$ git diff HEAD~2
```
```diff
diff --git a/.gitignore b/.gitignore
index 76ce7fc..489b0dd 100644
--- a/.gitignore
+++ b/.gitignore
@@ -1 +1,3 @@
 *.jpg
+!test1.jpg
+fuga_dir
diff --git a/test1.jpg b/test1.jpg
new file mode 100644
index 0000000..e69de29
```

#### 5.2.1. ある時点Ａからある時点Ｂの差分

 　ある時点Ａからある時点Ｂの差分を見たいという需要が考えられる。そのような方法はコミットハッシュを利用することで実現できる。例えば以下のようにして履歴一覧を表示する。  
```bash
$ git log --oneline
7517781 (HEAD -> master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
e1475de [Add] test5.txt
575b2b5 [Add] test4.txt
1ea6ace [Add] hoge_dir
2a94b90 [Update] test.txt
44b500f first commit
```
 　ここで `1ea6ace` から `e1475de`  の差分を見たい場合は以下のようにして確認する。  

```bash
$ git diff 1ea6ace e1475de
```
```diff
diff --git a/test4.txt b/test4.txt
deleted file mode 100644
index 02dc432..0000000
--- a/test4.txt
+++ /dev/null
@@ -1 +0,0 @@
+eeee
diff --git a/test5.txt b/test5.txt
deleted file mode 100644
index 6c269ec..0000000
--- a/test5.txt
+++ /dev/null
@@ -1 +0,0 @@
+ffff
```

 　確かに`1ea6ace [Add] hoge_dir` から `e1475de [Add] test5.txt`見ると、test4.txtとtest5.txtが追加されているため正しい。  
 　一方で以下のように逆方向の差分を取ると追加ではなく削除されていることが分かる。  

```bash
$ git diff e1475de 1ea6ace
```
```diff
diff --git a/test4.txt b/test4.txt
deleted file mode 100644
index 02dc432..0000000
--- a/test4.txt
+++ /dev/null
@@ -1 +0,0 @@
-eeee
diff --git a/test5.txt b/test5.txt
deleted file mode 100644
index 6c269ec..0000000
--- a/test5.txt
+++ /dev/null
@@ -1 +0,0 @@
-ffff
```

### 5.3. 実際の運用に関して

 　ここまでgitの履歴の表示や差分の表示に関して説明してきたが、筆者はこれらを使用していない。筆者はエディタとして`vscode`を使用しており、その拡張機能である [Git Grpah](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) および [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) を使用している。`vscode`では`ssh`で接続できる機能や`docker`上にもアクセスできる機能が存在するので、GUIを起動できないラズパイやJetsonのサーバーモード等でも問題なく運用できる。その他にもgitの履歴や差分表示のGUIツールがあると考えられるが、それらは自分で調べて導入してほしい。ただし、稀にそれらのGUIのツールが使用できない環境が存在するしうると考えられるので、説明したCUIでの使用方法は無駄にはならないと信じている。

## 6. ブランチの作成と切り替え

### 6.1. ブランチの説明とブランチの追加そして削除
 　履歴の表示の節でブランチの話を行った。
 ブランチの説明を行うために以下を見て欲しい。
```bash
$ git log --oneline
7517781 (HEAD -> master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
e1475de [Add] test5.txt
575b2b5 [Add] test4.txt
1ea6ace [Add] hoge_dir
2a94b90 [Update] test.txt
44b500f first commit
```
 　上記は履歴の表示の節で使用したコミットの履歴である。ここでは`master`はブランチで、`HEAD`は現在のブランチを指すポインタである。  
 　ブランチ(branch)とは直訳すると枝のことで、gitの履歴管理が木構造の概念で行われていることからそう呼ばれる。枝なので複数のブランチに枝分かれすることができ、またソレらを切り替えることができる。  
 　例えば新しいブランチを作成してみて、ブランチの一覧を表示する。`git branch`コマンドでブランチの一覧を表示することができ、同じコマンドで任意のブランチ名を後に付けると新しいブランチを作成することができる（例：`git branch HOGE`）。
```bash
$ git branch
* master

$ git branch HOGE
$ git branch
  HOGE
* master
```
 　アスタリスクが前についている方が現在`HEAD`が指しているブランチである。なお以下のように既に存在しているブランチを指定するとエラーが出る。
```bash
$ git branch master
fatal: A branch named 'master' already exists.
```
 　ブランチは枝で枝分かれが可能という概念を前述したが、以下のコマンドで確認できる
```bash
$ git log --oneline
7517781 (HEAD -> master, HOGE) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　上記の(HEAD -> master, HOGE)の部分を見ると`HOGE`が追加されていることが確認できるだろう。これは `master`というブランチがあるコミットオブジェクトを起点として枝分かれが行われているからである。  

 このように作成したブランチは以下のようにして削除することができる。
```bash
$ git branch -d HOGE
$ git branch
* master
```

### 6.2. ブランチの切り替え

 　次にブランチの切り替えに関しての説明を行う。現在のブランチ一覧は以下である。  
```bash
$ git branch
* master
```
 　まず準備として先程と同じ名前のHOGEブランチを作成する。
```bash
$ git branch HOGE
$ git branch
HOGE 
* master
```

 　ブランチの切り替えを行うコマンドは`git checkout`である。以下のようにして切り替えたいブランチを指定して切り替える。
```bash
$ git checkout HOGE
$ git branch
* HOGE
  master

$ git log --oneline
7517781 (HEAD -> HOGE, master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　上記を見ると、checkout した後は HOGE の方に現在のブランチを指すアスタリスクがついていることが確認できるので、ブランチの切り替えが行われたことが分かる。`git log` でも `master` の前に`HOGE`がついていることが確認できるので、現在参照しているブランチが変わっていることが分かる。また現在のブランチは以下のように`git status`コマンドでも確認することができる。
```bash
$ git status
ブランチ HOGE
nothing to commit, working tree clean
```

### 6.3. ブランチの切り替え（深堀り）

 　さてコレだけではブランチの便利さが分からないので、さらに深堀りしていく。現在のブランチ一覧は以下で HEAD が指しているブランチは HOGE であるとする。  
```bash
$ git branch
* HOGE
  master
```
 　その状態でファイルの追加とコミットを実行する。  
```bash
$ echo "gggg" >> test6.txt
$ git add test6.txt 
$ git commit -m "[Add] test6.txt"
[HOGE bbbcdce] [Add] test6.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test6.txt
```
 　無事追加された。この状態で履歴を確認してみよう。  
```bash
$ git log --oneline
bbbcdce (HEAD -> HOGE) [Add] test6.txt
7517781 (master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
```
 　現在のブランチである HOGE ブランチが master ブランチに先行して、一つ上のコミットオブジェクトに移動していることが分かるだろう。この HOGE が指しているコミットオブジェクトでは、test6.txt というモノが追加された。それでは master ブランチに切り替えてみよう。
```bash
$ ls test6.txt
test6.txt
$ git checkout master
Switched to branch 'master'
$ git branch
  HOGE
* master

$ ls test6.txt
ls: 'test6.txt' にアクセスできません: そのようなファイルやディレクトリはありません

$ git log --oneline
7517781 (HEAD -> master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　master ブランチに移動して test6.txt を探してみると存在しない。そしてその test6.txt を追加したコミットオブジェクトを探してみると存在しない。削除されてしまったのだろうか。
  いや違う。以下のコマンドで全てのブランチの履歴を表示する。
```bash
$ git log --oneline --all
bbbcdce (HOGE) [Add] test6.txt
7517781 (HEAD -> master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　存在が確認できた。`git log --oneline`だけでは未来の履歴に存在するコミットオブジェクトを確認できないようだ。また、HEADが指している対象が HOGE ブランチ から master ブランチ に切り替わっていることが分かる。もう一度、HOGE ブランチに切り替えてみる。
```bash
$ git checkout HOGE
Switched to branch 'HOGE'
$ git branch
* HOGE
  master
$ ls test6.txt 
test6.txt
$ git log --oneline
bbbcdce (HEAD -> HOGE) [Add] test6.txt
7517781 (master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　コチラでは問題なく test6.txt が存在していることが分かる。また過去のコミットオブジェクト上にあるブランチは`git log --oneline`だけで存在が確認できるようだ。  

### 6.4. ブランチの切り替え（さらに深堀り）

 　さらにブランチの便利さを体験してもらうために以下のようにして新しいブランチを master ブランチから作成して test7.txt を追加してコミットしてみる。まず、master ブランチから新しいブランチを作成する。ここで `git checkout -b FUGA` というコマンドを用いているが、これは `git branch FUGA + git checkout FUGA` をワンライナーでやってくれるので、筆者は通常コレを用いている。  
```bash
$ git checkout master
Switched to branch 'master'
$ git checkout -b FUGA
Switched to a new branch 'FUGA'
$ git branch
* FUGA
  HOGE
  master
$ git log --oneline --all
bbbcdce (HOGE) [Add] test6.txt
7517781 (HEAD -> FUGA, master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　上記を見てみると master ブランチから `FUGA` というブランチが枝分かれされている。続ける。FUGAブランチで test7.txt を追加してコミットを行う。そして`--all`オプションをつけて履歴を確認する。  

```bash
$ echo "hhhh" >> test7.txt
$ git add test7.txt
$ git commit -m "[Add] test7.txt"
[FUGA e756f92] [Add] test7.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test7.txt
$ git log --oneline --all
e756f92 (HEAD -> FUGA) [Add] test7.txt
bbbcdce (HOGE) [Add] test6.txt
7517781 (master) [Update] add do not include 'fuga' into .gitignore.
bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
71f617c [Add] .gitignore
...
```
 　FUGAブランチ が HOGE ブランチの上に来ている。またソレらの関係が分かりにくい。そこで、以下のように`--graph`オプションを付けて履歴を再度確認する。  
```bash
$ git log --oneline --all --graph
* e756f92 (HEAD -> FUGA) [Add] test7.txt
| * bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 (master) [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 　`--graph`オプションを付けたことでコミットオブジェクト間の関係性が分かりやすくなった。このコマンドにおけるアスタリスクの意味は木構造におけるノードの概念と同じである。上記を見ると、master ブランチから枝分かれした HOGE ブランチと FUGA ブランチの関係性が分かるだろう。  

 　そして、それぞれのブランチに切り替えを行い test6.txt と test7.txt の存在が独立していることが以下のようにして確認できるだろう。  

```bash
$ ls test7.txt
test7.txt
$ git checkout HOGE
Switched to branch 'HOGE'
$ ls test7.txt
ls: 'test7.txt' にアクセスできません: そのようなファイルやディレクトリはありません
$ ls test6.txt
test6.txt
$ git checkout FUGA
Switched to branch 'FUGA'
$ ls test6.txt
ls: 'test6.txt' にアクセスできません: そのようなファイルやディレクトリはありません
```
 　今回はファイルの追加のみを行ったが、ファイルの書き換えに関しても同じことができる。  

 　このようにブランチという概念とブランチの切り替えはコードの変更やファイルの追加を柔軟に切り替えられることができる。これが示すのは開発用のブランチと配布用のブランチを分けたり、特定の環境(Jetsonやラズパイなど)の時のみ限定のライブラリを使用するなどの用途で使えるということである。また、実験中に一時的にコードを変更するなどの用途にも使用できる。

## 7. マージ

### 7.1. マージの基礎

 　この章ではマージという概念に関しての説明を行うが、その説明の前にマージに関連したブランチについて説明を行う。  
 　さてブランチという概念について学んだが、ブランチには以下の３つのような基本ブランチとして定義されたモノがある。  
 - メインブランチ
 - 作業用ブランチ
 - ベースブランチ

 メインブランチは任意のブランチをそのように定義付けしてもよいが慣例として、`master`や`main`などの名前のブランチが選ばれる。そして作業用ブランチはブランチの章で取り扱った`HOGE`や`FUGA`などのファイルの追加や編集などを行ったブランチである。そしてベースブランチとは作業用ブランチが枝分かれをする際に**ベース**となったブランチのことを指す。  
 　gitを用いたプロジェクトの進め方としては、基本的にメインブランチ上でコミットを直接行うのでは無く、作業用ブランチを作成して、そのブランチ上でコミットを行うべきである。これは何時でも編集前のブランチに戻ることができるという観点から従った方がよいと筆者は考えている。それでは、作業用ブランチでの作業が完了してメインブランチに変更内容を取り込みたい時はどうすればよいだろうか？そのような疑問を解決する概念が**マージ**である。  

 　マージとは直訳すると'合流'である。これは枝分かれしたブランチを一つのブランチに合流させるマージの機能をそのまま表している。実際にマージを行ってみよう。前提として、ここまでのチュートリアルを終えていると以下のような履歴を持ったリポジトリを持っている。  
```bash
$ git log --oneline --all --graph
* e756f92 (HEAD -> FUGA) [Add] test7.txt
| * bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 (master) [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
```
 　ここでメインブランチである master ブランチが作業ブランチ HOGE の変更内容を取り込みたい場合以下のように`git merge`コマンドを用いて行う。  
```bash
$ git checkout master
Switched to branch 'master'
$ git merge HOGE
Updating 7517781..bbbcdce
Fast-forward
 test6.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test6.txt
$ git log --oneline --all --graph
* e756f92 (FUGA) [Add] test7.txt
| * bbbcdce (HEAD -> master, HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
```
 　上記のようにマージを行った後に履歴を確認すると master と HOGE が同じコミットオブジェクトの位置にいることが分かるだろう。これにより変更内容のマージ（合流）ができた。  

 　次に 作業用ブランチ 'FUGA' のブランチもメインブランチに取り込んでみよう。このマージは先程の場合と異なるマージとなるので'-m'オプションとメッセージを追加して行う。このオプションが無くともマージ自体は行われるがエディタが起動してしまって面倒なので付けた方がよい。  
```bash
$ git log --oneline --all --graph
* e756f92 (FUGA) [Add] test7.txt
| * bbbcdce (HEAD -> master, HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...

$ git merge FUGA -m "Merge branch 'FUGA'"
Merge made by the 'recursive' strategy.
 test7.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test7.txt
$ git log --oneline --all --graph
*   926d8cc (HEAD -> master) Merge branch 'FUGA'
|\  
| * e756f92 (FUGA) [Add] test7.txt
* | bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 作業用ブランチ FUGA の上に新しいコミットオブジェクトが追加され、ソコに master ブランチが移動していることが分かる。HOGE をマージした時と履歴のグラフが異なることに気づくだろう。ここで、どのような差分になっているのか確認する。

```bash
$ git diff HEAD~1
diff --git a/test7.txt b/test7.txt
new file mode 100644
index 0000000..f3e8683
--- /dev/null
+++ b/test7.txt
@@ -0,0 +1 @@
+hhhh
```
 　この差分は`git diff e7517781 e75692`の内容と一緒になる。つまり単純に test7.txt というファイルが追加されたことを表しているだけだ。
 　この HOGE ブランチ と FUGA ブランチのマージにおいての違いの要因は、マージが行われた時の master ブランチの位置である。以下の履歴は それぞれのマージが行われる前の`--all`オプションを外した履歴である（ここは実行のマネはしない）
```bash
$ git checkout HOGE
$ git log --oneline --graph
* bbbcdce (HEAD -> HOGE) [Add] test6.txt
* 7517781 (master) [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 　ここでは master ブランチの位置が HOGE ブランチより過去に存在している。そしてそのまま HOGE ブランチをマージした後に FUGA ブランチをマージする前の`--all`オプションを外した履歴は以下である（これも実行はマネしない）
```bash
$ git checkout FUGA
$ git log --oneline --graph
* e756f92 (HEAD -> FUGA) [Add] test7.txt
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 　上記を見ると master ブランチが FUGA より過去に存在しないことが分かる。何処にいるかというと`--all`オプションを付けて探すと...(これもマネしない)
```bash
$ git log --oneline --all --graph
* e756f92 (FUGA) [Add] test7.txt
| * bbbcdce (HEAD -> master, HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore`
...
```
 　このようにマージする側(HOGEやFUGA)から見てマージされる側（ココではmaster）が過去に存在する場合のマージと存在しない場合のマージでコミット履歴が変わる。前者を `fast-forward merge` と呼び、後者を `non-fast-forward merge` と呼ぶ。自分一人で開発を行っている際は基本的に`fast-foward merge`を用いることになるだろうが、一方で多人数での開発の場合は`non-fast-forward merge` を用いることになるだろう。  

### 7.2. コンフリクト

 　コンフリクトとは直訳すると衝突である。gitにおいてこの概念はマージを実行した際に変更が重複していることなどが原因でマージできない、つまり変更が衝突することを表している。  
 　さて実際にコンフリクトを発生させてどのような状況になるかを見ていこう。まず今の履歴の状態は以下のようになっているとする。  
```bash
$ git log --oneline --all --graph
 *   926d8cc (HEAD -> master) Merge branch 'FUGA'
|\  
| * e756f92 (FUGA) [Add] test7.txt
* | bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 　今回はHOGE2ブランチというブランチを作成して、そのブランチにFUGA2ブランチというブランチをマージさせる方式で説明を行う。まず、HOGE2ブランチを作成して、そこに test8.txt というファイルを追加してコミットする。  
```bash
$ git checkout -b HOGE2
Switched to a new branch 'HOGE2'
$ echo "aaaa" >> test8.txt
$ git add test8.txt
$ git commit -m "[Add] test8.txt"
[HOGE2 11c5154] [Add] test8.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test8.txt
```
 　次にHOGE2ブランチからFUGA2ブランチを枝分かれさせて、test8.txtのファイルの2行目に'bbbb'という文字列を追加してコミットする。  
```bash
$ git checkout -b FUGA2
Switched to a new branch 'FUGA2'
$ echo "bbbb" >> test8.txt
$ git add test8.txt
$ git commit -m "[Add] 'bbbb' into test8.txt 2nd line"
[FUGA2 e20bd6e] [Add] test8.txt
 1 file changed, 1 insertion(+)
```
 　つづいて、HOGE2ブランチに戻って、test8.txtのファイルの2行目に'cccc'という文字列を追加してコミットする。  
```bash
$ git checkout HOGE2
Switched to branch 'HOGE2'
$ echo "cccc" >> test8.txt
$ git add test8.txt 
$ git commit -m "[Add] 'cccc' into test8.txt 2nd line"
[HOGE2 383d819] [Add] 'cccc' into test8.txt 2nd line
 1 file changed, 1 insertion(+)
```
 　この状態における履歴を見てみよう。  
```bash
$ git log --oneline --all --graph
* bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
| * 383d819 (HEAD -> HOGE2) [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
|\  
| * e756f92 (FUGA) [Add] test7.txt
* | bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
...
```
 　ここでは HOGE2 と FUGA2 が分かれていることが見て取れる。つまり HOGE2 と FUGA2 の２つのブランチでは test8.txt は 'bbbb'という文字列と'cccc'と文字列のどちらか一つしか含んでいない状態である。

 　この状態で、HOGE2にFUGA2の変更を取り組むためのマージを実行してみよう。  
```bash
$ git merge FUGA2
Auto-merging test8.txt
CONFLICT (content): Merge conflict in test8.txt
Automatic merge failed; fix conflicts and then commit the result.
```
 　コンフリクト(CONFLICT)が発生した。状態を確認してみよう。
```bash
$ git status
ブランチ HOGE2
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   test8.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
 　どうやら test8.txt に複数の変更が含まれているようだ。これから、このコンフリクトの解決方法を解説していくが、その前にこのマージを取り消してコンフリクトが発生する前の状態に戻す方法を記載しておくい。以下の方法で今回のマージを一旦取り消すことができる。  
```bash
$ git merge --abort
$ git status
ブランチ HOGE2
nothing to commit, working tree clean
```
 　さてコンフリクトの解消を解説するためにもう一度マージをする。  
```bash
$ git merge FUGA2
Auto-merging test8.txt
CONFLICT (content): Merge conflict in test8.txt
Automatic merge failed; fix conflicts and then commit the result.
```
 　どのような変更になっている確認するために test8.txt の中身を見てみよう。  
```bash
$ cat test8.txt
aaaa
<<<<<<< HEAD
cccc
=======
bbbb
>>>>>>> FUGA2
```
 　上記が示しているのはイコール(=)より上の部分が HEAD(HOGE2)によって2行目に追加した部分、下の部分がFUGA2によって２行目に追加した部分である。これを解決するには、ドチラか片方の変更のみを取り組む、もしくは、両方の変更を取り組むなどの方法がある。 
 　今回は FUGA2 の変更を取り組みたいとすると以下のように行の削除を行ってファイルをキレイにする。  
```bash
$ code test8.txt # 任意のエディタで以下のようになるように修正する
$ cat test8.txt
aaaa
bbbb
```
 　状態を見てみると、全てのコンフリクトが解決された旨が表記される。一方で、まだコミットが終わっていないことが表記されている。
```bash
$ git status
ブランチ HOGE2
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

コミット予定の変更点:

	modified:   test8.txt
```
 　以下のコマンドでマージを完了させる。エディタが起動してしまうのが嫌なので以下の方式を採っているが、気にならない場合は`git merge --continue`だけでよい。  
```bash
$ git -c core.editor=/bin/true merge --continue
[HOGE2 e2c3ec0] Merge branch 'FUGA2' into HOGE2
```
 　今の履歴は以下のようになっている。  
```bash
$ git log --oneline --all --graph
*   e2c3ec0 (HEAD -> HOGE2) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
...
```
 　そして差分が以下である。  

```bash
$ git diff HEAD~
diff --git a/test8.txt b/test8.txt
index 373a425..64e09ee 100644
--- a/test8.txt
+++ b/test8.txt
@@ -1,2 +1,2 @@
 aaaa
-cccc
+bbbb
```
 　つまりマージをするためにHOGE2から'cccc'の文字列が削除して'FUGA2'での'bbbb'の文字列を追加しコミットオブジェクトを作成してマージの整合性を採っているのである。  

 　このようにしてコンフリクトを解消する。今回はvscodeのエディタを用いてコンフリクトの解消を行ったがemacsやvim等を用いることも可能である。一方でvscodeではマージエディタという機能があるのでより簡単にマージを実行することができるのでオススメである。  
 　また今回のコンフリクトは非常に簡単な内容であった。実際には大量のコンフリクトや今回の修正と異なる修正の仕方が必要になってくるだろう。そのような場合は自分で調べて解決してほしい。  

## 8. コミットの巻き戻し

### 8.1. 巻き戻しの基礎

 　gitはバージョン管理ツールであるので、もちろんコミットの巻き戻しが行える。まず現在の履歴を表示する。
```bash
*   e2c3ec0 (HEAD -> HOGE2) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
...
```
 　この状態から HOGE2 を masterブランチまで巻き戻していく。まず、保険として何時でも戻れるようにtempブランチを作成しておく。
```bash
$ git branch temp
$ git log --oneline --all --graph
*   e2c3ec0 (HEAD -> HOGE2, temp) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
...
```
 　巻き戻しを行うコマンド(git reset)の使い方は以下である。  
```bash
$ git reset HEAD~
Unstaged changes after reset:
M	test8.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git log --oneline --all --graph
*   e2c3ec0 (temp) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 (HEAD -> HOGE2) [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
...
```
 　上記を見ると、HOGE2ブランチがFUGA2ブランチを取り込む前のコミットオブジェクトに巻き戻っていることが分かる。また、状態を見てみると test8.txt の変更が残っていることが確認できる。中身を見てみるとFUGA2を取り込んだ時の変更が、そのまま残っていることが分かる（FUGA2のマージで文字列ccccが文字列bbbbに変更されたこと）。
```bash
$ git status
ブランチ HOGE2
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   test8.txt
$ cat test8.txt 
aaaa
bbbb
```
 　この変更を元に戻すには以下のように`git checkout`コマンドを用いることで元に戻すことができる。
```bash
$ git checkout test8.txt
$ cat test8.txt
aaaa
cccc
$ git status
ブランチ HOGE2
nothing to commit, working tree clean
```

 　さて`git reset`の`HEAD~`には数字を指定することが出来る。以下のように２を指定すると２個前に戻ることができる。  
```bash
$ git reset HEAD~2
```
 　履歴を確認すると以下のようになっている。
```bash
$ git log --oneline --all --graph
*   e2c3ec0 (temp) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (HEAD -> HOGE2, master) Merge branch 'FUGA'
```
 　先程HOGE2ブランチがあった`383d819`のコミットオブジェクトから２個前の`926d8cc`コミットオブジェクトに戻っていることが分かる。  
 　状態も見てみよう
```bash
$ git status
ブランチ HOGE2
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test8.txt

nothing added to commit but untracked files present (use "git add" to track)
``` 
 　test8.txtの変更が残っていることが分かる。しかし、先程と違い`modified`がついていない。これは`11c5154`で追加されたファイルなので現在のブランチでは存在しないファイルだからである。
 　中身も確認してみよう。
```bash
$ cat test8.txt
aaaa
cccc
```
 　中身を確認してみると文字列’aaaa'を追加した`11c5154`のコミットと文字列'cccc'を追加した`383d819`コミットの２つの変更がまるごと残っていることが分かる。  
 　この変更を削除して状態をキレイにするために先程と同じように`git checkout`を用いてみよう。
```bash
$ git checkout test8.txt
error: pathspec 'test8.txt' did not match any file(s) known to git.
$ git status
ブランチ HOGE2
追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	test8.txt

nothing added to commit but untracked files present (use "git add" to track)
```
 　エラーが発生した。これは test8.txt が未来のコミットで追加されたファイルであるため、現在のコミットオブジェクトでの状態が分からないからである。つまり`git checkout FILE`はあるファイルを今のコミットにおける状態に戻しているのである。このような場合では普通に`rm`コマンド等を用いて削除するのがよい。  

```bash
$ rm test8.txt
$ git status
ブランチ HOGE2
nothing to commit, working tree clean
```

### 8.2. 巻き戻し(git reset オプション)

 　さてHOGE2をmasterまで巻き戻してきたが更に戻してみよう。今の履歴は以下のようになっている（今回は`--all`オプションを用いない）
```bash
*   926d8cc (HEAD -> HOGE2, master) Merge branch 'FUGA'
|\  
| * e756f92 (FUGA) [Add] test7.txt
* | bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
...
```
 　ここから`7517781`のコミットオブジェクトに巻き戻す。今回の巻き戻しでは、`--hard`オプションを追加する。
```bash
$ git reset HEAD~2 --hard
$ git log --oneline --graph
* 7517781 (HEAD -> HOGE2) [Update] add do not include 'fuga' into .gitignore.
...
$ git status
ブランチ HOGE2
nothing to commit, working tree clean
```
 　さて無事に巻き戻ったが、HOGEとFUGAで追加された test6.txt とtest7.txt がキレイに無くなっていることが分かる（オプションを付けていない時は残っていた）。実は`git reset`には以下の３つのオプションが存在し、デフォルトでは`--mixed`が用いられている。  
 1. --mixed
    - コミットを巻き戻す。ファイルの変更は残す。デフォルトはコレ
 2. --hard
    - コミットもファイルの変更も巻き戻す
 3. --soft
    - コミットを巻き戻す。ファイルの変更とステージング(git commitを行う前のgit addしたファイルがある場所)は残す。

 　`--soft`オプションの使い道は、ファイル等を変更している時にブランチを巻き戻したい状況とかだろうか？（それでも`git stash`とかの方法がありそうだが...）。筆者の場合は`--mixed`と`--hard`しか使っていないが、知識として`--soft`も知っておこう。  


## 9. ブランチの強制削除

 　この章ではブランチの強制削除に関して取り扱う。本来はブランチの作成や切り替えの章で説明すべきであるが、話の流れ上コチラに記載することが自然であると考えコチラに記載する。  
 　今のgit履歴は以下のようになっている。
```bash
$ git log --oneline --graph --all
*   e2c3ec0 (temp) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (master) Merge branch 'FUGA'
|\  
| * e756f92 (FUGA) [Add] test7.txt
* | bbbcdce (HOGE) [Add] test6.txt
|/  
* 7517781 (HEAD -> HOGE2) [Update] add do not include 'fuga' into .gitignore.
* bef3708 [Update] gitの追跡の特例としてtest1.jpgを追跡対象に追加
* 71f617c [Add] .gitignore
* e1475de [Add] test5.txt
* 575b2b5 [Add] test4.txt
* 1ea6ace [Add] hoge_dir
* 2a94b90 [Update] test.txt
* 44b500f first commit
```
 　ここで master 以外のブランチを全て削除する。削除の方法はブランチの作成と切り替えの章で記載した`git branch -d`コマンドを用いる。`git checkout`コマンドで master に移動してから削除を行う。  
```bash
$ git checkout master
Switched to branch 'master'
$ git branch -d HOGE
Deleted branch HOGE (was bbbcdce).
$ git branch -d FUGA
Deleted branch FUGA (was e756f92).
$ git branch -d HOGE2
Deleted branch HOGE2 (was 7517781).
$ git branch -d FUGA2
error: The branch 'FUGA2' is not fully merged.
If you are sure you want to delete it, run 'git branch -D FUGA2'.
```
 　順調に削除が行われていたが FUGA2 のブランチを削除しようとするとエラーが発生する。ここでもう一度、git 履歴を確認する。  
```bash
$ git log --oneline --graph --all
*   e2c3ec0 (temp) Merge branch 'FUGA2' into HOGE2
|\  
| * bf054d1 (FUGA2) [Add] 'bbbb' into test8.txt 2nd line
* | 383d819 [Add] 'cccc' into test8.txt 2nd line
|/  
* 11c5154 [Add] test8.txt
*   926d8cc (HEAD -> master) Merge branch 'FUGA'
|\  
| * e756f92 [Add] test7.txt
* | bbbcdce [Add] test6.txt
|/  
* 7517781 [Update] add do not include 'fuga' into .gitignore.
...
```
 　FUGA2 ブランチは現在のブランチである master ブランチから見て過去に存在しない。マージするか削除する必要があるが、削除したい場合は強制的に削除する必要がある。
 　ブランチの強制削除は以下のようにオプション`-d`を大文字にするだけである。  
```bash
$ git branch -D FUGA2
Deleted branch FUGA2 (was bf054d1).
$ git branch -D temp
Deleted branch temp (was e2c3ec0).

$ git branch
* master
```
 　これでブランチが master だけになった。

## 10. Tips

### 10.1. コミット後の修正（コメントを修正したい or ファイルの追加忘れ or ファイルの変更点の保存忘れ）

 　節のタイトルの通り、以下の３点のようなことは頻繁に起きうる事象である。
 - コメントを修正したい
 - ファイルの追加忘れ
 - ファイルの変更点の保存忘れ

 　このような場合は以下のように`--amend`オプションを用いることで対応することができる
```bash
# コメントの修正
$ git commit --amend -m "New Comment"

# ファイルの追加忘れや変更の保存忘れ
# --no-editオプションを更に付けることでコメントは修正しないことができる
$ git commit --amend --no-edit
```