# 8-1. 新規画面作成手順
ここでは、詳細設計書に従って新規画面を作成する手順について説明します。

## 詳細設計の内容確認
SEから指示された詳細設計書を読み、疑問点の洗い出しを行います。
この時点での疑問点は、**開発着手前**に必ず設計担当者に確認してください。

## 画面初期表示処理の実装
### 掟
- いきなりすべての機能を実装しようとしてはならない
- まずは、URLアクセス -> 画面初期表示部の処理を作成する
  - コンソールアプリで最初にざっくりmainメソッドを作る作業に相当。

### 手順
#### 1. ビュー雛形のコピー
最初にビューを作りましょう。
misc/skeleton フォルダに新規画面作成用の雛形が用意されています。この中から担当する画面向けの雛形を選び、詳細設計書に記載されている場所・ファイル名でコピーしてください。

| 雛形ファイル名 | 説明 |
| -- | -- |
| unit.html | 単票形式の画面用雛形 |
| (作成予定) | 一覧形式の画面用雛形 |
| (作成予定) | ヘッダ・ディテール形式の画面用雛形 |

#### 2. 最小限のコントローラ作成
続いてコントローラを作成します。ただし、この時点で作るのは必要最小限です。
つまり、「特定のURLにアクセスすれば、先ほど用意したビューが表示される」処理のみを作ります。

まず、詳細設計書の指示に従い、コントローラクラスを作成します。

##### 実装例
```java
package com.akikura.web.order;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping("/orders")
public class OrderEntryController {
	@RequestMapping(path="/entry", method=RequestMethod.GET)
	public String entry() {
		return "order/order-entry";
	}
}
```
##### 規約
- 画面用のコントローラクラスには、@Controller アノテーションを付与する
- 画面用のコントローラクラスには、@RequestMapping アノテーションを付与する
  - pathには、サブシステムの親パスを指定する（例：荷主登録画面 /owners/entry -> 荷主サブシステムを表す /owners）
- @RequestMapping メソッドは以下の規約を守ること
  - 戻り値はString（表示画面を表すパス）
  - 引数は以下を指定可能（他にも指定可能だが、原則以下のみを使用する。）
    - 画面入力/表示項目用フォームオブジェクト
    - Validationありのリクエストの場合、BindingResult
    - リダイレクトありで、リダイレクト先に情報転送が必要な場合、RedirectAttributes

この時点で、akikuraを起動し、@RequestMappingメソッドに指定したURLにアクセスして、コピーしたビュー雛形の画面が表示されることを確認します。

#### 3. デザインイメージの適用
src/main/resources/static/sample の中にデザイン案が置いてあります。
例えば、オーダー登録画面なら、order-entry01.html です。
設計担当者に適用するデザイン案を確認して、1.で配置したビューに内容をコピーしてください。

#### 規約
- コピーするのは、articleタグの内容のみ。他の箇所はビュー用共通モジュール（templates.common)で定義済みのため。コンテンツ[START]/[END]のコメントの間を書き換える。
```html
<!--/* コンテンツ [START] */-->
<article class="container">
  <form class="form-horizontal" method="POST" id="submitForm"> 
    <h2 class="PageTitle">(ページタイトル)</h2>
    <fieldset>
    </fieldset>
  </form>
</article>
<!--/* コンテンツ [END] */-->
```
#### 4. 画面実装
以下の画面実装を行う
-
