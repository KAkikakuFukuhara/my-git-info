# git+github

- [1. 序文](#1-序文)
- [2. githubに関して](#2-githubに関して)
- [3. リポジトリに関して](#3-リポジトリに関して)
- [4. 初期設定](#4-初期設定)
  - [4.1. author設定](#41-author設定)
  - [4.2. 公開鍵設定](#42-公開鍵設定)
- [5. リポジトリの連携](#5-リポジトリの連携)
  - [5.1. ローカルリポジトリの作成](#51-ローカルリポジトリの作成)
  - [5.2. リモートリポジトリの作成と登録とアップロード](#52-リモートリポジトリの作成と登録とアップロード)
    - [5.2.1. リモートリポジトリの作成](#521-リモートリポジトリの作成)
    - [5.2.2. リモートリポジトリの登録](#522-リモートリポジトリの登録)
    - [5.2.3. リモートリポジトリへのアップロード](#523-リモートリポジトリへのアップロード)
  - [5.3. READMEの追加](#53-readmeの追加)
    - [5.3.1. READMEの書くことに関して](#531-readmeの書くことに関して)
  - [5.4. ライセンスの追加](#54-ライセンスの追加)
  - [5.5. fetch](#55-fetch)
- [6. リモートリポジトリのダウンロード(git clone)](#6-リモートリポジトリのダウンロードgit-clone)
- [7. 作業用ブランチのリモートリポジトリへのアップロード](#7-作業用ブランチのリモートリポジトリへのアップロード)
- [8. リモートリポジトリ上のブランチを削除](#8-リモートリポジトリ上のブランチを削除)
- [9. 複数人での開発](#9-複数人での開発)


## 1. 序文

 　本稿では`git`のチュートリアルを終えた読者を対象としている。まだの読者は`git`のチュートリアルを読むこと。  
 　さて、本稿では`git`および`github`を用いてバージョン管理に関して記載する。基本的に実践しながら使い方を学んでいくスタイルで進めていく。  

## 2. githubに関して

 　githubにはいろいろな機能があるが、基本的には`git`で作成したディレクトリをweb上で管理してくれるバージョン管理のwebツールである。ここにアップロードしておくとローカル側でプログラムを失ってもweb上にバックアップが存在するなど非常に便利なツールである。管理の都合上ローカル側の変更をgithub上に定期的にアップロードする必要がある.  

## 3. リポジトリに関して

 　リポジトリは`git`のチュートリアルで少し触れたが、履歴が保存してある場所という概念である。このリポジトリは大きく分けて**ローカルリポジトリ**と**リモートリポジトリ**に分かれる。  
 　ローカルリポジトリはその名の通り自分のコンピュータ側、つまりローカルに存在するリポジトリである。今まで`git`で履歴を保存してきた場所はローカルリポジトリである。一方でリモートリポジトリとは遠隔（リモート）にあるリポジトリであり、`github`に限らずにオンプレミスにサーバーを用意していて場所を提供するようなモノも含める（コレはgitlabという）。基本的なリポジトリ運用の流れは、管理したいディレクトリを`git`を用いてローカルリポジトリに保存して、リモートリポジトリにアップロードする、というものである。  

## 4. 初期設定

 　さて、githubのサービスを受けるには会員登録が必要である。ウィルスメール等が来る可能性を考慮にいれて会社のPCではなく、Googleのアカウントを作成して、そのメールアドレスを使うように一個間を挟んだ方がよいだろう（筆者の勝手な対応であるので確認した方がよいかもしれない）。  

### 4.1. author設定

 　Googleアカウントの作成とgithubの会員登録が終わったとする。まずローカル側にある`git`のユーザー名とメールアドレスを変えよう。`git`チュートリアルでは以下のように設定していたモノを正式なモノに変更するのだ。  
```console
user:~$ git config --global user.name "HOGE FUGA"
user:~$ git config --global user.email "HOGE@FUGA.test0527.com"
```
 　セキュリティ上の都合によりgithubに登録したメールアドレスを使用すると他人に登録しているメールアドレスが筒抜けになってしまうのでよろしくない。よって以下のURLの方法を用いてメールアドレスを再設定することで登録しているメールアドレスが表示されなくなるので、こちらで登録を行う。  
- [https://snowtree-injune.com/2021/08/17/mail-git002/](https://snowtree-injune.com/2021/08/17/mail-git002/)

 　以下の方法で author を登録する.  
 　idは８桁の番号でusernameはgithubのユーザー名である。これらの記載場所は上記の参照URLに記載場所が示されている  

```console
user:~/$ git config --global user.name "Taro Yamada"
user:~/$ git config --global user.email "id+username@@users.noreply.github.com" #推奨
```

### 4.2. 公開鍵設定

 　次に公開鍵登録を行う。公開鍵を登録しなくてもダウンロードやアップロードが可能であるが、毎回ログイン作業が発生してしまうので避けたほうがよい。  
 　公開鍵の作成＆設置は以下のURLを参考にして欲しい。  
- [https://qiita.com/shizuma/items/2b2f873a0034839e47ce](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)  

## 5. リポジトリの連携

 　さて、実際にローカルリポジトリを作成してリモートリポジトリと連携させてみよう。  

### 5.1. ローカルリポジトリの作成

 　適当に`sample_app`というディレクトリを作成して`test.py`を追加してローカルリポジトリを作成しよう。  
```console
user:~/$ mkdir sample_app
user:~/$ cd sample_app/
user:~/sample_app$ echo "aaaa" >> test.txt
user:~/sample_app$ git init
user:~/sample_app$ git add test.txt 
user:~/sample_app$ git commit -m "[Add] first commit"
[master (root-commit) 8fc057f] [Add] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
user:~/sample_app$ git status
ブランチ master
nothing to commit, working tree clean
```

### 5.2. リモートリポジトリの作成と登録とアップロード

#### 5.2.1. リモートリポジトリの作成

 　リモートリポジトリを作成する。作成の方法に関してのGUI部分は以下のURLの4番を参考にすること。  
  - https://qiita.com/sodaihirai/items/caf8d39d314fa53db4db

 **注意点**
 1. Ownerのところのドロップダウンリストはアップロード先を選択するモノである。  
    1. 個人的なプログラム（勉強用とか）は自分のところに、共有するプログラムは NT2025を選択すること  
 2. リポジトリは Public ではなく Private にすること。  
    1. Public は全世界に公開設定なので何らかの目的が無い場合以外は Private にすること  
 3. 他の部分は弄らなくよい。  
    1. `README`とか`LICENSE`とか`.gitignore`とかのチェックボックスやドロップダウンリストなど  

   リポジトリを作成した後は上にあるURL先の5番のGUIが表示される。  
   5番のGUIでURLをコピーする。  
 - URLをコピーする際は https ではなく ssh を使用すること  
    - httpsにすると毎回ログインが必要になるため  

#### 5.2.2. リモートリポジトリの登録

　以下のコマンドでローカル側にリモートリポジトリの場所を登録する。  
  ここで`origin`はURLを指すラベルなので自由に命名できるが慣例としてOriginを用いることになっている  
```console
# 以下で登録
user:~/sample_app$ git remote add origin <URL>

# 以下で登録されているか確認できる。
user:~/sample_app$ git remote show origin
# 間違えたら削除して再度登録
user:~/sample_app$ git remote remove origin
```

#### 5.2.3. リモートリポジトリへのアップロード

　リモートリポジトリへのアップロードを行う際には以下のように行う。  
　ここで`master`はブランチ名である。他にもアップロードしたいブランチがあれば`master`のところを変えればよい。  
```console
   git push -u origin master
```

 　リモートリポジトリを登録してアップロードするとローカルリポジトリ上で履歴を確認すると以下のように表示される。
```console
user:~/sample_app$ git log --oneline --all --graph
* 8fc057f (HEAD -> master, origin/master) [Add] first commit
```
 　ここで`origin/master`は、リモートリポジトリ上にアップロードした`master`ブランチを指す。なお注意してほしいのが自動で同期を採っているわけではないので、最後にローカル側がリモート側に同期を行った際の位置である点に注意をする。同期を取る方法は後述する。

### 5.3. READMEの追加

 　githubに`README.md`というファイルを登録すると、そのリモートリポジトリを開いた時に表示されるテキストが`README.md`の内容を反映したモノになる。試しに以下のように記載した`README.md`を作成して作業用ブランチを切ってファイルを追加して、masterブランチにマージしてコミットを行いプッシュしてアップロードする。  
```markdown
# README

ここに書いた内容が表示される。  
改行は半角スペースふたつである。  

```
```console
user:~/sample_app$ git checkout -b feature/add-README.md
user:~/sample_app$ git add README.md
user:~/sample_app$ git commit -m "[Add] README.md"
user:~/sample_app$ git checkout master
user:~/sample_app$ git merge feature/add-README.md
Updating 8fc057f..48def68
Fast-forward
 README.md | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 README.md
user:~/sample_app$ git log --oneline --all --graph
* 48def68 (HEAD -> master, feature/add-README.md) [Add] README.md
* 8fc057f (origin/master) [Add] first commit

user:~/sample_app$ git push origin master
user:~/sample_app$ git log --oneline --all --graph
* 48def68 (HEAD -> master, origin/master, feature/add-README.md) [Add] README.md
* 8fc057f [Add] first commit

# 無事にプッシュされていることを確認できたので作業用ブランチを消す。
user:~/sample_app$ git branch -d feature/add-README.md
```
 　リモートリポジト側のブラウザをF5キーなどで再度開くと反映されていることが分かるだろう。  
 　ここで紹介した`md`拡張子を用いたファイルをMarkdown(マークダウン)ファイルと呼ぶ。Markdown（マークダウン）ファイルの書き方は`Markdown 書き方`で調べて欲しい シャープ（`#`）の数で階層構造を構成していることだけ分かればとりあえず困らないだろう。  

#### 5.3.1. READMEの書くことに関して

 基本的に自由だが、最低でも以下の点は書いてほしい。  
 - 背景目的
 - 実行環境(pythonのバージョン、ROSのバージョン、gccのバージョンなどなど)
 - どのようなライブラリが使われているか(requirements.txtやpyproject.toml等があれば必要ない)
 - 構築方法（インストールなど）
 - 実行方法（どのようなファイルを使うことを目的としているのか）
   - 例えば '〇〇をするためを以下で実行する'など
     - `python src/test.py

 これは他人にわかりやすくするため以外に**未来の自分**に対しても分かりやすくするために必要である。  

### 5.4. ライセンスの追加

 　世の中のOSSにはライセンスが存在する。このようなライセンスは`github`上で作成することができる。試しに作成してみよう。ライセンスの追加に関しては以下を参考にしてほしい。選択するライセンスは今回は`MIT`として、また`commit directly...`でコミットしてほしい。  
- https://docs.github.com/ja/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository

 　ライセンスの追加が終わればリモートリポジトリ上に`LICENSE`というファイルが追加されていることが分かるだろう。  

### 5.5. fetch

 　さて今の状態で以下のようにコミットオブジェクトを更新してアップロードしよう。アップロードする方法は`README.md`の時と同じで、以下のようにしてプッシュを行う。  
```console
user:~/sample_app$ git checkout -b feature/test2.txt
Switched to a new branch 'feature/test2.txt'
user:~/sample_app$ echo "bbbb" >> test2.txt
user:~/sample_app$ git add test2.txt
user:~/sample_app$ git commit -m "[Add] test2.txt"
[feature/test2.txt bdf4c14] [Add] test2.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test2.txt
user:~/sample_app$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
user:~/sample_app$ git merge feature/test2.txt 
Updating 48def68..bdf4c14
Fast-forward
 test2.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test2.txt

user:~/sample_app$ git push origin master
To github.com:KAkikakuFukuhara/sample_app.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:KAkikakuFukuhara/sample_app.git'
ヒント: Updates were rejected because the remote contains work that you do
ヒント: not have locally. This is usually caused by another repository pushing
ヒント: to the same ref. You may want to first integrate the remote changes
ヒント: (e.g., 'git pull ...') before pushing again.
ヒント: See the 'Note about fast-forwards' in 'git push --help' for details.
```
 　エラーが発生した。履歴を確認してみよう。  
```console
user:~/sample_app$ git log --oneline --all --graph
* bdf4c14 (HEAD -> master, feature/test2.txt) [Add] test2.txt
* 48def68 (origin/master) [Add] README.md
* 8fc057f [Add] first commit
```
 　上記を見ると`README.md`を追加した時同じような履歴をしているため問題なくプッシュできそうに見え、エラーなど発生しないように思える。  
 　そういえば、先程リモートリポジトリ上で作成したライセンスファイルがローカルリポジトリに反映していないことに気がつく。リモートリポジトリとの同期を取るには以下のコマンド`git fetch`を用いる必要がある。ついでに履歴も確認する。  
```console
user:~/sample_app$ git fetch
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:KAkikakuFukuhara/sample_app
   48def68..ad15014  master     -> origin/master

user:~/sample_app$ git log --oneline --all --graph
* ad15014 (origin/master) Create LICENSE
| * bdf4c14 (HEAD -> master, feature/test2.txt) [Add] test2.txt
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　履歴を確認すると`origin/master`が更新されていることが分かるだろう。さらに`master`からみて分岐していることが分かる。これが原因でプッシュが成功しないのである。  

 　さてこの問題の対応としては、`master`ブランチを作業用ブランチをマージする前の`48def68`のコミットオブジェクトに戻してから`origin/master`をマージして、その後に作業用ブランチを再びマージすることで解決できる。以下のように行う。  
 　まず`master`ブランチを分岐する前に戻してから、`origin/master`をマージする。  
```console
user:~/sample_app$ git checkout master
user:~/sample_app$ git reset HEAD~1 --hard
HEAD is now at 48def68 [Add] README.md
user:~/sample_app$ git log --oneline --all --graph
* ad15014 (origin/master) Create LICENSE
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
|/  
* 48def68 (HEAD -> master) [Add] README.md
* 8fc057f [Add] first commit

user:~/sample_app$ git merge origin/master
Updating 48def68..ad15014
Fast-forward
 LICENSE | 21 +++++++++++++++++++++
 1 file changed, 21 insertions(+)
 create mode 100644 LICENSE
user:~/sample_app$ git log --oneline --all --graph
* ad15014 (HEAD -> master, origin/master) Create LICENSE
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　さて無事`master`と`origin/master`の位置が同じになった。次に`feature/test2.txt`を`master`にマージする。このマージは`non-fast-forward`マージとなるので`m`オプションを付けてコメントを加える(オプションを付けなくてもマージできるが固有のエディタが表示されるのが面倒なので付ける)。  

```console
user:~/sample_app$ git merge feature/test2.txt -m "Merge branch 'feature/test2.txt'"
Merge made by the 'recursive' strategy.
 test2.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test2.txt

user:~/sample_app$ git log --oneline --all --graph
*   b0f4e7e (HEAD -> master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 (origin/master) Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　さてマージが完了したのでリモートリポジトリにプッシュしてみよう。  
```console
user:~/sample_app$ git push
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 544 bytes | 544.00 KiB/s, done.
Total 5 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:KAkikakuFukuhara/sample_app.git
   ad15014..b0f4e7e  master -> master

user:~/sample_app$ git log --oneline --all --graph
*   b0f4e7e (HEAD -> master, origin/master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　無事にプッシュが成功した。ブラウザ側のリモートリポジトリを確認するとtest2.txt がアップロードされていることが確認できるだろう。また、履歴を確認するとリモートブランチの`origin/master`が指している位置が`master`と同じ位置にあることが分かる。  
 　このようにローカル側にあるリモートブランチの情報はフェッチ(fetch)やプッシュ(push)をすることで更新される。また、作業用ブランチをメインブランチにマージする際には、リモートブランチから変更がないかフェッチすることで確認を行い、変更があればリモートの変更をマージした後に作業用ブランチをマージすることが適切なリモートブランチとの付き合い方であることを知ってほしい。  

 　なお`master`ブランチ上で`origin/master`ブランチをフェッチしてマージするコマンドは以下のように行った。  
 ```console
 user:~/sample_app$ git fetch
 user:~/sample_app$ git merge origin/master
 ```
 　このコマンドを一つにまとめたモノとして以下のようなコマンドがある。基本的にコチラを使うことが多い。  
 ```console
 user:~/sample_app$ git pull
 ```

## 6. リモートリポジトリのダウンロード(git clone)

 　リモートリポジトリをローカルにダウンロードするには`git clone`というコマンドを使う必要がある。試しに違うディレクトリを作成して、その下のディレクトリ上で`git clone` を実行してみる。  
```console
user:~/sample_app$ cd ../
user:~/$ mkdir temp
user:~/$ cd temp
user:~/temp$ git clone git@github.com:USERNAME/sample_app.git
Cloning into 'sample_app'...
remote: Enumerating objects: 14, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 14 (delta 4), reused 11 (delta 3), pack-reused 0
Receiving objects: 100% (14/14), done.
Resolving deltas: 100% (4/4), done.
```
 　`git clone`以降のURLに関してはリモートリポジトリ上の`<> Code`と書かれた緑のドロップダウンリストの SSH タブで参照できるURLである。これを実行することでリポジトリのダウンロードができる。
```console
user:~/temp/$ ls -a
.  ..  sample_app
user:~/temp/$ cd sample_app/
user:~/temp/sample_app$ ls -a
.  ..  .git  LICENSE  README.md  test.txt  test2.txt
```


## 7. 作業用ブランチのリモートリポジトリへのアップロード

 　さて元のディレクトリに戻る。
```console
user:~/temp/sample_app$ cd ../../sample_app
```

 　ここまでの説明ではメインブランチである`master`をリモートリポジトリにアップロードしていた。作業用ブランチも同じようにリモートリポジトリにアップロードすることが可能である。  
 　以下の様にして新しく作業ブランチを切る。ファイルを追加してコミットを行う。  
```console
user:~/sample_app$ git checkout -b feature/test3.txt
user:~/sample_app$ echo "cccc" >> test3.txt
user:~/sample_app$ git add test3.txt 
user:~/sample_app$ git commit -m "[Add] test3.txt"
[feature/test3.txt d692e23] [Add] test3.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test3.txt
user:~/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　続けて`master`をアップロードした時のように作業用ブランチをプッシュする。
```console
user:~/sample_app$ git push origin feature/test3.txt
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 307 bytes | 307.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'feature/test3.txt' on GitHub by visiting:
remote:      https://github.com/KAkikakuFukuhara/sample_app/pull/new/feature/test3.txt
remote: 
To github.com:KAkikakuFukuhara/sample_app.git
 * [new branch]      feature/test3.txt -> feature/test3.txt

user:~/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt, origin/feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　上記で履歴も確認しているが作業用ブランチ`origin/feature/test3.txt`が確認でき無事にアップロードされていることが分かる。
 　また先程`git clone`でダウンロードしたディレクトリに移動して`git fetch`を実行してリモートリポジトリとの同期を取ると、以下のように`origin/feature/test3.txt`が追加されていることが分かる。

```console
user:~/sample_app$ cd ../temp/sample_app/
user:~/temp/sample_app$ git log --oneline --all --graph
*   b0f4e7e (HEAD -> master, origin/master, origin/HEAD) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
user:~/temp/sample_app$ git fetch
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:KAkikakuFukuhara/sample_app
 * [new branch]      feature/test3.txt -> origin/feature/test3.txt
user:~/temp/sample_app$ git log --oneline --all --graph
* d692e23 (origin/feature/test3.txt) [Add] test3.txt
*   b0f4e7e (HEAD -> master, origin/master, origin/HEAD) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```

このブランチをローカルのブランチとしてHEAD対象にするには`git checkout`を用いて以下のように行う。  
```console
user:~/temp/sample_app$ git checkout feature/test3.txt 
Branch 'feature/test3.txt' set up to track remote branch 'feature/test3.txt' from 'origin'.
Switched to a new branch 'feature/test3.txt'
user:~/temp/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt, origin/feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, origin/HEAD, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```

 　コレが示すことはコンピュータをまたいで作業用の編集内容を保持することができる点である。例えば本番環境でエラーが発生したモノをとりあえず作業用ブランチを切って一時的な変更をしたとする。この作業用ブランチをプッシュしておくことで、後日異なるPC上で作業用ブランチをフェッチしてくることで検証やさらなる修正を可能にすることができる。

## 8. リモートリポジトリ上のブランチを削除

 　さて先程リモートリポジトリにアップロードした作業用ブランチが必要でなくなったとする。このような場合は整理の観点から削除することが望ましい。削除は`git push`に`d`オプションを付けることで実行できる。
```console
user:~/temp/sample_app$ git push -d origin feature/test3.txt 
To github.com:KAkikakuFukuhara/sample_app.git
 - [deleted]         feature/test3.txt

user:~/temp/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, origin/HEAD, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　履歴を確認すると、`origin/feature/test3.txt` が消えていることが分かる。コレによりリモート側の作業用ブランチ`feature/test3.txt`が消えたことが分かった。なお、ローカル側にある作業用ブランチは消えないので、以下のようにブランチの削除を用いて削除する。今回は`master`に移動してから未来にある作業用ブランチを削除するので、強制削除で削除を実行した。
```console
user:~/temp/sample_app$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
user:~/temp/sample_app$ git branch -D feature/test3.txt 
Deleted branch feature/test3.txt (was d692e23).

user:~/temp/sample_app$ git log --oneline --all --graph
*   b0f4e7e (HEAD -> master, origin/master, origin/HEAD) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　元のディレクトリに戻って履歴を確認して見ると、削除した作業用ブランチがローカルリモートのそれぞれに存在していることが分かる。

```console
user:~/temp/sample_app$ cd ../../sample_app/
user:~/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt, origin/feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　`git push -d`を実行していない側でのリモート側で消えたブランチをローカル側から削除するには、以下のように`git fetch --prune`というコマンドを用いる必要がある。
```console
user:~/sample_app$ git fetch --prune
From github.com:KAkikakuFukuhara/sample_app
 - [deleted]         (none)     -> origin/feature/test3.txt
user:~/sample_app$ git log --oneline --all --graph
* d692e23 (HEAD -> feature/test3.txt) [Add] test3.txt
*   b0f4e7e (origin/master, master) Merge branch 'feature/test2.txt'
|\  
| * bdf4c14 (feature/test2.txt) [Add] test2.txt
* | ad15014 Create LICENSE
|/  
* 48def68 [Add] README.md
* 8fc057f [Add] first commit
```
 　これでリモートブランチ側にアップロードした作業用ブランチが削除できた。  

## 9. 複数人での開発

 　複数人での開発に関しての`git`と`github`の活用に関しては以下のURL先のドキュメントに記載。  
- [./tutorial_multi-user.md](./tutorial_multi-user.md)