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

---

## バージョン管理の歴史1

- RCS
  - ファイル単体、ローカルでの差分管理
  - サーバ上の設定ファイルの履歴管理で今でも使ったりする

---

## バージョン管理の歴史2

- CVS
  - 中央管理サーバの登場
  - RCS を複数ファイルに対応したイメージ
    - ただし個別のファイルの集合体なので、それぞれのファイルにバージョンが付く

---

## バージョン管理の歴史3

- SVN
  - 複数ファイルの修正を纏めて履歴管理、バージョン番号をつけられるようになった
  - バージョン番号が連番で付与される

---

## バージョン管理の歴史4

- Git
  - 中央管理サーバの排除・分散化
    - 独立した複数のリポジトリが協調して作用
    - リモート、分散開発に有利
  - ローカルも1つの独立したリポジトリ
    - ネットがなくても使える！
