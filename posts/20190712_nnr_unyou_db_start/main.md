---
keyword: 鉄道, 西鉄, 車両運用データベース
Copyright: (C) 2019 青竹
---

# 西鉄電車 車両運用データベース、始動

7月10日より、「西鉄電車 車両運用データベース」を運用開始しました。[こちら](https://nnr-potato.xyz)からどうぞ。

## はじめに

このデータベースは、Twitterの「西鉄運用メモ」タグに日々お寄せ頂いている、西鉄電車の運用見たまま情報を、再利用可能な形で収載すること目的としています。私はTwitterのこのタグの創始者ではありませんので、いち利用者としてタグ投稿時の書き方の目安をお願いすることはあっても、一通りの形式を強制することはできません。実際、一定の記述法に従いつつも、列車番号を用いたり用いなかったり、区切り記号の書き方が異なっていたり、併結編成をどの順序で記述するかといったことが、人により様々となっています。のちのち利用しやすくするためには、お寄せ頂いた情報を、一定の規則形式に従って書き直すことが重要だと考えています。

そこで、私は以前からかねて「西鉄電車の車両運用情報のデータベースを作りたい」と考えてきました。2018年末には、運用情報をテキストファイルとして日々更新し、ホームページサーバに置いて提供することを思いつき、実際に[サイトまで作りました](https://aotake91exp.soragoto.net)。しかし、更新手続きが煩雑であったことと、一覧性に乏しいことが災いし、実験用に用意したデータを公開したところで止まってしまいました。それからしばらく経ち、今度はリレーショナルデータベース (MySQL) + PHPという、データベースサイトを作る場合には定番の要素技術を用いて、データベースサイトを立ち上げました。これが6月上旬の話です。しかしこれも、データ更新のための手続きが煩雑になってしまい、立ち上げ後すぐに放置状態となりました。

どうにかして、データを楽に入力し、頑健に保存しつつ、簡単に閲覧できるデータベースサイトの仕組みを作り上げたい。そんな時に出逢った本が、『フルスクラッチから1日でCMSを作る シェルスクリプト高速開発手法入門 改訂2版』([Amazonへのリンク](https://www.amazon.co.jp/dp/4048930699/ref=cm_sw_r_tw_dp_U_x_Bs9jDb3V6T122 )) ([hontoへのリンク](https://honto.jp/netstore/pd-book_29682139.html)) でした。この本の初版はすでに保有していましたが、大幅に改訂された2版を読んでみて、これぞデータベースサイトの構築にふさわしい手法だと考えました。それには、私がシェルスクリプトに少しは慣れていたという背景もありましたが。

## サイト構築の概要

基本は、『シェルスクリプト高速開発手法入門 改訂2版 』の内容に従って、サイトのシステムを構築していきました。GitHubにデータ保存・管理用の公開リポジトリを置き、そこに連動する形でサイトの表示内容を更新するという形です。この形をとることにより、データはGitHubとサイト公開用のレンタルサーバ（+さらにはローカルのコンピュータ）に、同期されながら存在することになります。また、サイトの構造と内容を分離でき、サイトの内容を、あらかじめ許可した人に更新してもらうことが容易に可能となります。

データはテキストファイル、YAML形式で持つようにしました。YAML形式とはいっても難しいものではなく、「西鉄運用メモ」としてお寄せ頂いている情報の書き方に極力のっとり、簡単なスクリプトで変換できるようにこの形式を選択しました。

```
2063: 5125F+5128F
A071: 5033F+5126F
2065: 6051F
G073: 9104F+9002F+9109F
```

上記の例のように、行頭に列車番号を置き、コロンと半角スペースで区切りを入れ、その後に編成情報を記述する形としています。YAMLの規格に従って定義すると、列車番号をキー、編成情報を値としたマッピングにより記述しています。

## 今後の目標

現在は、列車番号と編成情報の対応を記録しているだけで、「運用と編成情報の対応」を求めるためのプログラムは整備しておりません。そのプログラムを整備し、また、検索機能を設けることにより、より有用なデータベースサイトにしていきたいと考えています。
