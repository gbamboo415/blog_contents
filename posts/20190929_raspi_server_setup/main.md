---
Keywords: Raspberry Pi, サーバ
Copyright: (C) 2019 青竹
---

# Raspberry Pi サーバセットアップ悪戦苦闘日記

先日，Raspberry Pi 3 Model B+を導入し，手持ちの1TB USBバスパワーHDDと組み合わせてテスト運用をしていました．しかし，バスパワーによる給電が不安定であったことと，HDDにエラーが頻発するようになったことにより，新しいHDDへの切り替えを実行しました．その際，かなり悪戦苦闘しましたので，後に続く人 (主に未来の自分) のためにメモを残す次第です．

## 使ったもの

- Raspberry Pi: Raspberry Pi 3 Model B+
- 旧HDD: 1TB USB バスパワー (USBから給電) HDD
- 新HDD: 4TB USB セルフパワー (ACアダプタから給電) HDD
- Linux ノートパソコン: Linux Mint搭載

## Trial 1: ディスククローニングによる環境移行 → 失敗

参考サイト: http://toranosuke.hatenablog.com/entry/2018-0913_disk-clone-linux

環境をそっくりそのまま移行できれば，それに越したことはありません．そのため，まずはddコマンドを使ってディスクを丸ごとクローニングすることにしました．クローニングしたあとにパーティションを拡張すればいけるはず……

ここで，旧HDDは /dev/sda，新HDDは /dev/sdb であるとします．

```
# dd if=/dev/sda of=/dev/sdb bs=64k conv=noerror,sync
```

ddコマンドを実行して待つこと2時間半，ディスクのクローニングが成功しました．次はGPTテーブルの修正っと……なんか変なメッセージが出てるけどまあヨシッ (現場猫のポーズ) ，手順どおり進めよう……

盲目的にマニュアルに従った末路は，大失敗と相場が決まっています．こともあろうに，MBR (マスターブートレコード) 方式で記録されたクローンディスクにGPTテーブルを書き込んでしまったので，当然ファイルシステムはぶっ壊れるわけです．パーティションのリサイズをしようと，GPartedを開いた時に愕然としました．ファイルシステムが存在しない……？？？

**結果: 失敗．**

この手順で行われた実質的内容は，新しいHDDに2時間半かけて，1TBものゴミデータを書き込んだだけでした．

## Trial 2: HDDへのRaspbian新規セットアップ → 成功……？

ディスククローニング作戦が失敗したので，新規インストールするより他にありません (と，この時は思い込んでいました)．インストーラの画面で，HDDの空き容量が2TBと表示されていましたが，何かの間違いだろうと思ってそのままインストール作業を続行しました．

インストールはうまくいきました．うまくいきましたが……4TBのディスクであるにもかかわらず，2TB分しかパーティションが作成されておらず，残る2TBは未割り当てとなっていました．一括で確保したかったのに面倒だなあ，まあいいか，別パーティションとしてマウントすれば……あれ？

そう，残る2TB分をどうやっても認識させることができませんでした．

**結果: 失敗．**

## Trial 3: MBRからGPTへの書き換え → 失敗

調べていくと，どうやらHDDのファイルシステム管理方式がMBR (マスターブートレコード) 方式になっているらしいということがわかりました．この方式ですと，認識できるディスクの最大容量が2TBになってしまうとのことでした．そしてGPT (GUIDパーティションテーブル) ならば2TB越えが可能であるということも知りました．

それなら，とMBRをGPTに書き換える手順を試して無事成功させ，さっそくシステムリブート……

すると，起動しなくなりました．終了．

**結果: 失敗．**

## Trial 4: Trial 2のやり直し → 成功するも不満が残る

結局，再インストールしました．しかし2TBをドブに捨てるなどもったいなさすぎます．旧HDDを復活させることも考えましたが，エラーが発生していたのを解決したくて新HDDを買ったのですから，これでは目的を達成できません．なんとかして4TB丸ごと使えないか……

## Trial 5: システム構成変更・再インストール → 成功

### Trial 5-1: microSDへのインストール → 成功

### Trial 5-2: マウントポイント移動 → 失敗

### Trial 5-3: マウントポイント見直し → 成功

## 最終結果