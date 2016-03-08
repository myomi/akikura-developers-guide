# 8-2. Spring MVC概説

ここでは、Spring Frameworkに含まれている、「Spring MVC」と言う機能について簡単に解説します。

まずは、[6-4. サーブレット](chapter06/0605servlet.md)で出てきた、サーブレットを使ったWebアプリの構成図をもう一度見てください。

![](../images/image-06-0003.png)

サーブレットの特徴をおさらいすると、以下の通りです。

- 特定のURLパスに対するHTTPリクエストが、対応するサーブレットに送られる(例： /app1 -> App1Servlet）
- サーブレット内の処理は、リクエストされたHTTPメソッドの種類に応じて、service, doPost, doGetなどに振り分けられる
- 専用のOutputStreamにHTML文字列を出力することにより、HTTPレスポンスを生成できる。

さて、この特徴を踏まえて、サーブレットのみでWebアプリケーションを作成することに対してリスクがないか、考えてみてください。サーブレットのみで、大規模なWebアプリケーションを高速かつ堅牢に作ることはできるでしょうか？

## サーブレットでWebアプリケーションを作ることのリスク

### サブパスへの対応
１つのWebアプリケーションで利用可能なURLのパスは１つではないはずです。例えば、akikuraアプリケーションのベースパスが/akikuraだったとして、以下のような設計が行われたとします。

| HTTPメソッド | URLパス | 動作仕様 |
| -- | -- | -- |
| GET | /akikura/orders | オーダー一覧画面を表示する |
| GET | /akikura/orders/{id} | 指定されたIDのオーダー詳細画面を表示する |
| GET | /akikura/orders/entry | オーダー登録画面を表示する |
| POST | /akikura/orders | オーダー登録を実行する |

皆さんはどう実装しますか？

- あなたはまず、/akikura に対応するサーブレット、AkikuraServletを作ります。
- AkikuraServletのdoPost, doGetメソッドの中で、リクエストされたパスをチェックし、処理を振り分けます。こんな感じで。。。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
  throws ServletException, IOException {
  
	String path = request.getRequestURI();
	if ("/akikura/orders".equals(path)) {
        // オーダー一覧
	} else if ("/akikura/orders/entry".equals(path)) {
		// オーダー登録
	} // ...地獄！
}
```

。。。これは嫌な予感がします。大規模なWebアプリケーションになると、大量のURLパスに対応する必要があるため、あっという間にコードが肥大化しそうです。

また、一つのメソッドの中に複数の画面処理が混在するため、ある特定の画面の修正を行ったつもりが、別の画面機能を意図せずに壊してしまった、、！というリスクもあります。

- **(リスク１）サーブレットメソッドの肥大化**
- **(リスク２）機能改修時のリグレッション（デグレード）**

