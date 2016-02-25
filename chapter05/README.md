# 5.研修 HTML基礎
この章ではHTMLについて学びましょう。

## 概要
- みんながブラウザで普段見ているwebサイトはHTMLという言語で書かれている
- Hyper Text Markup Lanuguage = すげえテキストを記述するための言語
- markupとは何か？これだ↓

（マークアップ前）
```text
第１章 森のなかまたち
その森にはたくさんの動物たちがいた。リス、きつね、フクロウ、そして熊。

```

（マークアップ後）
```html
<section>
  <h1>第１章 森のなかまたち</h1>
  <p>
  その森にはたくさんの動物たちがいた。リス、きつね、フクロウ、そして熊。
  </p>
</section>
```

- お分かりいただけただろうか？ただのテキストに「構造」と「意味」を明示するのがマークアップだ
- マークアップ後のテキストで以下のことが読み取れるようになった
    - これはある文章の中の「章」らしい
    - 章の「タイトル」は森のなかまたちらしい（h = ヘッダ）
    - タイトル以降は段落のようだ（p = パラグラフ）
- <p></p>などの括りを「タグ」または「要素(element)」という
    - <p> が開始タグ、</p>を終了タグという
    - 文章を開始タグと終了タグでくくることを俗に「マークアップする」という
    - HTMLはただの文章をマークアップするための言語
- ところで何が「ハイパー」テキストなんだろう
    - リンク。あるサイトから別のサイトにジャンプできる
    - 画像や動画・音声を埋め込める
- まとめるとHTMLとは、「ただのテキストををマークアップして構造や意味がわかりやすくハイパーなテキストにするための言語」

HTMLのタグ全てをインターン中に覚えるのは無理なので、代表的なものだけを取り上げます。

## HTML・CSS・JavaScript
HTMLについて学ぶと、もれなくCSSとJavaScriptがついてきます。ここではそれぞれの役割についてのみ覚えよう

- HTML ・・・文章に構造と意味を付与する。結果「ブラウザでどのように見えるか」については関与しない
- CSS ・・・マークアップされた文章が「どう見えるか」を定義する。例：h1タグは赤色

```css
/* 例 */
h1 {
  color: red;
}
```
- JavaScript・・・ブラウザ上で実行できるプログラム言語。WEBサイトに動きを与える。

## アプリケーションのためのHTML
- HTMLはテキストを構造化するための言語
- もともとアプリケーションの画面のためのものではない


## 適当に書いても動く
HTMLはプログラム言語ではなく、単にテキストを構造化するための記法にすぎない。多少間違えた書き方をしても、ブラウザがよしなに判断してそれなりに見える。
便利だが、逆に正しく書くのは難しい。

HTMLにも仕様書があり、仕様を満たすためのルールがある。
例：ulタグの中には、liタグしか書けない。

このルールをすべて暗記するのは困難。
そこで、「HTML Validator」のご紹介。

## 課題
- 次項より提示される問題（回答つき）を写経する。必ず**自分の手で書く**こと
- 前項で説明したHTML Validatorを利用してエラーがないことを確認する
- 「課題の実行方法」に記載された手順で動作確認する

### 課題の提出方法
作成したhtmlファイルはソースコードのsrc/main/resources/static/tutorial フォルダに「課題番号.html」というファイル名で保存し、コミットしてください。

### 禁止事項
- 回答をコピペして実行してはいけない。手で書くこと
- 回答を見ずに、考えてHTMLを書こうとしてはいけない。回答を写経して、「こんなものなのか」と思ってもらうだけでOK。

### 課題の実行方法
- STSを起動します。
- Springパースペクティブを開きます。
- akikura_xxx プロジェクトを右クリック->Run As->Spring Boot Appを選択
- Consoleビューに以下のような起動ログが出ることを確認する

```
2016-02-22 22:55:07.072  INFO 4058 --- [  restartedMain] com.akikura.AkikuraApplication           : 
Started AkikuraApplication in 22.499 seconds (JVM running for 23.275)

```

- Google Chromeを起動
- http://localhost:8080/tutorial/課題番号.html にアクセス

（例：課題番号1001なら、http://localhost:8080/tutorial/1001.html）