<!--
Copyright © 2012-2023 jrnl contributors
License: https://www.gnu.org/licenses/gpl-3.0.html
-->

# はじめに

## インストール

`jrnl`をインストールする最も簡単な方法は、[Python](https://www.python.org/) 3.10以上で[pipx](https://pipx.pypa.io/stable/installation/)を使用することです：

```sh
pipx install jrnl
```

!!! tip
`jrnl`のインストール時に`sudo`を使用しないでください。パスの問題が発生する可能性があります。

`jrnl`を初めて実行すると、ジャーナルファイルをどこに作成するか、そして暗号化するかどうかを尋ねられます。

## クイックスタート

新しいエントリーを作成するには、以下のように入力します

```text
jrnl yesterday: 病欠した。時間を使って掃除をし、本の執筆に4時間費やした。
```

そしてリターンキーを押します。`yesterday:`はタイムスタンプとして解釈されます。
最初の文章の区切り（`.?!:`）までがタイトルとして、残りが本文として解釈されます。ジャーナルファイルでは、結果は次のようになります：

```output
2012-03-29 09:00 病欠した。
時間を使って家の掃除をし、本の執筆に4時間費やした。
```

単に`jrnl`と入力すると、エントリーの作成を促されますが、外部エディタを使用するように*jrnl*を[設定する](advanced.md)こともできます。
