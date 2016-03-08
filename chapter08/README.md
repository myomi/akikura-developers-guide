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
| DIコンテナ・AOP・MVC | Spring Framework4 | http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/ |
| DBアクセス | Spring Data JPA | http://docs.spring.io/spring-data/jpa/docs/current/reference/html/ |
| HTMLテンプレート | Thymeleaf2.1 | http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf_ja.html |
| | (ThymeleafのSpring向け機能拡張) | http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html |

この中でも特に重要なフレームワークがSpring Frameworkです。PureなJavaでWebアプリケーションを作る（前章のサーブレット）ことの困難さを解決するためにSpringを導入しています。

実際の開発案件では、Pure Javaの上にフレームワークを１枚噛ませて開発することが通常であり、2016年時点では以下の３択でしょう。

- Spring Framework
- Java EE(6 or 7)
- その会社独自のフレームワーク

さて、インターンの学習目標は「Spring Frameworkをマスターすること」ではありません（もちろんマスターしても構いませんが）。今回の学習目標は以下の３つです。

1. Webアプリケーション用のFrameworkを導入することで、HTTPリクエスト->レスポンスまでの処理の流れが、コントローラ・サービス・データアクセス・ビューの４レイヤーに分割されることを学ぶ。また、それぞれのレイヤの役割を学ぶ
2. 納期を意識する。すなわち、初見のフレームワークに対して効率的に学び、かつ成果を出す手法を学ぶ。
3. 初見のフレームワークに対する恐怖心をなくす。就職先・アサイン先の案件がSpring Frameworkを使うとは限らない。そもそもJavaですらないかもしれない。そんな時でも、臆さずチャレンジできる、ガッキーのような~~面の皮の厚さ~~メンタルの強さを身につける。

## 開発時の注意
- ブログやQAサイト等での調査は構いませんが、質問・報告時は必ず出典(URL等）を明記すること
- 納期を意識する、フレームワーク・ライブラリを自主的に学ぶことは構わないし、推奨するが、自己調査では手にあまると感じたら、遠慮なく質問する。
- 荷主登録画面が機能的によく似ているので、ソースコードを参考にすること。ソースコードの場所が分からなければ、質問すること。