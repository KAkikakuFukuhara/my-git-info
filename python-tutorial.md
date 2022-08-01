# pythonプロジェクトにおけるgit活用

**目次**
- [1. 初期化](#1-初期化)
- [2. .gitignoreの設定](#2-gitignoreの設定)
- [3. README.mdファイルの追加](#3-readmemdファイルの追加)
- [4. pythonの環境構築するためのファイルの追加](#4-pythonの環境構築するためのファイルの追加)
- [5. 履歴対象ファイルを登録　＆　まとめて履歴として記憶する](#5-履歴対象ファイルを登録まとめて履歴として記憶する)
- [6. リモートリポジトリにアップロードする](#6-リモートリポジトリにアップロードする)
- [7. 基本的な流れ](#7-基本的な流れ)

pythonプロジェクトにおけるgitの導入方法ついて記載する。  
以下のようなディレクトリ構成のpythonプロジェクトに対してgitを導入していく。  

```
.
├── data
│   ├── dataset
│   │   ├── train1.jpg
│   │   ├── train2.jpg
│   │   └── train3.jpg
│   └── samples
│       ├── sample1.jpg
│       ├── sample2.jpg
│       └── sample3.jpg
└── python_tutorial
    ├── __main__.py
    └── lib
        ├── __init__.py
        ├── __pycache__
        │   ├── __init__.cpython-36.pyc
        │   └── loaders.cpython-36.pyc
        └── loaders.py
```
なお環境構築やgithubへの登録などは[tutorial.md](tutorial.md)で実行済みとする。

## 1. 初期化

まずは初期化を行う。適宜プロジェクトのルートディレクトリに移動してから以下のコマンドを実行する。
```bash
git init
```
初期化がうまくいっているか以下のコマンドを入力して確認する。うまくいっていればコマンドの下のような表示がされる。
```bash
git status
---
ブランチ master

No commits yet

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	data/
	python_tutorial/

nothing added to commit but untracked files present (use "git add" to track)

```

## 2. .gitignoreの設定

このままgitで履歴追跡対象に追加していってもいいが、データセットなどの大規模なファイル等を履歴の追跡対象に含めることは好ましくない。  
そのため無視するファイルを`.gitignore`というファイルに定義する。  
データセットは`jpg`ファイルで管理されているので`jpg`ファイルを無視するようにワイルドカードを用いて以下のようにして`gitignore`ファイルを作成する。ちなみに以下のコマンドは`"*.jpg"`というテキストを`.gitignore`ファイルに上書きするという意味である。
```bash
echo "*.jpg" >> .gitignore
```
無視するように定義した影響を見てみよう。追跡されていないファイルにさえ登場しなくなるように無視されていることが分かる(dataディレクトリはjpgファイルのみで構成されているためまとめて無視される)。
```bash
git status
---
ブランチ master

No commits yet

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	python_tutorial/
```

ついでに`__pycache__`とディレクトリも除外しておこう。このディレクトリはpython実行に作成されるモノとなるので必要ではないからである。ディレクトリを除外するときはディレクトリ名を`.gitignore`に追加する。
```bash
echo "__pycache__" >> .gitignore
```
これで一通り無視したいファイルの設定が終了した。しかし、困ったことにsample.jpg等は残して置きたい場合がある。このような際には特定のファイル例外として`.gitignore`に記載して無視されないようする必要がある。特定のファイルを除外する際には`!`を文頭に記載する必要がある。
```bash
# echo "!/data/samples/*.jpg" >> .gitignore # コノ方法ではエラーが起きたのでテキストエディタで追加する
```
結果を確認すると、先程無視されていた`data`ディレクトリが復活していることが分かる。
```bash
git status
---
ブランチ master

No commits yet

追跡されていないファイル:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	.gitignore~
	data/
	python_tutorial/

nothing added to commit but untracked files present (use "git add" to track)
```
これで無視するファイルと無視から除外するファイル（つまり追跡可能ファイル）を定義することができた。

## 3. README.mdファイルの追加

使い方等を記載するREADME.mdファイルを作成する。mdファイルは`markdown`の略語で、テキストベースで記載する階層型のテキストファイル記法である。以下のように`#`で階層を示すことができる。

```markdown
# README

## インストール方法について

### pip

## 使用方法について

python main.py

```

## 4. pythonの環境構築するためのファイルの追加

pythonでは使用しているpythonのバージョンやパッケージがプロジェクトによって異なることが多々ある。  
そのため環境を再現することができるためのファイルや情報を用意しておくことが大切である。  
以下のリンク先に環境構築用のファイルの作り方を記載しておくので、参考に環境構築用のファイルを作成するように。  
[git_memo/python_env.md](git_memo/python_env.md)
今回は`anaconda`で環境構築していたという仮定をする。

## 5. 履歴対象ファイルを登録　＆　まとめて履歴として記憶する

gitは木構造でファイルを管理しており、各ノードにファイルの履歴をまとめて記憶していく形式を取る（この木構造をワーキングツリーと呼ぶ）。  
各ノードにまとめて記憶するためのファイルを登録する場所を`インデックス`と呼び、インデックスに登録する作業を`add`と呼ぶ。そして履歴をまとめてワーキングツリーに記憶することを`コミット`と呼ぶ。  

ここで、今回のプロジェクトを`add`して`コミット`する過程を記載する。  
プロジェクトはココまでの処理によって以下のようなディレクトリ構成を持つ。  
```bash
.
├── README.md
├── data
│   ├── dataset
│   │   ├── train1.jpg
│   │   ├── train2.jpg
│   │   └── train3.jpg
│   └── samples
│       ├── sample1.jpg
│       ├── sample2.jpg
│       └── sample3.jpg
├── python_tutorial
│   ├── __main__.py
│   └── lib
│       ├── __init__.py
│       ├── __pycache__
│       │   ├── __init__.cpython-36.pyc
│       │   └── loaders.cpython-36.pyc
│       └── loaders.py
├── conda_env.yml
└── requirements.txt
```
プロジェクトのルートディレクトリにて`add`を行う。addコマンドは対象ファイルを選ぶ必要があるが、ディレクトリを指定すること、そのディレクトリ直下のファイルをまとめて追加することができる。  
```bash
git add ./
```
結果を確認すると、以下のようにコミット予定に除外されているファイル以外が登録されていることが確認できる。
```bash
git status
---
ブランチ master

No commits yet

コミット予定の変更点:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   README.md
	new file:   conda_env.yml
	new file:   data/samples/sample1.jpg
	new file:   data/samples/sample2.jpg
	new file:   data/samples/sample3.jpg
	new file:   python_tutorial/__main__.py
	new file:   python_tutorial/lib/__init__.py
	new file:   python_tutorial/lib/loaders.py
	new file:   requirements.txt
```

次に登録したファイル群をコミットする。mオプションをつけることでコメントを登録することができる。つけない場合はエディタが起動してコメントをつけろと催促してくるので、mオプションをつけることが推奨される。  
コミットを実行するとインデックスに登録されていたファイルが履歴としてワーキングツリーに記憶されていることが確認できる。

```bash
git commit -m "[Add] first commit"
---
[master (root-commit) 8b84e9d] [Add] first commit
 10 files changed, 10 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100644 conda_env.yml
 create mode 100644 data/samples/sample1.jpg
 create mode 100644 data/samples/sample2.jpg
 create mode 100644 data/samples/sample3.jpg
 create mode 100644 python_tutorial/__main__.py
 create mode 100644 python_tutorial/lib/__init__.py
 create mode 100644 python_tutorial/lib/loaders.py
 create mode 100644 requirements.txt
```
状態を確認するとインデックスがキレイになっていることが分かる。
```bash
git status
---
ブランチ master
nothing to commit, working tree clean
```
またログを確認すると、いつ、誰がコミットしたかを確認できる。
```bash
git log
---
commit 8b84e9deb1e288138b03a14ee5e8364f637d7de7 (HEAD -> master)
Author: Kenta Fukuhara <105337464+KAkikakuFukuhara@users.noreply.github.com>
Date:   Mon Aug 1 11:34:21 2022 +0900

    [Add] first commit
fukuhara@master-HP-Z4-G4-Work
```

## 6. リモートリポジトリにアップロードする

gitに対応したサービスに登録しているとワーキングツリーをクラウド上に保存することができる。  
代表的なサービスは`github`である。  
[git_memo/remote.md](git_memo/remote.md)を参考にリモートリポジトリを登録したら、`push`コマンドでアップロードすることができる。
```bash
git push -u origin master
# git push # 2回目以降
```

## 7. 基本的な流れ

基本的に作業が一段落するごとにコミットを行い、リモートリポジトリにアップロードするように意識する。  
コレにより、突然のPCの死亡等が発生した際の対策にもなる。  
以下の流れで作業を行うように意識することが大切である。

1. 編集
2. テスト
3. コミット
4. push