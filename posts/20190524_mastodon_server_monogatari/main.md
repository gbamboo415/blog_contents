---
Keyword: 
Copyright: (C) 2019 青竹
---

# マストドン (Mastodon) サーバを構築した話 (物語編)

みなさんは「マストドン (Mastodon)」をご存じでしょうか。ドイツのプログラマ、オイゲン・ロホコ氏によって2016年10月に公開されたマイクロブログサービスで、日本では2017年春と2018年夏に大きく盛り上がりました。私は2017年春に「mstdn.jp」にアカウントを作成し、2018年夏には試験的にサーバを構築し、1か月ほど運用しました。そしてこのたび、ふと思い立って、マストドンサーバを再び立ち上げました。

構築手順については、ウェブ上で検索して得られた情報をもとに、AWSのEC2インスタンス上にDockerを用いて構築しました。ウェブ上に書かれた構築記事のほとんどは2017年春の時点での情報であり、それから2年経った2019年春の時点では、少し手順が変わっている部分がありました。詳細な手順については、忘れないうちにメモ程度の記事を上げておこうと思います。**したがって、この記事には構築に役立つ情報はありません。ごめんなさい。**

Dockerイメージのビルドにすさまじく時間がかかったり、画像アップロード関係のエラーを解析するのに多大な時間を要したり、DNS設定を間違えたために、当雑記帖サイト全体にアクセスできなくなっていることに長時間気付かなかったりと、あらゆることに長い時間をかけて、ようやく正しく稼動させることができました。今後、ドメイン移管とDNS再設定のため、また一時的にサイトが表示できなくなるかもしれませんが、その節はご了承ください。

ちなみに、マストドンのアカウントは、[@aotake91@mstdn.aotake91.net](https://mstdn.aotake91.net/@aotake91) です。なお、当サーバは自分専用として構築しておりますため、もしフォローされる場合は、mstdn.jp や pawoo 、もしくは自分用サーバに作ったアカウントなどから、リモートフォローください。
