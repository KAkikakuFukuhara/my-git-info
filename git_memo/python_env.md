# pythonプロジェクトにおける環境の共有

pythonでプロジェクトを作成している場合、使っているpythonのバージョンやライブラリ等の環境を合わせる必要がある。  
そのため環境を合わせるために必要なファイルをgithub等にあげておく必要がある。  
ここでは、環境を合わせるために必要なファイルの出力方法を記載する。  
基本的にpipでインストールできる`requirements.txt`は必要なので必ず用意すること。

**目次**
- [1. 出力](#1-出力)
  - [1.1. pip 環境](#11-pip-環境)
  - [1.2. conda 環境](#12-conda-環境)
  - [1.3. poetry環境](#13-poetry環境)
- [2. 再現](#2-再現)
  - [2.1. pip](#21-pip)
  - [2.2. anaconda](#22-anaconda)
    - [2.2.1. conda環境ファイルを用いる場合](#221-conda環境ファイルを用いる場合)
    - [2.2.2. pip環境ファイルを用いる場合](#222-pip環境ファイルを用いる場合)
  - [2.3. poetry](#23-poetry)
    - [2.3.1. poetry 環境ファイルを用いる場合](#231-poetry-環境ファイルを用いる場合)
    - [2.3.2. pip環境ファイルを用いる場合](#232-pip環境ファイルを用いる場合)


## 1. 出力

### 1.1. pip 環境

pip 環境を用いている場合は以下の方法で環境情報ファイルを出力する。
```bash
pip freeze > requirements.txt
```
それに加えて使用したpythonのバージョンをREADME等に記載しておくことも忘れないように。

### 1.2. conda 環境

anacondaを使用している場合は anaconda で再現できるようにするための環境情報ファイルを出力することができる。  
anacondaは社員人数が多い企業での商業使用では有料になることがあるため、pip等での再現も行えるようにしておいた方がよい。  
そのため、そのファイルと、pipでの再現が可能になるファイルの２つを出力することが好ましい。  

- **conda環境ファイル**  
```bash
conda env export > env.yml
```

上記の方法はanacondaにインストールされているパッケージを全て記録した環境ファイルを作成してくれる。  
しかし、condaでインストールした際のパッケージに必要な下位パッケージが多く記載されているので冗長である。  
そこで上位パッケージだけを含んだ環境ファイルの作成方法を以下に記載する。  
この方法はpipでインストールしたパッケージを含んでくれないので、pipでインストールしたパッケージがあるプロジェクトでは使用しないほうがよい。  
```bash
conda env export --from-history > env.yml
```
pythonのバージョンが含まれていないので以下のURLを参照して`env.yml`にpythonのバージョン情報を記載する。  
[https://stackoverflow.com/questions/64670318/how-to-create-a-new-conda-env-based-on-a-yml-file-but-with-different-python-vers](https://stackoverflow.com/questions/64670318/how-to-create-a-new-conda-env-based-on-a-yml-file-but-with-different-python-vers)  

pipでインストールしたパッケージがあるプロジェクトで、冗長なパッケージを含んでいない環境ファイルを作成したい場合は以下のURLのスクリプトをコピーしてきて用いる必要がある。  
[https://gist.github.com/gwerbin/dab3cf5f8db07611c6e0aeec177916d8#file-conda_env_export-py](https://gist.github.com/gwerbin/dab3cf5f8db07611c6e0aeec177916d8#file-conda_env_export-py)    
yamlパッケージが必要となるのでインストールしてから使用する。(python3.11あたりで標準パッケージに含まれるのらしいので必要なくなるかも)  
```bash
conda activate TestEnv
conda install pyyaml
python conda_env_export.py > env.yml
```
こちらもpythonのバージョン情報が記載されていないので付加する必要がある。  


- **pip環境ファイル**  
```bash
conda list -e > requirements.txt
```

それに加えて使用したpythonのバージョンをREADME等に記載しておくことも忘れないように。  

### 1.3. poetry環境

poetry環境の場合、以下の２つのファイルが自動で作成されているのでそれらをgitに含めればよい。  
- pyproject.toml
- poetry.lock

それに加えて`requirements.txt`を作成しておくとよいだろう。  
```bash
poetry export --format requirements.txt --output requirements.txt
```
またpythonのバージョンをREADMEに記載しておくことを忘れない。  

## 2. 再現

### 2.1. pip 

pip環境の再現は以下の方法を用いることで再現できる。  
```bash
pip intsall -r requirements.txt
```

### 2.2. anaconda

conda環境ファイルを用いる場合とpip環境ファイルを用いる場合に分けて記述する。  

#### 2.2.1. conda環境ファイルを用いる場合

```bash
conda env create -f=env.yml
```

依存関係の解消に時間がかかるため、環境ファイルの内容を手動でインストールした方が早い場合もある。  

#### 2.2.2. pip環境ファイルを用いる場合

requirements.txtを用いる場合は、condaのpipパッケージを用いて環境の再現を行う。  
そのため、仮想環境を作成してから`pip`を使用してインストールを行う。  
```bash
conda create -n ENV python=3.X.X
conda activate ENV
pip install -r requirements.txt
```

### 2.3. poetry

#### 2.3.1. poetry 環境ファイルを用いる場合

poetry環境ファイルを用いる場合は以下を行う。  
```bash
poetry shell
poetry install
```

#### 2.3.2. pip環境ファイルを用いる場合

poetryがrequirements.txtを直接サポートしていないので、requirements.txtを無理やり流し込む。
```bash
poetry add $(cat requirements.txt)
```
参考：[https://stackoverflow.com/questions/62764148/how-to-import-requirements-txt-from-an-existing-project-using-poetry](https://stackoverflow.com/questions/62764148/how-to-import-requirements-txt-from-an-existing-project-using-poetry)