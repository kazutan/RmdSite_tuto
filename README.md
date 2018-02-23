# R MarkdownによるWebサイト作成 チュートリアル

以下の内容については,以下のページに出力しましたのでそちらを参照してください:

https://kazutan.github.io/RmdSite_tuto/

## このリポジトリの使い方

「00_sample」から「04_sample」までのディレクトリ内に，それぞれ`**_sample.Rproj`というプロジェクトファイルがあります。説明資料で指示があったら，そのフォルダのプロジェクトファイルを開いてください。

## レッスン0 専用のプロジェクトを準備

1. プロジェクトを新規で準備
2. 準備したら，**Tools - Project Option...**を開く  
(もしくは`*.Rproj`ファイルをRStudio上で開く)
    1. **Build Tools**に進み，**Website**を選択
    2. サイトのルートディレクトリを設定  
    (ここではプロジェクトのルートディレクトリにします)
3. OKをクリック

## レッスン1 最小構成で準備してbuild

以下，**01_sample**ディレクトリを使います。該当ディレクトリ内のプロジェクトファイルを開いてください。

1. `index.Rmd`を準備
    - トップページ(index.html)用
    - ファイル冒頭のYAMLフロントマターには`title`だけでOK
2. `_site.yml`を準備
    - これが**サイト全体設定ファイル**となります
    - ひとまず最低限のを記述しています
3. その他のページ用Rmdを準備
    - ファイル名などは適当に
    - 今回は簡単なのをひとつだけ
4. プロジェクトをBuild
    - **Build**タブにある**Build Website**をクリック
        - もし見当たらない場合は，一度プロジェクトを開き直してください
    - 問題がなければ，(デフォで)`_site`というディレクトリが作成
        - もしRmd内のチャンクにエラーがあって実行できない場合，buildもストップします
        - この中にWebサイトに必要なものが自動的に生成されています
        - 特に設定を変更してなければ，Viewerに表示されます
5. 生成物をチェック
    - 生成された`_site/index.html`をブラウザで開き，チェック

## レッスン2 基本的なサイト設定

以下，**02_sample**ディレクトリを使います。該当ディレクトリ内のプロジェクトファイルを開いてください。

サイトの全体設定は`_site.yml`ファイルに記述していきます。以下このファイルの内容を説明していきます。

### サイト名

```
name: "サイトタイトル"
```

- サイトのタイトルを文字列で指定します
- このサイトを開いた時，ウィンドウの一番上やタブに表示されます

### navbar関連

```
navbar: 
  title: "My ウェブサイト"
  left:
    - text: "ホーム"
      href: index.html
    - text: "趣味"
      href: shumi.html
```

- `navbar:`以降ぶら下がるのがいわゆるメニューバー設定
    - `title:`はサイト名として常に表示される
    - `left:`以降ぶら下がっているのが項目で，**左揃えで配置**される
        - `- `とその下にぶら下がるのが1項目分の情報
            - `-text:`は表示文字列，`href:`がリンク先
