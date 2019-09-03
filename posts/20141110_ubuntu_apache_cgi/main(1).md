---
Keyword: 
Copyright: (C) 2019 青竹
---

# Ubuntu上のApacheでCGIを実行できるようにするメモ

大学院の講義実習で用いるシステムを構築するために、Ubuntu上でApacheサーバーを立ち上げ、さらにその上でCGIを実行できるようにしようとしてかなり時間を取られたので、その手順をメモしておきます。

# 手順概略

1. ディレクトリを作る。
2. Apacheにモジュール (userdir, cgid) を組み込む。
3. 設定ファイルを書き換える。
4. Apacheを再起動する。

ファイルの書き換え配置を簡単にするために、ホーム配下にpublic_htmlディレクトリを作り、/~(ユーザ名)/でアクセスできるようにしました。

# 詳しい手順

## ディレクトリを作る

ホームディレクトリ配下に、public_htmlディレクトリを作ります。また、読み書き実行の権限を適切に設定します。

```
$ mkdir public_html $ chmod 755 public_html
```

## Apacheモジュール (userdir, cgid) を有効化する

Ubuntuでは、Apacheの設定ファイル (.conf) が目的別に分割されており、a2enmod コマンドを用いることでモジュールの有効・無効を簡単に切り替えることが出来ます。（行っていることは、設定ファイルの読み込み元ディレクトリに、別ディレクトリにある設定ファイル群へのシンボリックリンクを張る手続きのようです。）
userdir モジュールは、各ユーザのホームディレクトリ配下に用意したWebページ公開ディレクトリを有効にします。
cgidモジュールは、ApacheサーバーでのCGI実行を可能にします。UbuntuではデフォルトでCGIが無効になっているようですので、このモジュールを明示的に組み込む必要があります。（一番はまったのがここでした。）

```
$ sudo a2enmod userdir $ sudo a2enmod cgid
```

なお、モジュールを無効にするには、「a2dismod (モジュール名)」コマンドを実行すればできます。

## 設定ファイルを書き換える

設定ファイルを書き換えて、ホームディレクトリ配下のpublic_htmlディレクトリにあるCGIを実行できるようにします。
有効になっているモジュールの設定ファイルは、/etc/apache2/mod-enabled ディレクトリにシンボリックリンクが張られています。
その設定ファイルをエディタで編集します。
具体的には、

* mime.conf を編集して、.cgi の拡張子を持つファイルを cgi-script として処理するよう設定する。
* userdir.conf を編集して、public_html ディレクトリに ExecCGI Option を付加する。

を行います。

```
$ cd /etc/apache2/mod-enabled $ sudo vi mime.conf 220行目付近の AddHandler 行頭の#を削除し、後ろに.cgiを付加 #AddHandler cgi-script AddHandler cgi-script .cgi $ sudo vi userdir.conf Options にExecCGIを書き加える。 <Directory /home/*/public_html> AllowOverride All Options ExecCGI MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec ^^^^^^^ <Limit GET POST OPTIONS> Allow from all </Limit> <LimitExcept GET POST OPTIONS> Require all denied </LimitExcept> </Directory>
```

## Apacheを再起動する

Apacheの再起動は、次のコマンドで出来ます。

```
$ sudo service apache2 restart
```

もしくはこちら。

```
$ sudo /etc/init.d/apache2 restart
```

