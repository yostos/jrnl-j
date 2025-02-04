<!--
Copyright © 2012-2023 jrnl contributors
License: https://www.gnu.org/licenses/gpl-3.0.html
-->

# 基本的な使い方

`jrnl`には2つのモードがあります：**作成モード**と**閲覧モード**です。ダッシュ（`-`）
やダブルダッシュ（`--`）で始まる引数を入力しない場合は作成モードになり、
コマンドラインでエントリーを書くことができます。

私たちは意図的にコマンドライン引数の慣例を破っています：_一重のダッシュ_（`-`）で
始まるすべての引数は、閲覧前にジャーナルを*フィルタリング*します。フィルタ引数は
任意に組み合わせることができます。_二重のダッシュ_（`--`）で始まる引数は、
ジャーナルの表示やエクスポートの方法を*制御*します。制御引数は相互に排他的です
（つまり、一度に一つの方法でのみジャーナルの表示やエクスポートを指定できます）。

コマンドのリストを表示するには、`jrnl --help`と入力してください。

## エントリーの作成

作成モードは、引数なしで`jrnl`を起動する（外部エディタが起動します）か、
コマンドラインに直接エントリーを書くことで入ります：

```text
jrnl today at 3am: バーでスティーブ・ブシェミに会った！とても良い人だった。
```

