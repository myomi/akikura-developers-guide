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

### 1. バインドなし
まず、バインドなしのケースを
