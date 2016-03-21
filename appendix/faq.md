# Appendix.A よくある質問

## コントローラとViewのバインドの仕組み

### 0. 前提
ModelAttribute クラスが以下のように定義されていたとします。

```java
public class OrderForm {
  // getter, setter は省略
  private Long id;
}
```

また、以下のHTTPリクエストに対応する処理を実装中とします。

| Path | Method | 処理 |
| -- | -- | -- |
| /orders/entry | GET | オーダー登録画面の初期表示 |
| /orders | POST | オーダー登録実行 |


### 1. バインドなし
まず、バインドなしのケースを
