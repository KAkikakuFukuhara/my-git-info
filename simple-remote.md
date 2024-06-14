# simple upload remote

 　本稿ではチュートリアルとか面倒だから、さっさとアップロードしたい人向け。  
 なお本稿の方法は自分が管理しているプログラムのみに限定する方法として欲しい。  

## 準備

### github ユーザー登録

以下でユーザー登録
    [https://github.co.jp/](https://github.co.jp/)

### git インストール

 　git をインストール  

```
sudo apt update
sudo apt install git
```

### git ユーザー登録

 　以下の4章に従う  
 - [./github_memo/tutorial_git_remote.md#4-初期設定](./github_memo/tutorial_git_remote.md#4-初期設定)  

## gitでファイルを登録

### ディレクトリをgitの追跡対象に追加

```bash
cd YOUR_PROJECT
git init
```

### 無視するファイルを登録

 　画像ファイルなどは追跡する必要がないので以下のようにして除外する。他にも除外したいファイルがあれば追加する（例えば動画ファイルの"*.mp4"とか）  
```bash
echo "*.jpg" >> .gitignore
echo "*.png" >> .gitignore
```

 　pythonプロジェクトの場合は以下を追加する。
```bash
echo "__pycache__" >> .gitignore
echo ".venv" >> .gitignore
echo ".python-version" >> .gitignore
echo "poetry.lock" >> .gitignore
```

### ファイルを登録

```bash
git add ./
git commit -m "[Add] First Commit"
```

## リモートリポジトリへのアップロード

### アップロード先の作成
 　まずアップロード先を作成する。このアップロード先をリモートリポジトリと呼ぶ。  
 　最初にgithubにログインする。  
 　組織アカウント(例えば会社のグループ)にアップロードしたい場合は`Home`の左のユーザー名のプルダウンリストから組織アカウントをクリックする。  
 　リモートリポジトリを作成する際には右上のプラスボタン(`+`)のプルダウンリストから`New repository`をクリックする。そして`Repository name`を自由に決める。同画面で`Public`と`Private`という二者択一のラジオボタンがある、これを`Private`にする。`Public`だと世界に公開してしまうため、とりあえず`Private`がよい。それから`Create repository` という緑のボタンを押してリモートリポジトリを作成する。  
 　リモートリポジトリが作成しおわったら、`Quick Setup --if you've done this kind of thing before`という水色の背景がある場所の `git@github.com:~`となっているURLをコピーしておく。  

### アップロード先の登録

 　gitで管理しているディレクトリ上でアップロード先を登録する。以下のようにする。  
```bash
git remote add origin master git@github.com:~
```

### アップロードする

 　以下のコマンドでアップロードを行う。
 ```bash
 git push -u origin master
 ```


