# gitでのリモートリポジトリとの操作方法について記載する

ここでの用いるリモートリポジトリは`github`を使用することを想定する。   

**目次**
- [1. author関連](#1-author関連)
- [2. リモートリポジトリの登録と反映(push)](#2-リモートリポジトリの登録と反映push)
- [3. リモートリポジトリからローカルリポジトリへの変更の反映](#3-リモートリポジトリからローカルリポジトリへの変更の反映)

## 1. author関連

gitは最初にユーザー情報を登録する必要がある。  
必要な情報はユーザー名とメールアドレスである。  
（間違い）~~githubを使用している場合はメールアドレスにはgithubに設定したメールアドレスを使用する必要がある。~~　  
（訂正）セキュリティ上の都合によりgithubで使用されているメールアドレスを使用するとクローンされた時に登録しているメールアドレスが筒抜けになってしまうのでよろしくない。よって以下のURLの方法を用いてメールアドレスを再設定することで登録しているメールアドレスが表示されなくなるので、こちらで登録を行う。  
- [https://snowtree-injune.com/2021/08/17/mail-git002/](https://snowtree-injune.com/2021/08/17/mail-git002/)

以下の方法で author を登録する.
idは８桁の番号でusernameはgithubのユーザー名である。これらの記載場所は上記の参照URLに記載場所が示されている

```bash
git config --global user.name "Taro Yamada"
#git config --global user.email "email@example.com" # 間違い
git config --global user.email "id+username@@users.noreply.github.com" #推奨
```

## 2. リモートリポジトリの登録と反映(push)

リモートリポジトリの登録をすることでローカルリポジトリの内容をリモートリポジトリに反映することができるようになる。  
まずリモートリポジトリを登録する前にgithubでのssh接続ができるようにssh鍵を作成して、githubに公開鍵を設置しよう。  
ssh鍵の作成＆設置は以下のURLを参考にして欲しい。
- [https://qiita.com/shizuma/items/2b2f873a0034839e47ce](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)  

次にリモートリポジトリを作成と、ローカルリポジトリへの反映先の登録を行い、その後反映を行う。  
これらは以下のURLに全て書いてあるので参考にしてほしい。
ただし、リポジトリは基本的にprivateで作成すること。
privateだと許可した人以外にアクセスできなくすることができるため。
- [https://qiita.com/sodaihirai/items/caf8d39d314fa53db4db](https://qiita.com/sodaihirai/items/caf8d39d314fa53db4db)

## 3. リモートリポジトリからローカルリポジトリへの変更の反映

リモートリポジトリの内容をローカルリポジトリに反映するには以下の方法を使う  
コレは他のPCや他の人が編集した内容をローカルに反映させる際に使用する。

```bash
git fetch origin master # リモートリポジトリから更新情報をダウンロードする
git merge origin/master # 更新情報を取り込む
```

- 一括で行うコマンド( fetch + merge )
```bash
git pull origin master
```
