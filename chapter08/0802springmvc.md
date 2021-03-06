# 8-2. Spring MVC概# 8-2. Spring MVC概説

ここでは、Spring Frameworkに含まれている、「Spring MVC」と言う機能について簡単に解説します。

まずは、[6-4. サーブレット](../chapter06/0605servlet.md)で出てきた、サーブレットを使ったWebアプリの構成図をもう一度見てください。

![](../images/image-06-0003.png)

サーブレットの特徴をおさらいすると、以下の通りです。

- 特定のURLパスに対するHTTPリクエストが、対応するサーブレットに送られる(例： /app1 -> App1Servlet）
- サーブレット内の処理は、リクエストされたHTTPメソッドの種類に応じて、service, doPost, doGetなどに振り分けられる
- 専用のOutputStreamにHTML文字列を出力することにより、HTTPレスポンスを生成できる。

さて、この特徴を踏まえて、サーブレットのみでWebアプリケーションを作成することに対してリスクがないか、考えてみてください。果たしてサーブレットのみで、大規模なWebアプリケーションを高速かつ堅牢に作ることはできるでしょうか？

## サーブレット飲みでWebアプリケーションを作ることのリスク

### サブパスへの対応
１つのWebアプリケーションで利用可能なURLのパスは１つではないはずです。例えば、akikuraアプリのベースパスが/akikuraで、以下のような設計をしたと仮定します。

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

### HTMLの出力
先ほどのif文の中に、HTMLを生成する処理を書くことを考えると、さらにコードが肥大化しそうです。

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
  throws ServletException, IOException {
  
	String path = request.getRequestURI();
	if ("/akikura/orders".equals(path)) {
        // オーダー一覧
        PrintWriter out = response.getWriter();
		out.println("<html>");
        // 以下、延々と続く。。。
	} else if ("/akikura/orders/entry".equals(path)) {
		// オーダー登録
        // ここは別のHTML生成処理
	} // ...地獄！
}
```

また、これまで学んできたように、JavaとHTMLは全く違う言語です。Javaのコンテキストの中で、HTMLコードを**Java文字列で記述する**など、入力ミスする予感しかしません。Java文字列内の記述は、IDEのコードアシストが効きませんからね。。。

- **（リスク３）複数の言語がごちゃまぜでカオス！**

### DBアクセス処理 
さらに、サーブレットの中にDBアクセスの処理を追加して、、、となると、これはサーブレットメソッドが一体何行になるのか、見当もつきません。

適切なモジュール分割が必要です。

- **（リスク４）適切な粒度で、プロジェクトメンバ全員が共有出来るモジュール分割のルールがない**

### 全てが文字列
[7.研修 サーブレット](../chapter07/README.md)で作成したプログラムで、フォームに入力された値を取り出す処理をもう一度見てみましょう。

```java
String name = request.getParameter("name");
String sex = request.getParameter("sex");
```

このように、HttpServletRequest#getParameter(key) で入力値を取り出すわけですが、取り出した値は全て文字列です。できれば適切な型を使ってプログラミングしたいですよね。数値なら、Integer, Long, BigDecimal、日付ならLocalDateなど。。

そこで、以下のようなプログラミングになるわけです。

```java
// 年齢
String strAge = request.getParameter("age");
try {
	int age = Integer.parseInt(strAge);
} catch (NumberFormatException e) {
	// 数値以外が入力された時。。。
}
```

これは画面項目が増えるに従い、コード量が爆発的に増加しそうです。

- **（リスク５）画面項目の入出力が全て文字列でのやり取りとなる。そのため、業務処理（ビジネスロジック）実行のために明示的にキャストする必要がある**

## Spring MVC導入のメリット
以上のリスクを改善する手段として、Webアプリケーションフレームワークを導入します。タイトルには、「Spring MVC導入のメリット」と書きましたが、これはSpring MVCに限らず、他のWebフレームワークを導入する意図同じです。それぞれ細かな記法や設計方針の違いがありますが、どれも前述のリスクを改善するための手段として提供されるものと認識しましょう。

それでは、Spring MVCの導入により前述のリスクがどのように改善されるのかを見ていきましょう。



### Spring MVCのモジュール構成
まず、Spring Frameworkのリファレンス[21.2 The DispatcherServlet](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-servlet)の図21.1を引用します。これが、Spring Frameworkの構成だっ！！

![](../images/mvc.png)

...これだけだと、なんのこっちゃだと思うので、今まで説明で使ってきた~~しょぼい~~構成図にSpring MVCの登場人物を追記してみます。図が小さかったら、拡大してみてね。

![](../images/image-08-0001.png)

この図をもとに一つづつ機能を解説します。先ほど挙げたリスクが、どのように解決されているかを注目しながら聞いてください。

#### DispatcherServlet
Spring MVCには、デフォルトでDispatcherServletというサーブレットクラスが用意されています。そのため開発者はサーブレットを作成する必要がありません。

#### コントローラ（MVCの「C」）
DispatcherServletは、[7.研修 サーブレット](../chapter07/README.md)で作成したものと同様に、特定のパスに対するリクエストを一手に引き受けます。
図の例だと、/akikuraですね。

が、DispatcherServletは受けたリクエストを自分では処理せず、「コントローラー」というクラスに処理を丸投げします。

DispatcherServletはどのようにして、コントローラの存在を認識するかというと、@Controller アノテーションをチェックしています。
akikuraの起動時に、@Controller アノテーション付きのクラスを検索して、仕事丸投げできるマンを探しているのです。

加えて、@RequestMappingアノテーションで、自分が担当するURLパスやHTTPメソッドを宣言しています。
```java
@RequestMapping(path="/owners", method=RequestMethod.GET)
public String xxx() {
}
```
のように書くと、「自分は/owners へのGETリクエストしか処理しませんよ」という宣言になります。
すべてのリクエストを一つのコントローラが処理してしまうと、それはもう、ただのサーブレットと同じですもんね。役割分担できるのが、良いところ。

この特性によって、以下のリスクが改善されます。

- **(リスク１）サーブレットメソッドの肥大化**
  - サーブレット内でif文を書くことなく、各リクエストをコントローラに振り分けすることができます。
- **(リスク２）機能改修時のリグレッション（デグレード）**
  - 各リクエストごとの処理がクラスに分割されるので、意図せず他の機能を壊してしまうリスクが激減します。

さて、このコントローラですが実際に行う作業はたった３つです。
1. 自分が担当するHTTPリクエストを受け、メソッドを実行する。
2. HTTPレスポンス（通常、次に表示するHTML)に必要なデータを準備する。
3. 次に表示するHTML文字列を生成するよう、HTML生成担当係（後述）に処理を依頼する。

業務アプリケーションは、リクエストを受けて、単にレスポンスを返すだけのプログラムではありません。
- リクエストの情報を受けて、業務上必要な計算処理をする
- DBにデータを登録する
- DBからデータを読む

などなど、いろいろな仕事があるわけです。

でも、コントローラはこれらの仕事をしません。
どうするかというと、これらの仕事を全て、サービスクラスに丸投げするのです。なんという、多重下請け構造。。。！

#### サービス
サービスは業務処理の諸々を行うクラスです。
コントローラがやらない、前述の業務処理を全て担当します。
このことから、別名「ビジネスロジッククラス」とも呼ばれます。

別にこんなクラスなくても、コントローラが全てやればいいのに、と思うかもしれません。

実際、小規模なアプリではそういう設計もありえます。

前述のリスクの観点で言うと、サービスクラスは以下を解決する設計と言えるでしょう。

- **(リスク１）サーブレットメソッドの肥大化**
  - Spring MVCの観点で言うと、コントローラメソッドの肥大化を防ぐことができる、と言えます。コントローラには、HTTPリクエストとレスポンスだけを担当してもらい、ロジックやDBアクセスが絡む処理はサービスが担当、という役割分担。
- **（リスク４）適切な粒度で、プロジェクトメンバ全員が共有出来るモジュール分割のルールがない**
  - 長いwebアプリケーション開発の歴史の中で、この設計が良さそうだという、経験知の成果なんですねー。

