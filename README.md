# Nim標準コーディング規約

## はじめに
このコーディング規約は、NimmerJPの参加者、賛同者が守るべきものとして、[NEP1](https://nim-.org/docs/nep1.html)を前提としたうえで、コードの一貫性を保つために、より詳細なコーディング規約を定めるものです。

この文書は、時代遅れになったり、Nimの言語仕様が変更されたりするたびに更新されます。

また、各プロジェクトごとのコーディング規約の規定は、このコーディング規約の規定よりも常に優先されます。

## コーディング規約を無視しうる理由
コーディング規約を過度に守ろうとすることは、あまり良くないことです。
無理に守ろうとしたがために、より分かりにくくなったり、コードが煩雑になったりすることは全く望ましくありません。

そこで、次のような場合には、超法規的にコーディング規約を無視しうる、という方針を定めます。

1. コーディング規約に従うとより読みにくくなる場合。コーディング規約は、コードを読みやすくするために定めるものですから、より読みにくいコードになってしまうことは本末転倒といえます。ですから、この場合は当然にコーディング規約を遵守しなくても構いません。
2. (多くの場合歴史的経緯で残っている)周囲のコード片と一貫性を保つため。この場合は、コントリビューターの多くの同意を得られるならば、コーディング規約に従うように変えるべきです。しかし、そうでない場合も多いでしょう。その場合は、プロジェクトの暗黙的な規約に従うのが賢い選択かもしれません。
3. 問題になっているコードが、コーディング規約の出てくるより前に書かれたもので、それに準拠させること以外にコードを変更する理由がないとき。コーディング規約に準拠していないこと以外の理由がないコードの変更ほど馬鹿げた作業コストはありません。人手が有り余っているプロジェクトなどそう多くはないものです。別の何かに労力を注いだ方が有益ではないでしょうか。
4. コーディング規約で推奨されている機能をサポートしていない古いバージョンのNimと互換性を保つ必要がある場合。これは、Nimの文法上の制約になります。ですから、コーディング規約を守ることができないのは当然のことでしょう。

ここに挙げた理由以外でも、守らない理由が出てくるかもしれません。
その場合は、その理由を自らの良心、コードが読みにくくなるリスクと天秤にかけてください。
多くの場合、これ以外に***真に***守るべきではない理由はありません。

## コードのレイアウト

### インデント
1レベルインデントするごとに2つのスペース(`\x20`)を使います。
括弧(`()`)やブラケット(`[]`)、波括弧(`{}`)で囲まれた部分を改行する場合は、各行の要素の先頭を手で揃えます。
始めの行以外をインデントするやり方(以降、突き出しインデントとします)の場合は、はじめの行が開き括弧(`(`、`[`、`{`)以外で終わってはならず、また、次の行以降は1レベルのインデントを入れます。加えて、閉じ括弧(`)`、`]`、`}`)の前では改行するべきではありません。
仮引数のリストにおいて突き出しインデントを採用する場合や、条件文を改行する場合は、1レベルさらにインデントを追加します。

```nim
# 正しい
let shortName = ("Lorem ipsum dolor sit amet",
                 "consectetur adipiscing elit",
                 1, 2, 3, 4, 5)


let muchMuchLongName = (
  "Lorem ipsum dolor sit amet",
  "consectetur adipiscing elit",
  1, 2, 3, 4, 5)


proc someProc(arg1: int,
              arg2: string,
              arg3: float) =
  discard
  # or do something


proc farMoreLongNameProc(
    arg1: int,
    arg2: string,
    arg3: float) =
  discard
  # or do something


# 正しくない
let badExample = ("Lorem ipsum dolor sit amet",
  "consectetur adipiscing elit",　# ✗ 先頭が揃っていません。
  1, 2, 3, 4, 5
)                                 # ✗ 閉じ括弧の前では改行してはいけません。


proc badParameterArrangeProcOne(arg1: int,
  arg2: string, # ✗ 先頭が揃っていません。
  arg3: float
) =             # ✗ 閉じ括弧の前では改行してはいけません。
  discard
  # or do something


proc badParameterArrangeProcTwo(
  arg1: int, # ✗ 追加で1レベルインデントする必要があります。
  arg2: string,
  arg3: float) =
  discard
  # or do something
```

### 1行の長さ
1行は最大99文字(100文字未満)とします。コメント行やドキュメント行は最大80文字までです。
ただし、この制限はプロジェクトのコントリビューターの合意の上で、最大140文字まで緩和しうるものとします。

### 空行
各宣言の間の空行は、それぞれ2行開けてください。
また、`type`内のそれぞれの型の宣言の間の空行は1行開けてください。
ただし、`type`における型の各フィールドの宣言の間に空行は挟みません。

1行の空行は、任意の文の間に入れることができます。文内の各式の間には入れるべきではありません。

```nim
type
  WordBool   = distinct int16

  CalType    = distinct int

  SomeObject = object
    field: int
    someOtherField: string


const SomeConstantValue: int = 42


proc SomeProc(arg1: string) =
  discard
  # or do something
```

### 行末の空白
行末に空白を残すことは推奨されません。可能な限り消すようにしてください。

### import
importは、可能な限り各要素が辞書順に並ぶように配置します。
また、それぞれのimportは通常行を分けるべきです。
ただし、次の場合は行を分けなくてもよいものとします。

* 複数の標準ライブラリのimport
* `from`～`import`、`from`～`except`による同一モジュール内の複数のproc、template、macroなどのimport

```nim
# よい例
# このように辞書順に並べることは絶対(must)ではありませんが、そうなるように努めるべきです。
import macros, os, sequtils, strformat, strutils
from someModule import someOtherProc, someProc
from someOtherModule except SomeObject, SomeType


# 悪い例
import SomeModule, SomeOtherModule # ✗ 非標準のライブラリを同一のimport文でimportしてはいけません。
```

## コメント
複数行のコメントに複数行文字列を代用することは禁止します。
単行コメントは`#`の後に1つの空白を入れてください。
複数行コメントは、`#[`と`]#`の行には空白以外の文字を含めないでください。
そして、コメント部分は1レベルインデントします。

```nim
# よい例
# 複数行コメントは
# 単行コメントを
# このように繰り返すか、
#[
  このように
  複数行コメント構文を
  利用してください。
]#

#悪い例              ✗ `#`の後に空白がありません。
#[これはコメントです ✗ `#[`と同じ行にコメントを書いてはいけません。
改行します           ✗ 1レベルインデントする必要があります。
]#
discard """
実はこれもコメントです
"""               # ✗ 複数行のコメントに複数行文字列を代用してはいけません。
```
