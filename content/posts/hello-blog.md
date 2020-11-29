---
title: "Hugo × GitHub Pages でブログを開設しました"
date: 2020-11-29T16:10:13+09:00
draft: false
toc: false
images:
tags:
  - Hugo
  - Blog
---

## はじめに
就職活動も本格的に始まり，自分がどのようなエンジニアになりたいか，自分が入りたい企業はどんなところか，どのようなエンジニアなら尊敬できるかなど考える機会が沢山あります．

そんな中でも「どのようなエンジニアなら尊敬できるか」の一つの答えとして，
>日々勉強や研究を怠らず，ブログやSNSを通じて情報を発信ししている

ということを無意識のうちに一つの指標としていました．

自分も近いうちにそんな風に思ってもらえるようになりたいし，アウトプットは自分の知識を深める機会にもなるのでこの機会にブログを開設してみました．

## 今回利用したもの
というわけで，エンジニアブログらしくブログ開設においてつまずいた点などを挙げていきます．

### Hugoとは？[[1]](https://gohugo.io/)
公式サイトの["What is Hugo"](https://gohugo.io/about/what-is-hugo/)によると，
> HugoはGo言語製の高速かつモダンな静的サイトジェネレータ．

とあります．

動的サイトとは異なり，静的サイトはコンテンツを作成・更新したときにページをビルドし静的サイト（HTML）をホスティングするだけなので高速に表示可能です．


また，Go言語を利用しているということもあり，記事数が増加しても数秒程度のビルド時間で済んでしまうというスピード感も魅力的です．
<!-- （なぜGo言語はビルドが高速なのか？） -->

以下のベンチマークを見てわかるように，5000記事あったとしても6～7秒程度でビルドが完了しています．

{{< youtube CdiDYZ51a2o >}}

### GitHub Pagesとは？[[1]](https://gohugo.io/)
無料かつ高速な静的なサイトホスティングサービスです．

```<USERNAME>.github.io```というリポジトリを作成し，そこにコンテンツを置いておくだけでサイトが公開できてしまうのでとても簡単です．

また，SSL化（現在はデフォルトで```https```となる）や[カスタムドメイン](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site)の利用も可能であり無料なのに柔軟性があるのが魅力的です．

## Hugo × GitHub Pagesでブログを開設する
それでは，ブログ開設手順を見ていきましょう．

基本的には，

- [Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)

を見れば開設できると思いますが，追加の注意点等も記載しながら手順を追っていこうと思います．

### 前提条件
開発環境は以下の通りです．
- Windows 10 Home
- Hugo v0.79.0/**extended**←注意
- Windows Power Shell

### 1. 新規サイトの作成
まずは，[Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)に従って新規サイトを作成します．

#### 1-1. Hugoのインストール
Quick Startでは```macOS```を前提としているため，[Install Hugo | Hugo](https://gohugo.io/getting-started/installing)にある「Chocolatey (Windows)」を参考にしました．

今回ブログテーマとして使いたかった[hello-friend-ng](https://themes.gohugo.io/hugo-theme-hello-friend-ng/)は"extended" Sass/SCSSバージョンが必要だったので```hugo```ではなく```hugo-extended```をインストールする必要があります．

```shell
$ choco install hugo-extended -confirm
```

```shell
$ hugo version 
# Hugo Static Site Generator vx.xx.x/extended ...
```

#### 1-2. 新規サイトの作成

```shell
hugo new site <folder name>
```

今回は```<folder name>```を```blog```としました．これで```blog```というフォルダに新しいHugo製のサイトが生成されます．

#### 1-3. テーマの追加
テーマは[themes.gohugo.io](https://themes.gohugo.io/)を見て，好きなテーマを探しましょう！本サイトでは[hello-friend-ng](https://themes.gohugo.io/hugo-theme-hello-friend-ng/)を利用します．

まずは，GitHubからテーマをダウンロードして```themes```ディレクトリに追加します．

```shell
$ cd blog
$ git init
$ git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```
3つめのコマンドは利用するテーマに合わせてURIや```themes/<theme name>```部分を変更してください．

テーマのダウンロードができたら```config.toml```をエディタで開き，末尾に
```toml
theme = "hello-friend-ng"
```

を追加してください．

ちなみに，ドキュメント通りに```echo```を利用したらWindowsでは文字化けしました...

"hello-friend-ng"の部分は，自分が利用したいテーマの名前に変更してください！

#### 1-4. コンテンツの追加
試しに記事を作成してみます．

```shell
$ hugo new posts/my-first-post.md
```

で，自動的に記事を生成してくれます．
"hello-friend-ng"の場合は以下のような型で，ファイルが生成されます．

```md
---
title: "My First Post"
date: 2020-11-29T15:50:39+09:00
draft: true
toc: false
images:
tags:
  - untagged
---
```

#### 1-5. Hugoサーバを起動する
```shell
hugo server -D
```

でドラフト記事も含めてローカルホストで確認できるようになります．
[http://localhost:1313/](http://localhost:1313/)に接続して確認してみましょう．

#### 1-6. テーマをカスタマイズする
それでは，GitHub Pagesにアップロードする準備を始めましょう．
```config.toml```を開いて，

```toml
baseURL = "https://<USERNAME>.github.io/"
languageCode = "ja"
title = "好きなタイトル"
theme = "hello-friend-ng" # 選んだテーマの名前にしてください
```

のようにすれば最低限の設定は完了です．


### 2. GiHubへのホスティング
続いて[Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)を参考に，GitHubへホスティングしていきます．

#### 2-1. リポジトリの準備
ブログを公開するにあたって以下の2つのリポジトリを作成します：
- ```<USERNAME>.github.io```：生成されたコンテンツをホスティングするためのリポジトリ
- ```blog```：Hugoのコンテンツやその他のファイルを管理するリポジトリ

#### 2-2. ステップ１で作成したサイトをリポジトリにコピペする
まずは，```blog```リポジトリを```clone```します．

```shell
$ git clone <Repository URL of blog>
$ cd blog
```

その後，```blog```ディレクトリの下にステップ1で作成したサイトをコピペ．

```
blog
├─ archetypes
├─ data
├─ layouts
├─ public
├─ resources
├─ static
├─ themes
├─ .gitmodules
└─ config.toml
```

のようになると思います．

#### 2-3. サイトが動くか確認
ここまで出来たら，

```shell
$ hugo server -D
```

でサイトが動くかどうか確認しておきましょう．動かなかった場合は，
- ```config.toml```で設定した```theme```の名前が合っているか
- ディレクトリ構成が合っているか？
- コマンドを打った時にエラーが出てきていたのに無視していなかったか

等を確認してみてください．

ローカルホストで問題なく動いたのであれば，
- Ctrl+Cでサーバーをkill
- ```rm public```で```public```ディレクトリを完全に削除

しておきましょう．

#### 2-4. サブモジュールを作成する
```shell
$ git submodule add -b main https://github.com/<USERNAME>/<USERNAME>.github.io.git public
```

でサブモジュールを作成しましょう．

こうすることで```hugo```コマンドで```public```に静的サイトを生成し，そのディレクトリは```<USERNAME>.github.io```にリモートオリジンをとして登録されます．


#### 2-5. ```deploy.sh```の作成
シェルスクリプトを作成してデプロイを簡単にします．
```deploy.sh```は```blog```直下に作成して，以下のように設定します：

```shell
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Build the project.
hugo -t "hello-friend-ng" # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public

# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin main
```

ここで，```# Build the project.```の部分は自分の選んだテーマの名前に変更することに注意してください．

#### 2-6. デプロイ
```shell
$ ./deploy.sh
```

で```<USERNAME>.github.io```リポジトリに```public```の内容が反映され，[https://\<USERNAME\>.github.io/](https://<USERNAME>.github.io/)にアクセスすると，サイトが公開されていることが分かります．（変更の反映に時間がかかることがあります）

以上でサイトの公開が完了しました！

```config.toml```にプロパティを追加することでタイトルなどを変更できるため，選択したテーマのドキュメントを参考にして色々カスタマイズしていってください！

## 参考文献
- [Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)
- [Install Hugo | Hugo](https://gohugo.io/getting-started/installing)