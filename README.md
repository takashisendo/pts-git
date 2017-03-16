<!-- $theme: gaia -->

# チーム開発を変える<br>Gitの徹底活用法

#### 3/16 Pasona Tech セミナー

###### Sprocket CTO: 中田 稔

---

## Git とは？

---

==分散型バージョン管理システムの決定版！==

- テキストで管理できるものであればなんでも
  - Code、HTML、CSS、README、 etc.
- 差分情報を記録する
  - diff ベース
- 画像等のバイナリデータも扱える
  - あまり得意ではない
  - 差分が取れないため、まるっと保存

---

## diff とは？

---

2つのファイルを比較し、差分を抽出するツール

```diff
--- a/src/app.js
+++ b/src/app.js
@@ -19,7 +19,12 @@ const client = new Wit({

     getForecast: req => {
       console.log('getForecast:', JSON.stringify(req, null, '  '));
-      req.context.forecast = 'sunny';
+      if (!req.entities.location) {
+        req.context.missingLocation = true;
+      } else {
+        req.context.forecast = 'sunny';
+        req.context.location = req.entities.location[0].value;
+      }
       return Promise.resolve(req.context);
     }
   }
```
