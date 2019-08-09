---
Keyword: 
Copyright: (C) 2019 青竹
---

# 西鉄電車車番検索 (β版) 公開しました

先日、「近鉄車番検索」というWebアプリ ([https://arkw.net/products/web/kintetsu/](https://arkw.net/products/web/kintetsu/)) が公開されているのを知り、西鉄電車でも似たようなものが作れないかと考え、思い立ったが吉日とばかりに取り急ぎWebアプリらしきものを作ってみました。

「西鉄電車車番検索」(β版) は、こちらよりアクセスください。これから表示面などの改良を行ってまいります。ご意見ご感想などお待ちしております。 → [https://shellscript.aotake91.net/nnr-search.htm](https://shellscript.aotake91.net/nnr-search.htm)

当Webアプリの技術的側面は、「続きを読む」からご参照ください。

当Webアプリは、フロントエンドは手書きのHTML・CSS、バックエンドは**シェルスクリプト**によるCGIとテキストファイルを用いております。入力された車番をキーワードとして、テキストファイルをgrepし、その結果をそのまま返すという、単純明快な力技です。その仕様上、正規表現による検索も可能です。フロントエンド～バックエンド間はAjaxにより、クエリ送信と結果の受信を行っています。XMLHttpRequestを直接利用しているため、依存しているJavaScriptライブラリはありません。

