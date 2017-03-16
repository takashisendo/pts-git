<!-- $theme: gaia -->

## 基本操作のおさらい

#### リポジトリ操作関連

---

## init

カレントディレクトリで新規に git 管理を始める

```bash
git init
```

---
<!-- *template: invert -->

## init の実践

1. 任意のフォルダ（例: test）を作り、その中に移動
2. `git init` を実行
3. git 管理フォルダ `.git` の存在を確認
4. test フォルダを削除

---

## clone

リモートのリポジトリをローカルに複製（クローン）する

```bash
git clone git@github.com:u-minor/pts-git.git
```

---
<!-- *template: invert -->

## clone の実践

1. 作業用フォルダに移動
2. `git clone git@github.com:u-minor/pts-git.git` を実行
3. pts-git フォルダの存在を確認
4. pts-git フォルダに入り、`.git` 及び `README.md` があることを確認

---

## 基本操作のおさらい

#### ローカル履歴管理関連

---

## status

作業ツリーの状態を確認する

```bash
git status
```

現在のブランチ名や、未追加のファイルなどが確認できる

---
<!-- *template: invert -->

## status の実践

1. `git status` を実行
2. ステータスの表示を確認

---

## add

コミット予定の内容をインデックスに追加する

```bash
git add example.txt
```

複数の修正内容をまとめて1つの履歴として保存するための「目次」に追加
目次に追加するだけなので、まだ履歴に追加されていないことに注意

---
<!-- *template: invert -->

## add の実践

1. `example.txt` を作成
2. `git add example.txt` を実行
3. `git status` を実行
4. 以下のような表示を確認
    ```text
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            new file:   example.txt
    ```

---

## rm

作業ツリーからファイルを削除、インデックスにも登録する

```bash
git rm README.md
```

---
<!-- *template: invert -->

## rm の実践

1. `git rm README.md` を実行
2. `README.md` が削除されていることを確認
3. `git status` を実行
4. 以下のような表示を確認
    ```text
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            deleted:    README.md
            new file:   example.txt
    ```
    ※ `example.txt` も index に残っていることに注意

---

## commit

インデックスに登録された内容を履歴に登録する

```bash
git commit
```

ログを直接記述する場合は、`-m` を使う

```bash
git commit -m 'commit test'
```

---
<!-- *template: invert -->

## commit の実践

1. `git commit -m 'commit test'` を実行
2. `git status` を実行
3. 以下のような表示を確認
    ```text
    On branch master
    nothing to commit, working tree clean
    ```

---

## log

履歴を確認する

```bash
git log
```

log 単体では使いにくいため、通常様々なオプションを付加する

```bash
git log --graph --oneline --decorate=short
```

---
<!-- *template: invert -->

## log の実践

1. `git log --graph --oneline --decorate=short` を実行
2. 以下のような表示を確認
    ```text
    * 689a402 (HEAD -> master) commit test
    * fe78873 (origin/master) initial commit
    ```
    - master が origin/master より1つ進んでいることが確認できる				
---

## show

履歴の詳細を表示する

```bash
git show [コミットID]
```

コミットIDを省略した場合は、現在位置のコミット内容が表示される

`--stat` を使うとサマリーが確認できる

```bash
git show --stat [コミットID]
```

---
<!-- *template: invert -->

## show の実践

1. `git show` を実行
2. ログと共に修正内容が diff 形式で表示されることを確認
3. `git show --stat` を実行
4. 修正内容のサマリーが表示されることを確認

---
<!-- *template: invert -->

コミットした内容を一旦破棄しておく

※ reset については後ほど

1. `git reset --hard HEAD` を実行
2. `git log --graph --oneline --decorate=short` を実行
3. master が origin/master と同じであることを確認
    ```text
    * fe78873 (HEAD -> master, origin/master) initial commit
    ```

---

## 基本操作のおさらい

#### リモート操作関連

---

## push

ローカルの履歴を元にリモートリポジトリを更新する

```bash
git push origin master
```

上記の例では、ローカルの `master` ブランチの修正をリモートに送る

通常は「追跡ブランチ」となっていることが多いため、以下のように簡略化できる

```bash
git push
```

---
<!-- *template: invert -->

## push の実践

今は省略

`git@github.com:u-minor/pts-git.git` がリモート「origin」なので権限がなく、push できない

---

## fetch

リモートリポジトリの内容を取得・更新する

```bash
git fetch
```

追跡元のリモートから最新の情報を取得する

※ ローカルブランチは更新されないことに注意

---
<!-- *template: invert -->

## fetch の実践

事前準備: 講師が新しいファイルを追加し push する

1. `git fetch` を実行
2. ログと共に修正内容が diff 形式で表示されることを確認
3. `git show --stat` を実行
4. 修正内容のサマリーが表示されることを確認

---

## pull

fetch して、トラッキングしているローカルリポジトリにその内容を取り込む

```bash
git pull
```

pull = fetch + merge のイメージ

---
<!-- *template: invert -->

## pull の実践

1. `git pull` を実行
2. ログと共に修正内容が diff 形式で表示されることを確認
3. `git show --stat` を実行
4. 修正内容のサマリーが表示されることを確認
