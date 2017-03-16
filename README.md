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

---

## ローカルとリモート

分散管理なので、少々概念がややこしい
![](images/fetch_pull_push.png)

---

## チーム開発における<br>Git活用のポイント

---

## Git活用のポイント1

ブランチを多用する

- 何か作業をするときは基本的にブランチを作る
  - 違う機能やバグ修正は別ブランチ
  - ブランチ毎に==作業の独立性==を保つ
    - 他人の修正の影響を極力避ける
- 修正が終わったらプルリクエスト
  - マージされたらブランチ削除
  - ==作業場感覚==で作っては捨てる

---

## Git活用のポイント2

目的が明確なコミットを作る

- 1つの機能追加・バグ修正を==1つのコミット==にする
  - 中途半端・不完全なコミットを作らない
  - 「作業途中」「fix typo」 とかはダメ
  - レビューする人が苦痛
- 大きな機能追加の場合はサブ機能毎にコミットを作ることはある
  - 大量の修正をレビューするのは大変
  - コミット単位でレビューできるように

---

## Git活用のポイント3

コミットログだけで==修正内容がわかる==ようにする

- レビューしたり後から調査する際に重要
  - コードの中身を毎回見ていられない
  - 「バグ修正」「機能追加」のようなものはダメ
  - 「○○の□□を××した」のように明確にする
- ポイント2ができていれば自ずと明確になるはず

---

## Git活用のポイントまとめ

1. ブランチを多用する
2. 目的が明確なコミットを作る
3. コミットログだけで修正内容がわかるようにする

初めからそんなキレイなコミットは作れない...
⇒ 後から整理すればOK
⇒ ==履歴の書き換えが必須==

Gitは履歴を書き換えてなんぼの世界！

---

## 使い勝手を向上させる設定

---

## alias の作成

`git log` はそのままでは使いにくいので、graph 表示が可能なように alias を作成する

```bash
% git config --local alias.graph \
  "log --graph --oneline --decorate=short"
```

これで `git graph` が使えるようになる
※ 実際はもっと良い設定あり

```bash
% git graph

* fe78873 (HEAD -> master, origin/master) initial commit
```

※ 今回は `--local` ですが、`--global` が便利

---

他にも、SVN ユーザであれば

```bach
ci => commit
co => checkout
```

等の設定をしておくと便利。

`git ci`、`git co` のように利用できる。

---

## ブランチ操作をマスターする

---

## branch

ブランチの一覧、作成、削除等の操作を行う

```bash
% git branch --help
```

---
<!-- *template: invert -->

## branch 操作の実践

ブランチ一覧を見る（ローカルのみ）

```bash
% git branch

* master
```

ブランチ一覧を見る（リモート込み）

```bash
% git branch -a

* master
  remotes/origin/master
```

---
<!-- *template: invert -->

現在位置でブランチを作成する

```bash
% git branch test
% git branch

* master
  more_docs
  test

% git graph

* 8ee37c5 (HEAD -> master, origin/master, test) 使い勝手を向上させる設定を追加
* de78949 Git活用のポイントを追加
* bd1ba42 ローカルとリモートの図を追加
* bd7a8a9 バージョン管理の歴史を追加
* 8a57c36 git と diff のスライドを追加
* 336878d initial commit
```

---
<!-- *template: invert -->

ブランチをリネームする

```bash
% git branch -m test example
% git branch

  example
* master
  more_docs
```

ブランチを削除する

```bash
% git branch -d example

Deleted branch example (was 8ee37c5).

% git branch

* master
  more_docs
```

---

## checkout

ブランチを切り替えたり作業ツリーを復元する

```bash
% git checkout --help
```

---
<!-- *template: invert -->

## checkout の実践

作業ツリーを指定ブランチに切り替える

```bash
% git checkout more_docs

Switched to branch 'more_docs'
Your branch is up-to-date with 'origin/more_docs'.

% git branch

  master
* more_docs
  test
```

---
<!-- *template: invert -->

新しいブランチを作成して切り替える

```bash
% git checkout master -b test

Switched to a new branch 'test'

% git branch

  master
  more_docs
* test
```

ブランチ名(master) は省略可能、省略した場合は現在位置(HEAD)が使われる

---

## merge

別のブランチの内容を現在のブランチに取り込む

```bash
% git merge --help
```

---
<!-- *template: invert -->

## merge の実践

別のブランチの内容を現在のブランチに取り込む

```bash
% git checkout master
% git merge more_docs

Merge made by the 'recursive' strategy.
 another_doc.md | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 another_doc.md
```

※ ブランチが分岐していない場合は FastForward になる

---
<!-- *template: invert -->

merge の確認

```bash
% git graph

*   fcc71ea (HEAD -> master) Merge branch 'more_docs'
|\
| * a973eb9 (origin/more_docs, more_docs) another_doc.md を追加
* | 4967a5c checkout を追加
* | 4116edf branch を追加
|/
* 8ee37c5 (origin/master) 使い勝手を向上させる設定を追加
* de78949 Git活用のポイントを追加
* bd1ba42 ローカルとリモートの図を追加
* bd7a8a9 バージョン管理の歴史を追加
* 8a57c36 git と diff のスライドを追加
* 336878d initial commit
```
