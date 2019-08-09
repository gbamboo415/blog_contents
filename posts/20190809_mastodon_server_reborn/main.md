---
Keywords: マストドン, CentOS
Copyright: (C) 2019 青竹
---

# マストドンサーバを吹き飛ばして、9時間かけて構築しなおした話

## 私的マストドンの経歴

Twitter界の同志のみなさん、マストドン (Mastodon) を覚えていますか？　マストドンはTwitterと同じマイクロブログサービスに分類されるSNSで、「分散型」のSNSの実装のひとつです。
私は、第二次マストドンブーム（2017年4月）の時に「mstdn.jp」にアカウントを作成し、使ったり使わなかったりしていました。

2018年8月、とある方の勧めにより、AWS (Amazon Web Services) 上でマストドンサーバ（当時はインスタンスと呼んでいました）を立ち上げてみました。当時もかなり悪戦苦闘したと記録にあります。その時はTwitterが危機的状況だと騒がれていました。もしTwitterが斃れた時は、鉄道好きが集まるインスタンスにしようと思っていました。結局はTwitterが回復したこともあり、そのマストドンサーバは数か月で閉鎖しました。

時は流れて2019年5月下旬、Twitter何回目かの危機にして第三次マストドンブームの時に、今度は自分専用のマストドンサーバを立てようと思い立ちました。自分専用のサーバ（=自分1人しかいない）ことにも意味があるのが、マストドンの最大の特徴ともいえます。自分のサーバなので何もかもが自由自在、それでいて、自分以外のサーバの利用者を「リモートフォロー」できますので、寂しい思いをすることもありません。この時は本格的にドメインも取り、本運用していました。

## もともと存在した計画

それから2か月半経って、AWSの無料利用枠を超えてしまい、経費が嵩むようになってきたので、別のVPSにサーバを移すことを考えるようになりました。別のレンタルサーバで運営していたウェブサイトもそのVPSに統合し、統一的に扱うことを考えたのです。もともとDockerイメージでマストドンを運用していたので、丸ごと新しいサーバに引っ越すことができる可能性がありました。

## 第一の事件発生→復旧作業

しかし、ここで問題が発生しました。AWSインスタンスにアクセスするための秘密鍵を紛失してしまっていたのです。これではデータを移せません。幸いにして、その場合の復旧手順が示されていたので、その手順に従って、新しい鍵ペアを作成して鍵を書き換える作業を始めました。幸い、順調に作業は進められました。

最後に、一時作業用インスタンスを破棄――

## 第二の事件発生

――すると、なぜかマストドンのサーバを展開しているインスタンスまで「Terminating (破棄開始)」と出たではありませんか。「まずい」と思う間もなく、ステータスは「Terminated (破棄完了)」に変わってしまいました。急いでヘルプを漁りましたが、破棄されてしまったインスタンスを救助する手段は存在しないとのことでした。

やっちまったぜ。

## 再構築トライアル 1

正直なところ、これをきっかけにマイマストドンサーバをやめてしまおうかとも考えました。でも、情報技術者のはしくれとして、サーバを構築してみたいという欲が勝って、サーバを立て直すことにしました。

これから新たに使うVPSのコンソールにログインし、インスタンスのスペックとOSイメージを選択……しようとしたら、OSイメージのバージョンが古いものしかありませんでした。古いならば自分の手でアップグレードすればええやんけ、そう思って、OSイメージはDebian 7を選びました。なに、バージョン10に上げれば万事OKさ。

結果から述べると、インスタンスがぶっ壊れてしまい、削除せざるを得なくなりました。アップグレードの途中でglibcと何かがバージョン衝突を起こし、そこでリブートしてしまったため、すべてが動かなくなってしまいました。インスタンスをつぶしても、1日あたりの契約料金31円が請求されます。南無三！

ここまでの所要時間、3時間。

## 再構築トライアル 2

古いOSはできるだけ使いたくないので、CentOS7を選ぶことにしました。

当初は、AWSの時と同じ、Dockerを用いてサービスを構築しようとしました。しかし、なぜか動きません。動かないならば、Dockerに頼らない方法で構築する必要があります。

問題となったのは、マストドンサーバ構築に推奨されていたOSは、Ubuntu Server 18.04であったことでした。CentOS7ですと、パッケージ名称が異なっているものが多く、公式手順書をコピペして済ませることができません。仕方がないので、パッケージを要求されてセットアッププロセスがエラー終了するたびに、必要なパッケージを導入する作戦に打って出ました。

コマンドを打ち直したりすること6時間、ついにマストドンを復活させることができました。

## 再構築トライアル3 with Docker

後からわかったことですが、Dockerが動かなかったのは、カーネルのバージョンが古いのが原因でした。カーネルのバージョンを上げようにも、当初用いたVPS自身がコンテナ型のサービスで提供されていて、カーネル操作ができないと明示されていました。問題の原因がわかってめでたしめでたし……とも言えません。マストドンのアップデートが頻繁に行われていて、セキュリティや他サーバとの連携の観点から、アップデートにできるだけ追従することが推奨されています。そのためには、できればサービスをDockerで動かしたい。

そこで目をつけたのが、同じVPS事業者が提供していたもうひとつのVPSサービスでした。こちらはOSのアップデートやカーネル操作に関わることが幅広く許可されていました。しかもスペックを調整すると、もともとのコンテナ型VPSを使うよりも若干安くできることがわかりました。……よし、作り直そう。

新たに構築するにあたり、OSはDebian 10を選ぶことにしました。しかし、サーバイメージはDebian 8が最新であったため、自力でアップグレードしました。DockerとDocker Compose、Nginxを入れたら後はDockerの力でほぼ自動で環境構築ができます。もっとも、ネット上の文献は2017年頃のものがほとんどで、最新の情報を得るのには苦労しました。公式のドキュメントすらなかったので、わりと困りました。後続の人のために、構築手順を文書化しようと思います。

今度は1時間ほどで環境構築が完了し、再びマストドン環境を復活させることに成功しました。

## おまけ: マストドンの将来

マストドンは、TwitterやFacebookのような大手商業SNSほどの規模はないとはいえ、一定の利用者を確保しました。分散型という特徴が活かされることにより、いくつかの商業サーバが閉鎖されても、一気に衰退することはなく、個々のサーバは独立して稼動し、連合により交流活動しています。栄枯盛衰は世の定め、栄えたものはいつかは衰退しますが、マストドンの衰退はまだまだ先のことであると思っています。

マストドンのシステムは結構高度で複雑なので、システム構築・管理・維持の観点からは、シンプルなマイクロブログSNSシステムが作れないかなと思っています。……bashSNSって可能なんでしょうか。