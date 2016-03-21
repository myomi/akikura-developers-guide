# Appendix.A よくある質問

## コントローラとViewのバインドの仕組み

### 0. 前提
ModelAttribute クラスが以下のように定義されていたとします。

#### モデルアトリビュート(OrderForm)
```java
public class OrderForm {
  // getter, setter は省略
  private String id;
}
```

また、以下のHTTPリクエストに対応する処理を実装中とします。

| Path | Method | 処理 |
| -- | -- | -- |
| /orders/entry | GET | オーダー登録画面の初期表示 |


### 1. バインドなし
まず、バインドなしのケースを考えます。

#### コントローラ
```java
@Controller
@RequestMapping("/orders")
public class OrderEntryController {
	@RequestMapping(path="/entry", method=RequestMethod.GET)
	public String entry() {
		return "order/order-entry";
	}
}
```

#### ビュー（抜粋）
```html
<form method="post">
  <input type="text" name="id" id="id"/>
</form>
```
この場合、コントローラ・ビューともに「モデルアトリビュートのことを知らない」ので、エラーなく実行することができます。

### 2. リクエストスコープ
Spring FrameworkとThymeleafは、ユーザからHTTPリクエストが送られてくると、お互い共有のオブジェクト置き場（特定のメモリ領域）を参照します。
オブジェクト置き場には幾つか種類があるのですが、一番よく利用する（そして、akikuraの大半のケースで利用する）置き場のことを **request scope（リクエストスコープ）**と言います。

Spring Framework で@ModelAttribute アノテーションが付与されたメソッドや引数は、リクエストの都度初期化され、リクエストスコープに保存されます。

![](../images/appendix-0001.png)

### 3. ビューにth:object属性を付与

ビューにth:object属性を付与して、モデルアトリビュートをバインドします。

```html
<form method="post" th:object="${orderForm}">
  <input type="text" th:field="*{id}"/>
</form>
```

この定義によりビューは、「リクエストスコープ上に orderForm という名前でオブジェクトが存在する」前提で動作をします。一方、Springは依然としてモデルアトリビュートの存在を知らないため、初期化をしません。

従って、リクエストスコープ上に誰もorderFormを作成することなく、Thymeleafが参照に行くため、エラーとなります。
