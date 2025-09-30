---
html:
  embed_local_images: true
  embed_svg: true
  offline: true
  toc: true
export_on_save:
  html: true
---

# template markdown

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [template markdown](#template-markdown)
  - [専用のUbuntuを用意](#専用のubuntuを用意)
  - [環境メモ](#環境メモ)
    - [参考](#参考)
    - [流れ](#流れ)
    - [実践](#実践)
      - [言語設定](#言語設定)
      - [Rubyのインストール](#rubyのインストール)
        - [ダウンロード](#ダウンロード)
        - [解凍](#解凍)
        - [移動](#移動)
        - [configの前準備](#configの前準備)
        - [config(ライブラリ不足)](#configライブラリ不足)
        - [make(ライブラリ不足)](#makeライブラリ不足)
        - [config(ライブラリ追加後clean前)](#configライブラリ追加後clean前)
        - [make(ライブラリ追加後clean前)](#makeライブラリ追加後clean前)
        - [config(ライブラリ追加後clean後)](#configライブラリ追加後clean後)
        - [make(ライブラリ追加後clean後)](#makeライブラリ追加後clean後)
        - [インストール](#インストール)
        - [バージョン確認](#バージョン確認)
      - [PostgreSQLの設定](#postgresqlの設定)
        - [postgresqlインストール](#postgresqlインストール)
        - [専用ユーザの作成](#専用ユーザの作成)
        - [Redmine用DBの作成](#redmine用dbの作成)
        - [DB作成結果の確認](#db作成結果の確認)
      - [Redmineのインストール](#redmineのインストール)
        - [redmineに必要なライブラリ](#redmineに必要なライブラリ)
        - [redmineのダウンロード](#redmineのダウンロード)
        - [設定ファイルの作成](#設定ファイルの作成)
        - [フォントデータの用意](#フォントデータの用意)
        - [gemパッケージのインストール](#gemパッケージのインストール)
        - [セッション改竄防止用秘密鍵の作成](#セッション改竄防止用秘密鍵の作成)
        - [データベースのテーブル作成](#データベースのテーブル作成)
      - [Apacheの設定](#apacheの設定)
        - [Apache本体のインストール](#apache本体のインストール)
        - [passengerのインストール](#passengerのインストール)
        - [PassengerのApache用モジュールのインストール](#passengerのapache用モジュールのインストール)
        - [Apache用設定の確認](#apache用設定の確認)
        - [Apacheの設定ファイル作成](#apacheの設定ファイル作成)
        - [Apacheの設定更新](#apacheの設定更新)
      - [Apache上のPassengerでRedmineを実行するための設定](#apache上のpassengerでredmineを実行するための設定)
      - [他PCからの接続](#他pcからの接続)
        - [TCPポート80による接続確認](#tcpポート80による接続確認)
          - [一時的に開放](#一時的に開放)
          - [プロキシ設定前の設定確認](#プロキシ設定前の設定確認)
          - [hostPCからWSL2へのプロキシ設定](#hostpcからwsl2へのプロキシ設定)
          - [プロキシ設定後の設定確認](#プロキシ設定後の設定確認)
          - [他PCからの接続確認](#他pcからの接続確認)
          - [設定削除](#設定削除)
  - [HTTPS接続](#https接続)
    - [SSL証明書の準備](#ssl証明書の準備)
      - [SSL証明書に必要なパッケージのインストール](#ssl証明書に必要なパッケージのインストール)
      - [自己署名証明書の作成](#自己署名証明書の作成)
      - [秘密鍵のパーミッション設定](#秘密鍵のパーミッション設定)
    - [ApacheでのHTTPS設定(ポート443の有効化)](#apacheでのhttps設定ポート443の有効化)
      - [SSLモジュールの有効化](#sslモジュールの有効化)
      - [Redmine用バーチャルホストの修正(HTTPS設定の追加)](#redmine用バーチャルホストの修正https設定の追加)
      - [設定の有効化とApacheの再起動](#設定の有効化とapacheの再起動)
    - [アクセス確認](#アクセス確認)
      - [実際にアクセス](#実際にアクセス)
      - [アクセス時の注意点](#アクセス時の注意点)
    - [証明書の期限が切れた・切れる前に](#証明書の期限が切れた切れる前に)
      - [新しい証明書ペアの生成](#新しい証明書ペアの生成)
      - [秘密鍵のパーミッション確認](#秘密鍵のパーミッション確認)
      - [Apacheの設定更新と再起動](#apacheの設定更新と再起動)
  - [Redmine設定](#redmine設定)
    - [初期設定](#初期設定)
      - [管理者パス設定](#管理者パス設定)
      - [デフォルトデータのロード](#デフォルトデータのロード)
      - [言語設定・ユーザ名表示形式の変更](#言語設定ユーザ名表示形式の変更)
      - [文字コードの自動判別設定](#文字コードの自動判別設定)

<!-- /code_chunk_output -->

## 専用のUbuntuを用意

Ubuntu24.04をMicrosoftStoreから検索してダウンロード

以下設定を実施
☆

```plain
username: 
pass: 
```

以下にて既存のUbuntuとは別に保持できることを確認した。

```bash
PS C:\Users\okome> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu          Running         2
  Ubuntu-24.04    Running         2
PS C:\Users\okome>
```

## 環境メモ

デバイス名 DESKTOP-5900X
プロセッサ AMD Ryzen 9 5900X 12-Core Processor(3.70 GHz)
実装 RAM 128 GB

エディション Windows 11 Pro
バージョン 24H2
OS ビルド 26100.6584

Ubuntu24.04 lts

### 参考

[https://blog.redmine.jp/articles/6_0/install/ubuntu24/]

### 流れ

いつもの  

```bash
sudo apt update
sudo apt upgrade -y
```

### 実践

#### 言語設定

デフォルトの設定確認

```bash
komekomekome@DESKTOP-5900X:~$ localectl list-locales
C.UTF-8
komekomekome@DESKTOP-5900X:~$
```

日本語の言語パックをインストール。  
[実行ログ](log\0925-0_言語設定ログ.txt)  

```bash
sudo apt install -y language-pack-ja
```

この時点で設定も更新されていそうなことを確認できた。

```bash
komekomekome@DESKTOP-5900X:~$ localectl list-locales
C.UTF-8
ja_JP.UTF-8
komekomekome@DESKTOP-5900X:~$
```

念のためもう2つ目のコマンドも実行しておく

```bash
komekomekome@DESKTOP-5900X:~$ sudo locale-gen ja_JP.UTF-8
Generating locales (this might take a while)...
  ja_JP.UTF-8... done
Generation complete.
komekomekome@DESKTOP-5900X:~$
```

#### Rubyのインストール

##### ダウンロード

まずソースコードをダウンロード。  
-O(大文字)オプションにより、リンク先のファイル名のままカレントディレクトリへダウンロードして保存する。  

```bash
curl -O https://cache.ruby-lang.org/pub/ruby/3.4/ruby-3.4.6.tar.gz
```

##### 解凍

ダウンロードしたファイルを解凍する

```bash
tar xvf ruby-3.4.6.tar.gz
```

##### 移動

解凍したフォルダへ移動する

```bash
cd ruby-3.4.6
```

##### configの前準備

cコンパイラが必要になるのでビルドツールのパッケージをインストールする。  
オープンソースツールをまとめたものなので、無償で商用利用可能

```bash
sudo apt install -y build-essential
```

##### config(ライブラリ不足)

コンフィグコマンドによりドキュメントのインストール回避を指定しつつ、makefileを作成する。  
[実行ログ](log\0925-1_make-0ライブラリ不足-config.txt)  

```bash
./configure --disable-install-doc
```

##### make(ライブラリ不足)

makeコマンドによりmakefileに基づいたビルドを行う  
[実行ログ](log\0925-1_make-1ライブラリ不足-make.txt)  

```bash
make
```

##### config(ライブラリ追加後clean前)

ビルド時に必要となるライブラリをインストールする

```bash
sudo apt install -y libssl-dev libcurl4-openssl-dev zlib1g-dev libyaml-dev libffi-dev
```

それぞれの対象

- openssl の不足: libssl-dev と libcurl4-openssl-dev
- zlib の不足: zlib1g-dev
- psych の不足: libyaml-dev
- fiddle の不足: libffi-dev

不足分のライブラリを追加したらconfigから再実行する  
[実行ログ](log\0925-2_make-0ライブラリ追加後clean前-config.txt)  

```bash
./configure --disable-install-doc
```

##### make(ライブラリ追加後clean前)

この状態でmakeを再実行してもwarningは残ってしまう。
[実行ログ](log\0925-2_make-1ライブラリ追加後clean前-make.txt)  

```bash
make
```

##### config(ライブラリ追加後clean後)

前回のmake結果を削除する

```bash
make clean
```

三度config実行する
[実行ログ](log\0925-3_make-0ライブラリ追加後clean後-config.txt)  

```bash
./configure --disable-install-doc
```

##### make(ライブラリ追加後clean後)

三度makeを実行する
[実行ログ](log\0925-3_make-1ライブラリ追加後clean後-make.txt)  

```bash
make
```

##### インストール

以下コマンドによりインストール
[実行ログ](log\0925-4_make-install.txt)  

```bash
sudo make install
```

##### バージョン確認

以下でインストールの成功を確認する

```bash
komekomekome@DESKTOP-5900X:~/ruby-3.4.6$ ruby -v
ruby 3.4.6 (2025-09-16 revision dbd83256b1) +PRISM [x86_64-linux]
komekomekome@DESKTOP-5900X:~/ruby-3.4.6$
```

#### PostgreSQLの設定

##### postgresqlインストール

必要なパッケージをインストールする

```bash
sudo apt install -y postgresql libpq-dev
```

- postgresql : postgresql本体
- libpq-dev : postgresqlに対するIF。Rubyの依存関係(Gem)をコンパイルする際にこのライブラリが必要になります。

##### 専用ユーザの作成

postgresという管理者権限のユーザにて、"redmine"というユーザを作成する。

```bash
sudo -i -u postgres createuser -P redmine
```

パスワードを設定する。本手順書では分かりやすく以下とするが、実際に利用する場合は設定値・管理に気を付けること。

```plain
PqRmP4ss
```

実際のLogは以下のようになる

```bash
komekomekome@DESKTOP-5900X:~$ sudo -i -u postgres createuser -P redmine
Enter password for new role:
Enter it again:
komekomekome@DESKTOP-5900X:~$
```

##### Redmine用DBの作成

postgresという管理者権限のユーザにて、"redmine"というユーザが権限を有する"redmineDB"というDBを作成する

```bash
sudo -i -u postgres createdb -E UTF-8 -l ja_JP.UTF-8 -O redmine -T template0 redmineDB
```

結果はsimpleに以下

```bash
komekomekome@DESKTOP-5900X:~$ sudo -i -u postgres createdb -E UTF-8 -l ja_JP.UTF-8 -O redmine -T template0 redmineDB
komekomekome@DESKTOP-5900X:~$
```

##### DB作成結果の確認

postgresユーザーでpsqlを起動します。

```bash
sudo -i -u postgres psql
```

これにより、ターミナルが postgres=# に変わる

```bash
komekomekome@DESKTOP-5900X:~$ sudo -i -u postgres psql
psql (16.10 (Ubuntu 16.10-0ubuntu0.24.04.1))
Type "help" for help.

postgres=#
```

データベース一覧を表示するため、list(一覧)を意味するpsql内の特殊コマンド"\l"を実行する

```bash
\l
```

以下結果が得られる。"redmineDB"以外はデフォルトのモノ。  

```bash
postgres=# \l
                                                      List of databases
  Name    |  Owner   | Encoding | Locale Provider |   Collate   |    Ctype    | ICU Locale | ICU Rules |   Access privileges
-----------+----------+----------+-----------------+-------------+-------------+------------+-----------+-----------------------
postgres  | postgres | UTF8     | libc            | C.UTF-8     | C.UTF-8     |            |           |
redmineDB | redmine  | UTF8     | libc            | ja_JP.UTF-8 | ja_JP.UTF-8 |            |           |
template0 | postgres | UTF8     | libc            | C.UTF-8     | C.UTF-8     |            |           | =c/postgres          +
          |          |          |                 |             |             |            |           | postgres=CTc/postgres
template1 | postgres | UTF8     | libc            | C.UTF-8     | C.UTF-8     |            |           | =c/postgres          +
          |          |          |                 |             |             |            |           | postgres=CTc/postgres
(4 rows)

postgres=#
```

確認出来たら終了するため、quit(終了)を意味する"\q"コマンドにより元のBashプロンプトに戻る

```bash
\q
```

#### Redmineのインストール

##### redmineに必要なライブラリ

svnコマンドを利用するのでパッケージをインストールしておく

```bash
sudo apt install -y subversion
```

##### redmineのダウンロード

インストール先フォルダ作成

```bash
sudo mkdir /var/lib/redmine
```

作成したフォルダの所有者を"www-data"に変更する。  
redmineからファイルの読み書き・設定変更などするにあたり、webサーバのユーザ権限と同じにしておく必要があるため。  

```bash
sudo chown www-data /var/lib/redmine
```

"www-data"ユーザの権限で、指定URLからチェックアウトして、指定ディレクトリへ保存する

```bash
sudo -u www-data svn co https://svn.redmine.org/redmine/branches/6.1-stable /var/lib/redmine
```

##### 設定ファイルの作成

以下コマンドでまず、root権限でファイルを作成・編集する

```bash
sudo nano /var/lib/redmine/config/database.yml
```

ファイル内にctrl+Vで以下コピペする。  
パスワードは前述のサンプルをそのまま指定しているので、本番では専用のモノに置き換えること。  

```yaml
production:
  adapter: postgresql
  database: redmineDB
  host: localhost
  username: redmine
  password: PqRmP4ss
  encoding: utf8
```

ctrl+oでファイルをセーブして、ctrl+xで終了する。  
以下でファイル内容を確認する。  

```bash
sudo cat /var/lib/redmine/config/database.yml
```

ファイルの所有者とグループの両方を www-data に設定しておく。

```bash
sudo chown www-data:www-data /var/lib/redmine/config/database.yml
```

以下ファイル一覧を確認して他のファイル群と同じ権限になっていればOK

```bash
ls -la /var/lib/redmine/config
```

##### フォントデータの用意

Redmineで画像生成する際の文字化けを防ぐために必要なパッケージをインストールする。  

```bash
sudo apt install -y imagemagick fonts-takao-pgothic
```

- imagemagick: 画像ファイルの処理や操作を行うためのオープンソースのソフトウェアスイートです。Redmineでは、ガントチャートやグラフ、カレンダーなどを画像として生成するために利用されます。
- fonts-takao-pgothic: Takao Pゴシックという日本語フォントをインストールします。前述のimagemagick(またはその関連ライブラリであるRMagick)が、日本語を正しく画像に描画するためにこのフォントを利用します。IPAフォントライセンスで公開されており商用・無償利用可能。  

以下設定ファイルを用意する。  

```bash
sudo nano /var/lib/redmine/config/configuration.yml
```

ファイル内にctrl+Vで以下コピペする。  

```yaml
production:
  # メール送信機能を使用しないため、email_deliveryセクションは削除

  # 日本語の画像生成のための設定
  rmagick_font_path: /usr/share/fonts/truetype/takao-gothic/TakaoPGothic.ttf 
```

ctrl+oでファイルをセーブして、ctrl+xで終了する。  
以下でファイル内容を確認する。  

```bash
sudo cat /var/lib/redmine/config/configuration.yml
```

ファイルの所有者とグループの両方を www-data に設定しておく。

```bash
sudo chown www-data:www-data /var/lib/redmine/config/configuration.yml
```

以下ファイル一覧を確認して他のファイル群と同じ権限になっていればOK

```bash
ls -la /var/lib/redmine/config
```

##### gemパッケージのインストール

gemパッケージをインストールするため、まずredmineのインストールフォルダへ移動する。

```bash
cd /var/lib/redmine
```

bundlerの設定を変更して、このフォルダ内では不要なgemをインストールしないように変更する

```bash
sudo bundle config set --local without 'development test'
```

同ディレクトリ内の"Gemfile"に従って、必要なgemをインストールする。  
[実行ログ](log\0927-1_sudoでgemをインストール.txt)  

```bash
sudo bundle install
```

##### セッション改竄防止用秘密鍵の作成

Redmineが使用するセッション情報やクッキーの安全性を確保するために、秘密鍵を自動生成する。

```bash
cd /var/lib/redmine
sudo -u www-data bin/rake generate_secret_token
```

- sudo -u www-data: Webサーバーの実行ユーザーである www-data の権限で実行する。
- bin/rake generate_secret_token: Redmine(Rails)の管理ツールであるrakeを使用して、ランダムでユニークな長い文字列(秘密鍵)を生成させる。

##### データベースのテーブル作成

Redmineの動作に必要なデータベースの構造(テーブル)を実際に作成する。
[実行ログ](log\0927-2_テーブル作成.txt)  

```bash
cd /var/lib/redmine
sudo -u www-data bin/rake db:migrate RAILS_ENV="production"
```

- sudo -u www-data: www-data ユーザーの権限で実行する
- db:migrate: Redmineが持つマイグレーションファイル(データベースの構造を定義したファイル群)を読み込み、データベースサーバーに対して「これらのテーブルを作ってください」という命令を実行する。
- RAILS_ENV="production": この操作が本番環境(production)の設定(config/database.ymlで指定したredmineDB)に対して実行するよう指定している

#### Apacheの設定

##### Apache本体のインストール

大前提となるライブラリをインストールする

```bash
sudo apt install -y apache2 apache2-dev
```

##### passengerのインストール

WebサーバーとRedmineを連携させるのに必要なPassenger(パッセンジャー)をインストールする。  
[実行ログ](log\0927-3_passenger.txt)  

```bash
sudo gem install passenger -N
```

- sudo gem install passenger: 管理者権限でRubyのパッケージ管理ツール(gem)を使い、passengerというGemをインストールする。
- -N: ドキュメントの生成(インストール後のヘルプファイル作成など)をスキップする。

##### PassengerのApache用モジュールのインストール

Apache用のモジュールのビルドとインストールを行う。
[実行ログ](log\0927-4_passenger-1apatche後.txt)  

```bash
sudo passenger-install-apache2-module --auto --languages ruby
```

##### Apache用設定の確認

Passengerインストールウィザードで表示されたApache設定用のスニペット(断片的な設定)を再表示する。

```bash
passenger-install-apache2-module --snippet
```

そのまま結果を載せておく。

```bash
komekomekome@DESKTOP-5900X:/var/lib/redmine$ passenger-install-apache2-module --snippet
LoadModule passenger_module /usr/local/lib/ruby/gems/3.4.0/gems/passenger-6.1.0/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
  PassengerRoot /usr/local/lib/ruby/gems/3.4.0/gems/passenger-6.1.0
  PassengerDefaultRuby /usr/local/bin/ruby
</IfModule>
komekomekome@DESKTOP-5900X:/var/lib/redmine$
```

##### Apacheの設定ファイル作成

以下設定ファイルを新規作成する

```bash
sudo nano /etc/apache2/conf-available/redmine.conf
```

ファイル内にctrl+Vで以下コピペする。  

```conf
# Passengerの基本設定。
# passenger-install-apache2-module --snippet で表示された設定を記述。
# 環境によって設定値が異なるため以下の5行はそのまま転記せず、必ず
# passenger-install-apache2-module --snippet で表示されたものを使用すること。
LoadModule passenger_module /usr/local/lib/ruby/gems/3.4.0/gems/passenger-6.1.0/buildout/apache2/mod_passenger.so
<IfModule mod_passenger.c>
  PassengerRoot /usr/local/lib/ruby/gems/3.4.0/gems/passenger-6.1.0
  PassengerDefaultRuby /usr/local/bin/ruby

# 必要に応じてPassengerのチューニングのための設定を追加(任意)。
# 詳しくは Configuration reference - Passenger + Apache (https://www.phusionpassenger.com/docs/references/config_reference/apache/) 参照。
# * PassengerMaxPoolSize: passengerが同時に起動できるプロセス数の上限設定
# * PassengerMaxInstancesPerApp: Redmineが同時に起動できるプロセス数の上限設定
# * PassengerPoolIdleTime: アイドル状態のプロセスを終了させるまでの時間(秒)設定
# * PassengerStatThrottleRate: コード変更をチェックする頻度(秒)
  PassengerMaxPoolSize 20
  PassengerMaxInstancesPerApp 4
  PassengerPoolIdleTime 864000
  PassengerStatThrottleRate 10
</IfModule>
```

ctrl+oでファイルをセーブして、ctrl+xで終了する。  
以下でファイル内容を確認する。  

```bash
sudo cat /etc/apache2/conf-available/redmine.conf
```

##### Apacheの設定更新

Apacheの conf-available ディレクトリにある redmine.conf ファイルを有効化する。  
Passengerの基本設定（LoadModuleなど）をApacheに読み込ませる準備。  

```bash
sudo a2enconf redmine
```

実行結果は以下

```bash
komekomekome@DESKTOP-5900X:/var/lib/redmine$ sudo a2enconf redmine
Enabling conf redmine.
To activate the new configuration, you need to run:
  systemctl reload apache2
komekomekome@DESKTOP-5900X:/var/lib/redmine$
```

Apacheの設定ファイル全体に構文エラーや問題がないかをチェックする。  
設定ミスによるApacheの起動失敗を防ぐための必須の安全確認。  
このコマンドがSyntax OKを返せば問題ない。  

```bash
apache2ctl configtest
```

以下問題ないことが確認できた

```bash
komekomekome@DESKTOP-5900X:/var/lib/redmine$ apache2ctl configtest
Syntax OK
komekomekome@DESKTOP-5900X:/var/lib/redmine$
```

動作中のApacheサービスに対し、設定ファイルのみを再読み込みして、新しい設定(redmine.conf)を反映させる。  
サービスを完全に停止・起動(restart)するよりも高速で、処理中のリクエストを中断させにくい手段。  

```bash
sudo systemctl reload apache2
```

特に結果の出力はない。

```bash
komekomekome@DESKTOP-5900X:/var/lib/redmine$ sudo systemctl reload apache2
komekomekome@DESKTOP-5900X:/var/lib/redmine$
```

#### Apache上のPassengerでRedmineを実行するための設定

URLのサブディレクトリでRedmineにアクセスできるように設定する。  
同じサーバでRedmine以外のアプリケーションを実行する場合や、複数のRedmineを実行できるように備える。  

> 例: http://サーバIPアドレスまたはホスト名/redmine

先の手順で作成したファイルとは別のファイルに設定を追記する。

```bash
sudo nano /etc/apache2/sites-available/redmine.conf
```

ctrl+Vで以下コピペすることで、`redmineKome`がサブディレクトリ名となる。  

```conf
# サブディレクトリ形式でアクセスできるようにする
Alias /redmineKome /var/lib/redmine/public
<Location /redmineKome>
  PassengerBaseURI /redmineKome
  PassengerAppRoot /var/lib/redmine
</Location>
```

アセット(スタイルシートやJavascriptなど)をコンパイルする。  
RAILS_RELATIVE_URL_ROOTに指定するのは先述のサブディレクトリ名となる。  

```bash
sudo -u www-data bundle exec rake assets:precompile RAILS_ENV="production" RAILS_RELATIVE_URL_ROOT=/redmineKome
```

Apacheを再起動する

```bash
apache2ctl configtest
sudo systemctl reload apache2
```

以下URLでアクセスすればredmineのページが表示されるハズ。

```url
http://localhost/redmineKome
```

#### 他PCからの接続

##### TCPポート80による接続確認

###### 一時的に開放

疎通確認のため、一時的にファイアウォールルールを追加する。  
powershellを管理者権限で実行して以下コマンドを実行する。  

```PowerShell
New-NetFirewallRule -DisplayName "WSL2 Apache HTTP 80 (Redmine Temp)" `
  -Direction Inbound `
  -Protocol TCP `
  -LocalPort 80 `
  -Action Allow `
  -Profile Private,Domain `
  -RemoteAddress LocalSubnet
```

疎通確認が完了した後は、セキュリティリスクを避けるため、必ずこのルールを削除する。

###### プロキシ設定前の設定確認

念のため現状の設定を確認しておく。

```PowerShell
PS C:\Users\okome> netsh interface portproxy show all

PS C:\Users\okome>
```

###### hostPCからWSL2へのプロキシ設定

以下コマンドで転送設定を施す

```PowerShell
netsh interface portproxy add v4tov4 listenport=80 listenaddress=192.168.1.14 connectport=80 connectaddress=xxx.xxx.xxx.xxx
```

各オプションの意味は以下の通り。

- netsh interface portproxy: Windowsのポートプロキシ機能を使用してトラフィックを転送する。
- add: 新しいルールを追加
- v4tov4: IPv4からIPv4への転送（他にv4tov6, v6tov4, v6tov6も可能）
- listenport=80:  
  Windowsが受信を待機するポート番号の指定。  
  ポート80で外部からの接続を受け付ける。  
  例: 他のPCから`http://192.168.1.14:80`でアクセスされた際に反応する  
- listenaddress=192.168.1.14: Windowsが受け付けるIPアドレスの指定  
- connectport=80: 転送先のポート番号の指定。WSL2のApacheが動作しているポート80に転送する。  
- connectaddress=xxx.xxx.xxx.xxx: 転送先のIPアドレスの指定。WSL2の仮想IPアドレスに転送する。`hostname -I`で確認できる。  

###### プロキシ設定後の設定確認

問題なく設定できていることを確認しておく。
☆

```PowerShell
PS C:\Users\okome> netsh interface portproxy show all

ipv4 をリッスンする:         ipv4 に接続する:

Address         Port        Address         Port
--------------- ----------  --------------- ----------
192.168.1.14    80          xxx.xxx.xxx.xxx 80

PS C:\Users\okome>
```

###### 他PCからの接続確認

以下コマンドを同一LAN内の他PCから実行することで、疎通確認する。  

```PowerShell
Invoke-WebRequest -Uri "http://192.168.1.14" -Method Head
```

上記でエラーがでなければ、WebブラウザからWSL2上のRedmineにアクセスできるハズ。

###### 設定削除

ポートプロキシの設定を削除する

```PowerShell
netsh interface portproxy delete v4tov4 listenport=80 listenaddress=192.168.1.14
```

ファイアウォールのルールを削除する

```PowerShell
Remove-NetFirewallRule -DisplayName "WSL2 Apache HTTP 80 (Redmine Temp)"
```

削除できたことを確認する

```PowerShell
netsh interface portproxy show all
Get-NetFirewallRule -DisplayName "WSL2 Apache HTTP 80 (Redmine Temp)" -ErrorAction SilentlyContinue
```

以下のようにいずれも何も出力が出なければOK。

```PowerShell
PS C:\Users\okome> netsh interface portproxy show all

PS C:\Users\okome> Get-NetFirewallRule -DisplayName "WSL2 Apache HTTP 80 (Redmine Temp)" -ErrorAction SilentlyContinue
PS C:\Users\okome>
```

## HTTPS接続

HTTPS(SSL/TLS暗号化通信)を利用してRedmineにアクセスできるようにする。

### SSL証明書の準備

自己署名証明書を作成して設定する。

#### SSL証明書に必要なパッケージのインストール

既に存在しているので不要なハズだが、足りなければ以下実行する。

```Bash
sudo apt update
sudo apt install -y openssl
```

#### 自己署名証明書の作成

以下のコマンドを実行して、秘密鍵(redmine.key)と証明書ファイル(redmine.crt)を生成する。

```Bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/redmine.key -out /etc/ssl/certs/redmine.crt
```

- sudo openssl:  
  OpenSSLプログラムを実行  
  暗号化通信(SSL/TLS)に必要な鍵や証明書を作成・管理するためのツール本体
- req:  
  証明書要求(CSR)を管理  
  証明書を作成するためのサブコマンド。  
  このコマンドを単体で使うと「証明書署名要求」（CSR）を作成するが、-x509オプションと組み合わせることで自己署名証明書を作成する。  
- -x509:  
  自己署名証明書として生成  
  通常の証明書(CAに署名してもらうもの)ではなく、自分自身で署名した証明書(自分自身が認証局となる)を作成するよう指定します。  
- -nodes:  
  秘密鍵を暗号化しない  
  no des の略で、生成される秘密鍵(redmine.key)をパスフレーズなしの平文で保存するように指定する。これにより、Webサーバー(Apache)起動時に毎回パスフレーズの入力を求められるのを防ぐ。セキュリティ上は推奨されないが、サーバーの自動起動には必須となる。  
- -days 365:  
  証明書の有効期間  
  作成する証明書の有効期限を365日間(1年間)に設定する。この期間が過ぎると証明書は期限切れとなり、ブラウザでアクセスできなくなる。  
- -newkey rsa:2048:  
  秘密鍵のアルゴリズムとサイズ  
  新しい秘密鍵を生成するよう指定し、アルゴリズムに RSA を、鍵長に 2048ビット を指定する。現在のセキュリティ標準を満たす鍵長です。  
- -keyout ... :  
  秘密鍵の出力先  
  生成された秘密鍵ファイルを /etc/ssl/private/redmine.key に保存する  
- -out ... :  
  証明書の出力先
  生成された証明書ファイルを /etc/ssl/certs/redmine.crt に保存する

コマンド実行後に表示される対話形式のログは以下の通り。

```Bash
Country Name (2 letter code) [AU]:JP
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:192.168.1.14
Email Address []:
komekomekome@DESKTOP-5900X:~$
```

Common Nameには、Apacheの設定で決めたアクセス名を入力する。  

#### 秘密鍵のパーミッション設定

秘密鍵ファイルは、rootだけが読み書きできるようにアクセス権限を設定する。

```Bash
sudo chmod 600 /etc/ssl/private/redmine.key
```

### ApacheでのHTTPS設定(ポート443の有効化)

Apacheにポート443で待ち受ける設定を追加し、作成した証明書を読み込ませる。  

#### SSLモジュールの有効化

以下コマンドでApacheのSSLモジュールを有効化する

```Bash
sudo a2enmod ssl
```

以下コマンドを実行し、'Listen 443'が記載されていることを確認する。  

```Bash
sudo nano /etc/apache2/ports.conf
```

```conf
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```

#### Redmine用バーチャルホストの修正(HTTPS設定の追加)

/etc/apache2/sites-available/redmine.conf ファイルに、HTTPS用の`<VirtualHost>`ブロックを追加する。

```Bash
sudo nano /etc/apache2/sites-available/redmine.conf
```

ファイルの一番下に、以下の設定を追記する。  
ポート番号が443であることと、SSL関連のディレクティブがあることがポイントです。

```conf
# HTTPS (SSL) 用のバーチャルホスト設定
<VirtualHost *:443>
  ServerName 192.168.1.14

  # Redmineの公開ディレクトリを指定
  DocumentRoot /var/lib/redmine/public

  # サブディレクトリ形式でアクセスできるようにする
  Alias /redmineKome /var/lib/redmine/public
  <Location /redmineKome>
    PassengerBaseURI /redmineKome
    PassengerAppRoot /var/lib/redmine
  </Location>

  # HTTPSの有効化と証明書の指定
  SSLEngine on
  SSLCertificateFile    /etc/ssl/certs/redmine.crt
  SSLCertificateKeyFile /etc/ssl/private/redmine.key

  # Passengerの設定(HTTPと同じ)
  PassengerAppRoot /var/lib/redmine

  # Redmineの公開ディレクトリへのアクセス許可 (HTTP設定と共通)
  <Directory /var/lib/redmine/public>
    Require all granted
    Options -MultiViews
  </Directory>
</VirtualHost>
```

#### 設定の有効化とApacheの再起動

新しいバーチャルホスト設定を有効化し、Apacheを再起動して設定を反映させる。

まずsites-available の設定ファイルを有効化

```Bash
komekomekome@DESKTOP-5900X:~$ sudo a2ensite redmine.conf
Enabling site redmine.
To activate the new configuration, you need to run:
  systemctl reload apache2
komekomekome@DESKTOP-5900X:~$
```

次に構文チェック。権限が足りないと見えないファイルもあるので注意。  

```Bash
komekomekome@DESKTOP-5900X:~$ sudo apache2ctl configtest
Syntax OK
komekomekome@DESKTOP-5900X:~$

```

Apacheを再起動して設定を反映する

```Bash
komekomekome@DESKTOP-5900X:~$ sudo systemctl restart apache2
komekomekome@DESKTOP-5900X:~$
```

### 自PCからのアクセス確認

#### 実際にアクセス

Apacheが正常に再起動したら、ブラウザから以下のURLでアクセスする。

[https://localhost/redmineKome/]
[https://[ホストPCのIPアドレス]/redmineKome/]

#### アクセス時の注意点

自己署名証明書を使用しているため、ブラウザでアクセスすると「この接続はプライベートではありません」や「証明書が無効です」といったセキュリティ警告が表示される。  
「詳細設定」や「危険を承知で続行」などを選択してアクセスを続行する。  

### 証明書の期限が切れた・切れる前に

証明書は有効期限(今回の場合365日)が設定された一時的なファイルであり、期限が切れたら古いものを破棄して新しいものに交換する必要がある。  

#### 新しい証明書ペアの生成

初回実行したコマンドと全く同じコマンドを実行する。  
これにより、新しい秘密鍵と証明書が生成され、古いファイルが上書きされる。  

↑`自己署名証明書の作成`で実行したコマンド

#### 秘密鍵のパーミッション確認

秘密鍵のアクセス権限を再度設定する。  

↑`秘密鍵のパーミッション設定`で実行したコマンド

#### Apacheの設定更新と再起動

更新した証明書ファイル(.crt)と秘密鍵ファイル(.key)をApacheに読み込ませるために、サービスを再起動する。

```Bash
# 構文チェック
apache2ctl configtest
# Apacheを再起動して新しい証明書を読み込む
sudo systemctl restart apache2
```

### 他PCからのアクセス確認

管理者権限で起動したWindowsTerminalから以下コマンド実行する。  

```PowerShell
PS C:\Users\okome> New-NetFirewallRule -DisplayName "WSL2 Apache HTTPS 443 (Redmine)" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow -Profile Private,Domain

Name                          : {737dc2fd-adca-4616-90ac-ac6352cb8a4f}
DisplayName                   : WSL2 Apache HTTPS 443 (Redmine)
Description                   :
DisplayGroup                  :
Group                         :
Enabled                       : True
Profile                       : Domain, Private
Platform                      : {}
Direction                     : Inbound
Action                        : Allow
EdgeTraversalPolicy           : Block
LooseSourceMapping            : False
LocalOnlyMapping              : False
Owner                         :
PrimaryStatus                 : OK
Status                        : 規則は、ストアから正常に解析されました。 (65536)
EnforcementStatus             : NotApplicable
PolicyStoreSource             : PersistentStore
PolicyStoreSourceType         : Local
RemoteDynamicKeywordAddresses : {}
PolicyAppId                   :
PackageFamilyName             :

PS C:\Users\okome>
```

以下でHost側の設定を確認する  

```PowerShell
PS C:\Users\okome> Get-NetFirewallRule -DisplayName "WSL2 Apache HTTPS 443 (Redmine)" | Format-Table DisplayName, Enabled, Direction, Action, LocalPort, Profile -AutoSize

DisplayName                     Enabled Direction Action LocalPort         Profile
-----------                     ------- --------- ------ ---------         -------
WSL2 Apache HTTPS 443 (Redmine)    True   Inbound  Allow           Domain, Private

PS C:\Users\okome>
```

Ubuntu側での待機状態を確認する

```bash
komekomekome@DESKTOP-5900X:~$ sudo ss -tuln | grep 443
tcp   LISTEN 0      511                 *:443              *:*
komekomekome@DESKTOP-5900X:~$
```

host側で以下設定をする。  
UbuntuのIPは`hostname -I`で確認できる。  
☆

```PowerShell
netsh interface portproxy add v4tov4 listenport=443 listenaddress=192.168.1.14 connectport=443 connectaddress=xxx.xxx.xxx.xxx
```

## Redmine設定

### 初期設定

#### 管理者パス設定

初回時は管理者ユーザのみ。  
ログインページより、ユーザ・パス共にadminでログインする。  

その後、画面表示に従ってパスワードを変更する。  
☆

> 

#### デフォルトデータのロード

画面上部のメニューより"管理"をクリックする。  
以下メッセージが表示されていれば、"デフォルト設定をロード"をクリックする。  

```plain
ロール、トラッカー、チケットのステータス、ワークフローがまだ設定されていません。
デフォルト設定のロードを強くお勧めします。ロードした後、それを修正することができます。
```

以下表示されればOK。

```plain
デフォルト設定をロードしました。
```

#### 言語設定・ユーザ名表示形式の変更

画面上部のメニューより"管理"をクリックする。  
画面左から"設定"をクリックする。  
タブから"表示"をクリックする。  
"デフォルトの言語"に"Japanese(日本語)"を選択する。  

同画面よりユーザ名の表示形式も変更する。
"ユーザー名の表示形式"に"Admin Redmine"を選択する。  
ユーザ名の設定を変更していないので、上記選択肢が姓→名の順を意味する。  

"保存"ボタンをクリックする。  

#### 文字コードの自動判別設定

画面上部のメニューより"管理"をクリックする。  
画面左から"設定"をクリックする。  
タブから"ファイル"をクリックする。  
"添付ファイルとリポジトリのエンコーディング"に"UTF-8, CP932, EUC-JP, Shift_JIS, SJIS"を選択する。  

設定できるエンコードは以下から確認できる。

```bash
ruby -e "puts Encoding.name_list"
```
