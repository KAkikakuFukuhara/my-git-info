# git submodule に関して

gitに他のリポジトリを加える方法として `git submodule` がある。
この機能によって追加されたディレクトリは独立した git 環境を持っている。このディレクトリ内での変更をリモート側に反映したり、リモート側の変更をローカル側に反映することができる。

## チュートリアル
- 追加
```bash
git submodule add URL TARGET # TARGETをしてしない場合はカレントディレクトリに追加
```

- 完全に削除
```bash
git submodule deinit TARGET 
git rm TARGET
rm -rf .git/modules/TARGET
```

- submodule を含むリポジトリのclone
```bash
git clone --recursive URL
```
上記を忘れた場合は以下
```bash
git submodule update --init
```

## 削除に関して

git submodule add を実行すると以下の箇所に submodule のデータが保存される。
```console
TARGET
.gitmodules
.git/config
.git/modules
```
削除する際にはコレらの全ての箇所を削除する必要がある。
削除は以下の順番で行う。

```bash
git submodule deinit TARGET 
# このコマンドにより以下の箇所のsubmoduleの内容が削除される
# TARGET/* # TARGETは残る
# .git/config
# まだコミットしていない場合は -f オプションをつける

git rm TARGET
# このコマンドにより以下の箇所のsubmoduleの内容が削除される
# TARGET
# .gitsumodules
# まだコミットしていない場合は -f オプションをつける

rm -rf .git/modules/TARGET
```

## 再追加

再度同じ名称の submodule を追加する場合は通常の追加と同じコマンドを打つ
エラーが発生した場合は、完全削除ができていない。


## Rename

https://chaika.hatenablog.com/entry/2017/05/05/090000

## 参考
https://qiita.com/sparklingbaby/items/8a5a27ba52160a20ee3d