---
Keywords: Vim
Copyright: (C) 2019 青竹
---

# Vim Kaoriya Patchを調べてみた

Vimを使う日本人の8割がお世話になっているであろう、KoRoNさん製作の香り屋版Vimは素晴らしいプロダクトです。私もお世話になっています。

最近、Vimを自分でビルドして最新のものを利用することにはまっておりまして、その際には、KoRoNさんが公開していらっしゃる香り屋版diffパッチを自分で適用することになります。その時の手順をQiitaに投稿したりもしました。

さて、この記事を書いている時点での最新版は、Vim 8.1.1836です。まだ香り屋版パッチが公式サポートしていないバージョンですが、物は試しとパッチを適用しました。結果は失敗でした。修正すべき部分が私の力量を超えてしまっていました。そのため、この記事は、香り屋版パッチを適用せずにLinux Mint上でビルドしたVimで書いています。

パッチが更新されるまで座して待つのは受け身すぎてよくありません。ユーザのはしくれたる者、パッチを自力で更新してプルリクエストを送る心意気でいきたいものです。そのためには、KoRoNさんが書かれたパッチの概要を把握する必要があります。この記事はそのメモです。基本的にソースコード先頭のコメントを読むだけですので、あまり内容はありません。

## パッチ内容の調査

### 0000-kaoriya_marks.diff

Vimのバージョン情報の機能表示に、「+kaoriya」を追加します。

### 0001-rich_icon.diff

Vimのアイコンを高解像度にします。バイナリパッチという形態になっているそうです。

### 0100-embed_manifest.diff

Microsoft Visual C++ 環境において、EXEファイルやDLLファイルを生成する時に、利用可能なマニフェストファイルを埋め込むようです。

### 2001-improve_modelines.diff

ファイルフォーマット、ファイルエンコーディング、バイナリ読み込みの強制指定に対応……するパッチでしょうか？

### 2002-windows_transparency.diff

Windows 環境において、画面透過を実装するパッチです。

### 2003-charspace.diff

Windows GUI 環境において有効な、文字間の水平方向のスペース調整オプションを導入するパッチです。

### 2004-caption_switch.diff

GUIのオプションに、'C'オプションを追加します。これは、ウィンドウのキャプションを表示するかどうかを決めるものです。

### 2005-ambiwidth_auto.diff
### 2006-init_winsock.diff
### 2008-sentence_until_punctuation.diff
### 2010-imaf_imsf_on_gui.diff
### 2520-migemo_feature.diff
### 2530-guess_encode_feature.diff
### 4001-ftp_autolink_2html.diff
### 4002-japanese_punctuation_javadoc.diff
### 4003-disable-irreparable-settings.diff
### X011-GH592-fix-height-adjustment.diff
### python2-warn.diff

## これからすべきこと

