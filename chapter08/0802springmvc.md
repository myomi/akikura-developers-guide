# 8-2. Spring MVC概説

ここでは、Spring Frameworkに含まれている、「Spring MVC」と言う機能について簡単に解説します。

まずは、[6-4. サーブレット](chapter06/0605servlet.md)で出てきた、サーブレットを使ったWebアプリの構成図をもう一度見てください。

![](../images/image-06-0003.png)

サーブレットの特徴をおさらいすると、

- 特定のURLパスに対するHTTPリクエストが、対応するサーブレットに送られる(例： /app1 -> App1Servlet）
- 