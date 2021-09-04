---
id: 227
title: 2017年Python環境設定(andaconda/virtualenv/cookiecutter)
date: 2017-06-22T08:42:19+09:00
author: Shohei
layout: revision
guid: http://www.wazalab.com/2017/06/22/220-revision-v1/
permalink: /?p=227
---
## 1. Install anaconda / conda 

[condaによるポータブルなPython環境構築のすすめ](http://qiita.com/y__sama/items/5b62d31cb7e6ed50f02c)を参考にすると、(ana)condaが最近の主流とのこと。
下記にアクセスし、各OSのパッケージをダウンロードしてインストール。3系を使います。

https://www.continuum.io/downloads


あとでLinux上で動かす予定なので、なるべく依存関係を作りたくないので、pyenvの導入はしないことにしました。

### 2. Create virtualenv

Rubyのbundler相当なのが、virtualenvらしいので、仮想環境を作り、その環境に入る。

<pre class="lang:default decode:true " >
$ conda create -n kagamibot
$ conda env list
$ source activate kagamibot # deactivate で出る
</pre> 



### 3. Install packages

基本、conda installでパッケージを入れていくだが、パッケージがないことも多く、そんなときは、conda-forgeを入れるようになるので、最初から追加しておく

```
$ conda config --add channels conda-forge
```

とはいえ、virtualenv変更後はpipでも良いので、 `conda install`でなかったら、`pip install`みたいな感じ。

```
$ conda install xxx 
$ pip install xxx
```

virtualenvでのパッケージはcondaで管理できるので、新しいプロジェクトをコピーするときは、下記のようにexport/importする

<pre class="lang:default decode:true " >conda env export &gt; myenv.yaml  #  conda env create --file myenv.yaml</pre> 

(参考）http://qiita.com/Hironsan/items/4479bdb13458249347a1

### 4. Create Project

調べてみるとプロジェクトのディレクトリ構造の自由度が高い。
つまり、人々が各々ディレクトリを作ったり、ファイルを作ったりしている。それがフレームワークを使ってる私みたいな人には、非常に気持ちが悪い。
プロジェクトの雛形が欲しいので、調べるとcookiecutterというのがあるのでそれを使う。


今回はデータサイエンス用のテンプレを使用しているが、テンプレートはpythonだけでなく多言語のもある。
詳しくは、[githubのcookiecutter](https://github.com/audreyr/cookiecutter)を参照してほしいが、
下記のようなboilerplateができるので自分で考えなくてよいのは非常によい。
 
 
<pre class="theme:dark-terminal font-size:11 line-height:12 lang:default decode:true " >├── LICENSE
├── Makefile           &lt;- Makefile with commands like `make data` or `make train`
├── README.md          &lt;- The top-level README for developers using this project.
├── data
│   ├── external       &lt;- Data from third party sources.
│   ├── interim        &lt;- Intermediate data that has been transformed.
│   ├── processed      &lt;- The final, canonical data sets for modeling.
│   └── raw            &lt;- The original, immutable data dump.
│
├── docs               &lt;- A default Sphinx project; see sphinx-doc.org for details
│
├── models             &lt;- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          &lt;- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         &lt;- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            &lt;- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        &lt;- Generated graphics and figures to be used in reporting
│
├── requirements.txt   &lt;- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze &gt; requirements.txt`
│
├── src                &lt;- Source code for use in this project.
│   ├── __init__.py    &lt;- Makes src a Python module
│   │
│   ├── data           &lt;- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       &lt;- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         &lt;- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  &lt;- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            &lt;- tox file with settings for running tox; see tox.testrun.org</pre> 



作成方法は、

```
$ conda install cookiecutter
$ cookiecutter https://github.com/drivendata/cookiecutter-data-science 
# インタラクティブに質問に答えるかたちでプロジェクト作成
```

プロジェクト名のディレクトリが作成されるので、そこに入り、パッケージをインストール。

```
$ cd kagamibot 
# pipのインストール用のrequirements.txtをcondaでインストールする。
$ cat requirements.txt | while read package; do conda install $package; done
```



### 5. python-dotenv

Githubに載せたくないAPIのキーなどを管理する
cookiecutterのテンプレートにあるのに、requirements.txtにないのでインストール。

```
$ conda install phython-dotenv

# PROJECT_ROOT/.env
TWITTER_AUTH_TOKEN=xxx
```

使い方は、下記のような感じ。

```
import os
from dotenv import load_dotenv, find_dotenv

# find .env automagically by walking up directories until it's found
dotenv_path = find_dotenv()

# load up the entries as environment variables
load_dotenv(dotenv_path)

twitter_auth_token = os.environ.get("TWITTER_AUTH_TOKEN")
```

以上が、色々なページを参考にして構築した環境です。
こういうのって時間がかかるので、参考になれば幸いです。