!!! note
ほとんどのシェルには`#`や`*`などの予約文字があります。これらの文字や、
バランスの取れていない単一または二重引用符、括弧などは、おそらく問題を
引き起こします。予約文字は`\`を使ってエスケープできますが、長文の
書き込みには理想的ではありません。解決策：まず`jrnl`と入力してリターンキーを
押します。その後、ジャーナルエントリーのテキストを入力できます。
または、[外部エディタを使用する](./advanced.md)こともできます。

ファイルから直接エントリーをインポートすることもできます：

```sh
jrnl < my_entry.txt
```

### 日付と時間の指定

日付と時間を指定しない場合（例：`jrnl 弟への手紙を書き終えた`）、`jrnl`は現在の日付と時間を使用してエントリーを作成します。過去のエントリーの場合、タイムスタンプを使用して`jrnl`にエントリーの配置場所を指示できます。タイムスタンプは様々な形式で入力できます。以下は機能する例です：

- at 6am
- yesterday
- last monday
- sunday at noon
- 2 march 2012
- 7 apr
- 5/20/1998 at 23:42
- 2020-05-22T15:55-04:00

タイムスタンプを使用しない場合、`jrnl`は現在の時刻を使用してエントリーを作成します。
日付のみを使用する場合（時刻なし）、`jrnl`は[設定ファイル](./reference-config-file.md#default_hour-and-default_minute)で
指定されたデフォルトの時刻を使用します。
内部的には、`jrnl`はエントリーを年代順に並べ替えます。

### タグの使用

`jrnl`はタグをサポートしています。デフォルトのタグシンボルは`@`です（主に`#`が
予約文字であるため）。[設定ファイル](./reference-config-file.md#tagsymbols)で
独自のタグシンボルを指定できます。タグを使用するには、目的のタグの前にシンボルを
付けます：

```sh
jrnl @ビーチで@トムと@アンナと素晴らしい一日を過ごした。
```

エントリーにタグを付ける際に大文字を使用できますが、タグによる検索は
大文字小文字を区別しません。

1つのエントリーで使用できるタグの数に制限はありません。

### エントリーにスターを付ける

エントリーをお気に入りとしてマークするには、単にアスタリスク（`*`）を使って
「スター」を付けます：

```sh
jrnl last sunday *: 人生最高の日。
```

日付を追加したくない場合（つまり、日付を*now*として入力したい場合）、
以下のオプションは同等です：

- `jrnl *: 人生最高の日。`
- `jrnl *人生最高の日。`
- `jrnl 人生最高の日。*`

!!! note
アスタリスク（`*`）の前後に空白がないことを確認してください。
`jrnl 人生最高の日！ *`は機能しません。なぜなら、`*`文字はほとんどの
シェルで特別な意味を持つからです。

## エントリーの閲覧と検索

`jrnl`は様々な方法でエントリーを表示できます。

すべてのエントリーを表示するには、次のように入力します：

```sh
jrnl -to today
```

`jrnl`はいくつかのフィルタリングコマンドを提供しており、一重のダッシュ（`-`）で
始まり、より具体的な範囲のエントリーを見つけることができます。例えば、

```sh
jrnl -n 10
```

は最新の10件のエントリーを表示します。`jrnl -10`はさらに簡潔で、同じように機能します。
昨年の初めから今年の3月末までに書いたすべてのエントリーを見たい場合は、
次のように入力します：

```sh
jrnl -from "last year" -to march
```

複数の単語を使用するフィルタ条件は、引用符（`""`）で囲む必要があります。

特定の日のエントリーを見るには、`-on`を使用します：

```sh
jrnl -on yesterday
```

### テキスト検索

`-contains`コマンドは、その後に入力したテキストを含むすべてのエントリーを表示します。
これは、エントリーを検索する際に、書いた時にタグを付けたかどうか覚えていない場合に
役立つかもしれません。

ある単語をよく使っていることに気づき、過去のすべてのエントリーでそれをタグに
変えたいと思うかもしれません。

```sh
jrnl -contains "犬" --edit
```

外部エディタを開き、"犬"という単語のすべてのインスタンスにタグシンボル
（デフォルトでは`@`）を追加できます。

### タグによるフィルタリング

ジャーナルエントリーをタグでフィルタリングできます。例えば、

```sh
jrnl @ピンキー @世界征服
```

は`@ピンキー`または`@世界征服`のいずれかが出現するすべてのエントリーを表示します。
タグフィルタは他のフィルタと組み合わせることができます：

```sh
jrnl -n 5 @ピンキー -and @世界征服
```

は`@ピンキー`_と_`@世界征服`の*両方*を含む最新の5つのエントリーを表示します。
[設定ファイル](./reference-config-file.md#tagsymbols)でタグに使用したいシンボルを
変更できます。

!!! note
`jrnl @ピンキー @世界征服`と入力すると、両方のタグが存在するエントリーが
表示されます。これは、コマンドライン引数が与えられていないにもかかわらず、
すべての入力文字列がタグのように見えるためです。`jrnl`は、タグのみで
構成される新しいエントリーを作成するのではなく、タグでフィルタリング
したいと想定します。

ジャーナル内のすべてのタグのリストを表示するには、次のように入力します：

```sh
jrnl --tags
```

### スター付きエントリーの表示

お気に入り（スター付き）のエントリーのみを表示するには、次のように入力します：

```sh
jrnl -starred
```

## エントリーの編集

エントリーを書いた後で編集することができます。これは特に、ジャーナルファイルが
暗号化されている場合に便利です。この機能を使用するには、[設定ファイル](./reference-config-file.md#editor)で
外部エディタを設定する必要があります。また、特定の検索条件に一致するエントリー
のみを編集することもできます。例えば、

```sh
jrnl -to 1950 @テキサス -and @歴史 --edit
```

は外部エディタを開き、1950年以前に書かれた`@テキサス`と`@歴史`のタグが付いた
すべてのエントリーを表示します。変更を加えてファイルを保存して閉じると、
それらのエントリーのみが変更されます（該当する場合は暗号化されます）。

複数のジャーナルを使用している場合、特定のジャーナルから特定のエントリーを
簡単に編集できます。フィルタ文字列の前にジャーナルの名前を付けるだけです。
例えば、

```sh
jrnl work -n 1 --edit
```

は'work'ジャーナルの最新のエントリーを外部エディタで開きます。

## エントリーの削除

`--delete`コマンドは、エントリーを削除するための対話型インターフェースを開きます。
ジャーナル内の各エントリーの日付とタイトルが一つずつ表示され、各エントリーを
保持するか削除するかを選択できます。

フィルタが指定されていない場合、`jrnl`はジャーナル全体の各エントリーを一つずつ
保持するか削除するかを尋ねます。ジャーナルに多くのエントリーがある場合は、
`--delete`コマンドを渡す前にエントリーをフィルタリングする方が効率的かもしれません。

例を挙げます。過去12年間のブログ投稿をインポートしたジャーナルがあるとします。
`@本`タグを頻繁に使用しており、何らかの理由で、そのタグを使用したエントリーの
一部（全部ではない）を削除したいのですが、2004年以前に書いたものだけです。
どのエントリーを保持したいかわからず、決定する前に確認したいとします。
次のように入力するかもしれません：

```sh
jrnl -to 2004 @本 --delete
```

`jrnl`は関連するエントリーのみを表示し、削除したいものを選択できます。

2004年以前に書いた`@本`を含むすべてのエントリーを削除したい場合もあるでしょう。
数十または数百ある場合、最も簡単な方法は外部エディタを使用することです。
削除したいエントリーをエディタで開きます...

```sh
jrnl -to 2004 @本 --edit
```

...すべてを選択し、削除して保存して閉じると、それらのエントリーすべてが
ジャーナルから削除されます。

## ジャーナルのリスト表示

すべてのジャーナルをリスト表示するには：

```sh
jrnl --list
```

表示されるジャーナルは、`jrnl`の[設定ファイル](./reference-config-file.md#journals)で
指定されたものに対応します。
