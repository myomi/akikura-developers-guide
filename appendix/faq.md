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
| /orders | POST | オーダー登録実行 |


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

### 3. ビューにth:object属性を付与

