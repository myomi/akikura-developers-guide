# 8.実践 商蔵

あなたはakikuraプロジェクトの一員として、とある開発チームに配属されました。
ここからは実際に業務アプリケーションを作成していきましょう。

## 作成する画面
以下の２画面を担当してください。
- オーダー登録画面
- オーダー詳細画面

## 開発期間
開発着手から10日間。
開発後に結合テストフェーズを別途2日間もうけます（テスト仕様書は準備します）

## 設計書
ファイルサーバの以下の場所に詳細設計書があります。
開発着手する前に必ず目を通し、疑問点があれば設計担当者に問い合わせてください。

## 技術情報
akikuraプロジェクトでは以下のフレームワーク・ライブラリを利用します。
ドキュメントのURLを併記しますので、必要に応じて参照してください。

### フロントエンド
| 分類 | フレームワーク・ライブラリ | マニュアル |
| -- | -- | -- |
| CSSフレームワーク | Bootstrap3 | http://getbootstrap.com/ |
| JSライブラリ | jQuery2 | http://api.jquery.com/ |

### バックエンド
| 分類 | フレームワーク・ライブラリ | マニュアル |
| -- | -- | -- |
| 各フレームワークの統合 | Spring Boot1.3 | http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/ |
| DIコンテナ・AOP | Spring Framework4 | http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/ |
| DBアクセス | Spring Data JPA | http://docs.spring.io/spring-data/jpa/docs/current/reference/html/ |
| HTMLテンプレート | Thymeleaf2.1 | http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf_ja.html |
| | (ThymeleafのSpring向け機能拡張) | http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html |

## 開発時の注意
- ブログやQAサイト等での調査は構いませんが、質問・報告時は必ず出典(URL等）を明記すること
- 納期を意識する、自己調査では手にあまると感じたら、遠慮なく質問する
- 荷主登録画面が機能的によく似ているので、参考にすること。