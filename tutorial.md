
# チュートリアル

- [1. 用語](#1-用語)
- [2. インストール](#2-インストール)
- [3. 環境を設定する。](#3-環境を設定する)
- [4. ローカルリポジトリに保存しよう](#4-ローカルリポジトリに保存しよう)
- [5. README.mdファイルを用意しよう](#5-readmemdファイルを用意しよう)
- [6. リモートリポジトリに保存しよう](#6-リモートリポジトリに保存しよう)
- [7. 例題１](#7-例題１)
- [8. 差分の表示](#8-差分の表示)
- [9. リポジトリをダウンロードする](#9-リポジトリをダウンロードする)
- [10. 環境構築サポート](#10-環境構築サポート)
  - [10.1. pythonプロジェクトの場合](#101-pythonプロジェクトの場合)

## 1. 用語

|        用語        |                意味                |
| ------------------ | ---------------------------------- |
| リポジトリ         | データの保存先                     |
| ローカルリポジトリ | ローカル環境(PC上)にあるリポジトリ |
| リモートリポジトリ | サーバ側にあるリポジトリ(github等) |


## 2. インストール

git プログラムをインストールする。

```bash
sudo apt update # 全てのパッケージの最新情報を取得
sudo apt upgrade # 全てのパッケージを更新する
sudo apt install git
```

## 3. 環境を設定する。

以下のmarkdownファイルの1章と2章を参考にgitとgithubの環境設定を行う。
[git_memo/remote.md](git_memo/remote.md)



## 4. ローカルリポジトリに保存しよう

- git の初期化  

現在のディレクトリをgitの監視下に追加する
```bash
git init
```

- gitの監視されない（無視する）ファイルを設定する  

動画ファイル等、巨大なファイルを含むプロジェクトはgithub上にアップロードできないので監視下から外す必要がある。  
そのために`.gitignore`ファイルを作成して明示的に無視させる。
```bash
echo "" >> .gitignore
echo "*.png" >> .gitignore
```
詳細な.gitignoreの仕様に関しては以下のURLを参照。  
[https://www-creators.com/archives/1662](https://www-creators.com/archives/1662)  

- gitの履歴対象にファイルを追加(ステージング)
```bash
git add FILE_NAME # 現在のディレクトリ下の特定のファイルを追加する
git add ./ # カレントディレクトリ以下を全て追加する。
```

- gitの履歴対象をまとめて履歴として保存(コミット)  

mオプション以下はコメント。つけない場合は`nano`エディタが起動してコメントを書けと催促されるので必ずつける。
```bash
git commit -m 'hoge'
```

## 5. README.mdファイルを用意しよう

github上では各ディレクトリのREADME.mdファイルもしくはREADME.rstファイルが文章として表示される。  
そのため、プロジェクトをリモートリポジトリに保存するためには作成する必要がある。  
.mdファイル（Markdown)が書きやすいのでオススメである。


## 6. リモートリポジトリに保存しよう

リモートリポジトリはgithub等のクラウド上にある保存先である。  
ローカルリポジトリの内容を反映させることができる。  
まずは事前に空のリポジトリを作成しておく必要がある。  

- リモートリポジトリを作成
github上でリモートリポジトリを作成する。作成方法に関してはネットで調べよう

- リモートリポジトリ先を登録
ローカルリポジトリとリンクするリモートリポジトリを登録する
```bash
git remote add origin git@github.com:hoge/hoge.git
```

- リモートリポジトリに保存
ローカルリポジトリの内容をリモートリポジトリに反映させる。
```bash
git push origin master
```

リモートリポジトリ関連の情報は[git_memo/remote.md](git_memo/remote.md)に記述している。

## 7. 例題１

例としてテストプロジェクトを作成して、自分のgithubにアップロードしてみよう。

```bash
# 2章と3章が終わっているとする
# github上にtest-projectというリポジトリを作成している
mkdir test-project
cd test-project
echo "test" >> test.txt
git init
git status # 現在の状態が表示される
git add ./
git commit -m "[Add] first commit"
git remote add origin git@github.com:hoge/test-project.git
git push -u origin master # github上にアップロードされているか確認しよう (private)かどうかも要確認
```

## 8. 差分の表示

gitは差分を表示することができる。  
例えば先程の例の続きのリポジトリを使用して差分を確認する。  
```bash
echo "aaaa" >> test.txt
git diff
```
差分が表示される。`aaaa`が追加されたことが分かる。
```git
diff --git a/test.txt b/test.txt
index 9daeafb..c3a88a1 100644
--- a/test.txt
+++ b/test.txt
@@ -1 +1,2 @@
 test
+aaaa
```
変更したファイルを元に戻すにはcheckoutを用いる。
```bash
git checkout test.txt
cat test.txt
```
`aaaa`が追加されてない状態に戻る。
```
test
```

## 9. リポジトリをダウンロードする

ダウンロードしたいディレクトリにコピーする
```bash
git clone 'http:/github.com/hoge/hoge'
```
これによりリモートリポジトリの内容がローカルリポジトリとして保存される。
`注意` リポジトリを更新するわけではない。


## 10. 環境構築サポート

言語によって環境構築を簡易にするための方法がある。  
それらの方法について記述する。

### 10.1. pythonプロジェクトの場合

pythonプロジェクトの場合、パッケージのバージョンの違いやpythonのバージョンの違い等を揃える必要がある。  
そのため、環境構築を楽にするための方法がある。それらの具体的な方法については[git_memo/python_env.md](git_memo/python_env.md)に記載している。pythonプロジェクトをgitで管理する場合は必ず行うように。