- この他，色々設定できますが省略します
    - 詳しくは[公式ドキュメントのここ](http://rmarkdown.rstudio.com/rmarkdown_websites.html#site_navigation)にほぼすべて書いてあります

### output関連

```
output:
  html_document:
    theme: cosmo
    highlight: textmate
    toc: true
    toc_float:
      collapse: false
    df_print: "kable"
    include:
      after_body: footer.html
    css: site_style.css
```

- 通常のR Markdownでのoutputの指定と同様です
    - 設定可能項目はたくさん
    - (現時点での)最新一覧は以下の資料を  
    [R Markdownのhtml_documentで指定できるyamlヘッダ項目について - Qiita](http://qiita.com/kazutan/items/726e03dfcef1615ae999)
- **この設定が(原則)全てのRmdファイルに当たります**
    - 個別のRmdファイルに記述すれば，そっちが優先されて上書きされます

### output_dir

```
output_dir: "docs"
```

- buildで生成される，Webサイトに必要なファイル一式を出力する場所を指定
    - ここにできたものをまるっとWebサーバーに設置すればOK
    - 標準では`_site`ディレクトリ
    - `github.io`に設置を考えているなら，`docs`がおすすめ

## レッスン03 サイトデザインを考える

以下，**03_example**ディレクトリを使用します。該当ディレクトリ内のプロジェクトファイルを開いてください。

開いたら，一旦buildしてみて確認してください。

### 知っておくべきこと

R Markdownでhtmlを生成してデザインを考える上で，以下のことは頭に留めておいてください:

1. Rmdは**Pandoc**を利用してhtmlを生成
    - Rmd -> (knit) ->  **md -> (Pandoc) -> html**
    - 良くも悪くもPandocの影響を色濃く受ける
        - [Pandoc ユーザーズガイド 日本語版 - Japanese Pandoc User's Association](http://sky-y.github.io/site-pandoc-jp/users-guide/)
        - [RStudioの&quot;knit HTML&quot;でPandocに送っている内容 - Qiita](http://qiita.com/kazutan/items/eb15a42607f87f57b525)
2. 生成されたhtmlには**[Bootstrap](http://getbootstrap.com/)**と**[jQuery](https://jquery.com/)**が標準で組み込まれる
    - Bootstrapベースでデザインすると楽
    - 別のcssフレームワークにする場合は以下の記事を参照
        - [R Markdownで標準のCSSとhighlightを取り除くには - Qiita](http://qiita.com/kazutan/items/fe142344f33b1c980276)

### Rmdで生成されるタグのid, class

- 基本的にPandocの仕様で生成されます
    - Rmdのサンプルは`03_example/id_class_check.Rmd`
    - 生成されるhtmlは`03_example/docs/id_class_check.html`
    - これの詳しい解説は，以下の記事を参照
        - [R Markdownで生成するhtmlタグ要素のidとclassを確認 - Qiita](http://qiita.com/kazutan/items/807192a0449615dc8852)
- 上記サンプルを参考に，cssを設定すればOK
    - サイト全体テーマなら，`_site.yml`で指定したcssファイルに
    - 個別ページで当てたい場合については後述

### Bootstrapのgrid systemを利用

Bootstrapは[grid system](http://getbootstrap.com/css/#grid)を採用しています。R MarkdownではBootstrapを標準で組み込んでいますので比較的簡単に実装できます。

実装する方法はいくつかあります:

1. `<div>`タグ直打ちで準備
    - gridレイアウトにしたい部分に対して<div>を直打ちして指定
    - 生成物が複雑になりがちでメンテしにくくなる
2. Pandocの拡張機能を利用してclass属性を付与
    - 詳しくは以下の記事を参照
        - [R MarkdownでBootstrapのグリッドシステムを利用する - Qiita](http://qiita.com/kazutan/items/6301087a41a18c96faf2)
    - Rmdのサンプルは`03_example/grid_test.Rmd`
    - 生成されるhtmlは`03_example/docs/grid_test.html`
    - 設定する見出しのレベルにさえ配慮しておけば簡単

個人的には後者をおすすめします

### cssの優先順位

- **個別ページでcssを設定した場合`_site.yml`で指定したcssと差し替える**
    - サイト設定によるcssを活かしたい場合:
        - cssファイルを2つ(orそれ以上)全てそのページで指定
        - yaml部分で`include`を活用してスタイルシートを指定
        - cssチャンクを利用して<body>部分にstyleを記述
- cssチャンク > includeによる指定 > サイト設定 の順で優先される
    - Rmdサンプルは`03_example/css_test.Rmd`
    - 生成されるhtmlは`03_example/docs/grid_test.html`

他の複数ページでも同じスタイルを利用するのであれば，yamlの`include`オプションでうまく当てるのがスムーズです。もし「このページだけでちょっと当てたい」というのであればcssチャンクを使うのが楽でしょう。

## レッスン04 発展編

以下，**04_example**ディレクトリを使用します。該当ディレクトリ内のプロジェクトファイルを開いてください。

開いたら，一旦buildしてみて確認してください。

### Rでhtml生成

- Rでhtmlを生成していくことも可能
    - `knitr::kable`などもそのひとつ
    - **htmltools**パッケージを駆使すれば色々できる
    - 詳しくは[公式ドキュメントのこちら](http://rmarkdown.rstudio.com/rmarkdown_websites.html#html_generation)を参照
- たとえばサムネイルを作成することも可能
    - Rmdサンプルは`04_example/html_gene.Rmd`
    - 生成されるhtmlは`03_example/docs/html_gene.html`

### JavaScriptを利用

- htmlwidgets系を使えばインタラクティブな可視化
    - 詳細は省略しますが，そのままRチャンクに組み込めばOK
- R MarkdownではJavaScritpチャンクも使用可能
    - 詳しくは[公式ドキュメントのこちら](http://rmarkdown.rstudio.com/authoring_knitr_engines.html#javascript)を参照
    - jQueryを標準で読み込んでいるので，そのまま使える
- htmlwidgetsやjQueryを組み込んだ例:
    - Rmdサンプルは`04_example/js_test.Rmd`
    - 生成されるhtmlは`03_example/docs/js_test.html`

### その他

説明および具体例は省略します。

- **include**オプションと**exclude**オプションでoutput_dirへファイルをコピーするかどうかを制御可能
    - 詳しくは[公式ドキュメントのこちら](http://rmarkdown.rstudio.com/rmarkdown_websites.html#site_configuration)を参照
- ナビゲーションバー(navbar)について,自分でもっとBootstrapの機能を使ったものにできます
    - `_navbar.html`というファイルを準備
    - ここにBootstrapのnavbarに関するhtmlシンタックスを記述すればOK
    - 詳しくは[公式ドキュメントのこのあたり](http://rmarkdown.rstudio.com/rmarkdown_websites.html#site_navigation)を参照
- 繰り返し使うような要素はそこを別Rmdファイルに部分化し,呼び出して使うと楽
    - 詳しくは[公式ドキュメントのこのあたり](http://rmarkdown.rstudio.com/rmarkdown_websites.html#site_navigation)や,[この記事](http://qiita.com/kazutan/items/72276e204944b191c5d5)あたりを参考にしてください


## サイト公開(デプロイ)

サイトを作成したら公開するのですが，設置する場所が必要です。

### 通常のWebサーバー

- 設置先のサーバーを準備
- サーバ上の，Webサイトでのルートディレクトリに設定した場所を確認
- `_site.yml`で`output_dir: `指定した出力先(標準では`_site/`)に入っているファイルやディレクトリをまるっとコピー

ようするに，出力されたものをまるっと置いてしまえばOKです。

### GitHub上に設置して公開

- GitHubにリポジトリを作成
    - 公開するにはPublicである必要あり
- プロジェクトのディレクトリをGitHubリポジトリに紐付け
    - `.gitignore`に`doc/`を追記しとくこと
- `_site.yml`の`output_dir: `を**"docs"**へ指定
    - 設定したらbuildを実行
- commitしてmasterにPush
- ブラウザでGitHubの該当リポジトリへ
    - **Setting**タブをクリック
    - 下へスクロールして**GitHub Pages**へ
    - **Source**で，**Master branch /docs folder**に切り替える
- しばらく待って，`https://(アカウント名).github.io/(リポジトリ名)/`へアクセス

以降は修正したらbuildしてcommit - pushでOKです。Gitに慣れているならば，これが一番楽になるでしょう。また，上記のレッスン00でプロジェクトを準備する際に，はじめからVersion Controlを指定しておくと楽でしょう。

**Enjoy!**