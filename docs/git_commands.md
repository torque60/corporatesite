# Gitコマンド一覧

このドキュメントでは、Gitの基本的な使い方からチーム開発で役立つ応用コマンドまでを解説します。

## 目次

1. [**基本コマンド**](#1-基本コマンド)
    - [1.1 git init](#git-init)
    - [1.2 git clone](#git-clone)
    - [1.3 git add](#git-add)
    - [1.4 git commit](#git-commit)
    - [1.5 git status](#git-status)
    - [1.6 git log](#git-log)
    - [1.7 git diff](#git-diff)
2. [**取り消し・修復コマンド**](#2-取り消し修復コマンド)
    - [2.1 git restore](#git-restore)
    - [2.2 git reset](#git-reset)
    - [2.3 git revert](#git-revert)
3. [**ブランチ・同期コマンド**](#3-ブランチ同期コマンド)
    - [3.1 git branch](#git-branch)
    - [3.2 git switch / git checkout](#git-switch--git-checkout)
    - [3.3 git merge](#git-merge)
    - [3.4 git remote](#git-remote)
    - [3.5 git fetch](#git-fetch)
    - [3.6 git pull](#git-pull)
    - [3.7 git push](#git-push)
    - [3.8 git stash](#git-stash)
4. [**チーム開発における重要トピック**](#4-チーム開発における重要トピック)
    - [4.1 プルリクエスト](#プルリクエスト-pull-request)
    - [4.2 コンフリクトの解消](#コンフリクト衝突の解消)
    - [4.3 マージの際の注意点](#マージの際の注意点)
    - [4.4 取り消しの注意点](#取り消しの注意点)

---

## はじめに：Gitコマンドの基本書式

Gitコマンドは基本的に以下の形式で構成されています。

```bash
git <コマンド> [<オプション>] [<引数>]
```

1.  **`git`**: Gitプログラムを呼び出す起点です。
2.  **`<コマンド>`**: 行いたい具体的な操作（例：`add`, `commit`）を指定します。
3.  **`[<オプション>]`**: 操作の詳細を微調整します（例：`-m "メッセージ"`）。通常は `-` や `--` で始まります。
4.  **`[<引数>]`**: 操作の対象（例：ファイル名、ブランチ名）を指定します。

---

## 1. 基本コマンド

プロジェクトを開始し、変更を記録するための最も基本的なコマンドです。

### git init
```bash
git init
```
> **Git 公式の定義**:
> 「空の Git リポジトリを作成するか、既存のリポジトリを再初期化します。」 [1]

**コマンド説明**
現在のディレクトリを Git の管理下に置きます。実行すると `.git` という隠しディレクトリが作成され、履歴の管理が可能になります。

**オプション説明**
- (特になし。主に新規プロジェクト開始時に引数なしで実行します。)

**このセクションの出典:**
- [1] [git-init - Git Documentation](https://git-scm.com/docs/git-init)

---

### git clone
```bash
git clone <リポジトリURL>
```
> **Git 公式の定義**:
> 「リポジトリを新しいディレクトリにクローン（複製）します。」 [2]

**コマンド説明**
既存のリポジトリ（GitHub上のプロジェクトなど）を、自分のローカル環境に丸ごとコピーします。

**オプション説明**
- `--depth <数字>`: 指定した回数分だけの履歴を取得します（軽量なクローン）。

**このセクションの出典:**
- [2] [git-clone - Git Documentation](https://git-scm.com/docs/git-clone)

---

### git add
```bash
git add <ファイル名>
```
```bash
git add .
```
> **Git 公式の定義**:
> 「ファイルの内容をインデックス（ステージングエリア）に追加します。」 [3]

**コマンド説明**
変更したファイルを、次のコミット（保存）の対象として準備状態にします。

**オプション説明**
- `.` (ドット): 現在のディレクトリ以下のすべての変更をステージングします。
- `-u`: 追跡済みのファイルのみ、変更・削除をステージングします（新規ファイルは含みません）。

**このセクションの出典:**
- [3] [git-add - Git Documentation](https://git-scm.com/docs/git-add)

---

### git commit
```bash
git commit -m "<メッセージ>"
```
> **Git 公式の定義**:
> 「インデックスの現在の内容を、プロジェクトの変更内容を記述するログメッセージとともに、新しいコミットとして格納します。」 [4]

**コマンド説明**
`git add` で準備した変更を、一つの「歴史（コミット）」としてリポジトリに記録します。

**オプション説明**
- `-m "<メッセージ>"`: コマンドラインから直接コミットメッセージを指定します。
- `-a` (または `--all`): 変更された既存のファイルを自動的に `add` してコミットします。

**このセクションの出典:**
- [4] [git-commit - Git Documentation](https://git-scm.com/docs/git-commit)

---

### git status
```bash
git status
```
> **Git 公式の定義**:
> 「ワーキングツリーの状態を表示します。」 [5]

**コマンド説明**
どのファイルが変更されているか、どのファイルがステージングされているか（`add` 済みか）など、現在の状態を確認します。

**オプション説明**
- `-s` (または `--short`): 出力を短縮形式で表示します。

**このセクションの出典:**
- [5] [git-status - Git Documentation](https://git-scm.com/docs/git-status)

---

### git log
```bash
git log
```
> **Git 公式の定義**:
> 「コミットログを表示します。」 [6]

**コマンド説明**
リポジトリのこれまでの変更履歴（コミットの一覧）を表示します。

**オプション説明**
- `--oneline`: 各コミットを1行でコンパクトに表示します。
- `-n <数字>`: 直近の指定した数だけコミットを表示します。
- `--graph`: 履歴の流れ（ブランチの分岐・合流）を視覚的に表示します。

**このセクションの出典:**
- [6] [git-log - Git Documentation](https://git-scm.com/docs/git-log)

---

### git diff
```bash
git diff
```
> **Git 公式の定義**:
> 「コミット間、コミットとワーキングツリー間などの変更を表示します。」 [7]

**コマンド説明**
ファイル内の具体的な変更内容（どの行を消して何を追加したか）の差分を確認します。

**オプション説明**
- `--cached` (または `--staged`): ステージングされた（`add` 済みの）変更内容を確認します。

**このセクションの出典:**
- [7] [git-diff - Git Documentation](https://git-scm.com/docs/git-diff)

---

## 2. 取り消し・修復コマンド

間違えた操作を元に戻すためのコマンドです。チーム開発では影響範囲に注意が必要です。

### git restore
```bash
git restore <ファイル名>
```
```bash
git restore --staged <ファイル名>
```
> **Git 公式の定義**:
> 「ワーキングツリーのファイルを復元します。」 [8]

**コマンド説明**
ファイルの変更を元に戻したり、`add` したファイルをステージングから外したりします。

**オプション説明**
- `--staged`: `add` した変更を取り消し、ステージング前の状態に戻します（ファイルの中身は変わりません）。
- (オプションなし): ワーキングツリーでの変更を破棄して、最後にコミットした時の状態に戻します。

**このセクションの出典:**
- [8] [git-restore - Git Documentation](https://git-scm.com/docs/git-restore)

---

### git reset
```bash
git reset <コミットID>
```
> **Git 公式の定義**:
> 「現在の HEAD を指定した状態にリセットします。」 [9]

**コマンド説明**
履歴を特定の時点まで巻き戻します。コミットを取り消したい場合に主に使用します。

**オプション説明**
- `--soft`: コミットだけを取り消し、変更内容はステージングされた状態で残します。
- `--mixed` (デフォルト): コミットとステージングを取り消し、変更内容はワーキングツリーに残します。
- `--hard`: 指定した時点以降のすべての変更（ファイル内容含む）を完全に破棄します。**※元に戻せないので注意。**

**このセクションの出典:**
- [9] [git-reset - Git Documentation](https://git-scm.com/docs/git-reset)

---

### git revert
```bash
git revert <コミットID>
```
> **Git 公式の定義**:
> 「既存のいくつかのコミットを元に戻すための新しいコミットを作成します。」 [10]

**コマンド説明**
特定のコミットで行った変更を「打ち消す内容のコミット」を新しく作成します。履歴を削除せずに元に戻すため、共有リポジトリで安全に使用できます。

**オプション説明**
- `-n` (または `--no-commit`): 打ち消し内容をステージングするだけで、自動的にコミットはしません。

**このセクションの出典:**
- [10] [git-revert - Git Documentation](https://git-scm.com/docs/git-revert)

---

## 3. ブランチ・同期コマンド

並行して開発を進めたり、リモートサーバーと情報を同期したりするためのコマンドです。

### git branch
```bash
git branch
```
```bash
git branch <ブランチ名>
```
> **Git 公式の定義**:
> 「ブランチの一覧表示、作成、削除を行います。」 [11]

**コマンド説明**
現在のブランチ一覧を表示したり、新しいブランチを作成したりします。

**オプション説明**
- `-a`: ローカルとリモートの両方のブランチを表示します。
- `-d <ブランチ名>`: 指定したブランチを削除します（マージ済みの場合のみ）。

**このセクションの出典:**
- [11] [git-branch - Git Documentation](https://git-scm.com/docs/git-branch)

---

### git switch / git checkout
```bash
git switch <ブランチ名>
```
```bash
git checkout <ブランチ名>
```
> **Git 公式の定義 (switch)**:
> 「ブランチを切り替えます。」 [12]

**コマンド説明**
作業するブランチを切り替えます。最新の Git ではブランチの切り替えには `switch`、ファイルの復元などには `restore` を使うことが推奨されています。

**オプション説明**
- `-c <ブランチ名>`: 新しいブランチを作成して、同時にそのブランチに切り替えます（`git checkout -b` と同等）。

**このセクションの出典:**
- [12] [git-switch - Git Documentation](https://git-scm.com/docs/git-switch)

---

### git merge
```bash
git merge <ブランチ名>
```
> **Git 公式の定義**:
> 「2つ以上の開発履歴を統合します。」 [13]

**コマンド説明**
別のブランチ（例：feature）で行った変更を、現在のブランチ（例：main）に取り込みます。

**オプション説明**
- `--no-ff`: ファストフォワード（履歴の単純な継承）が可能であっても、必ずマージコミットを作成します。
- `--abort`: コンフリクト（衝突）が発生した際に、マージ作業を中止して元の状態に戻します。

**このセクションの出典:**
- [13] [git-merge - Git Documentation](https://git-scm.com/docs/git-merge)

---

### git remote
```bash
git remote -v
```
> **Git 公式の定義**:
> 「追跡対象のリポジトリ（リモート）を管理します。」 [14]

**コマンド説明**
ローカルリポジトリと通信するリモートリポジトリ（GitHubなど）の設定を確認・管理します。

**オプション説明**
- `add <名前> <URL>`: 新しいリモートリポジトリを登録します。
- `-v`: 登録されているリモートのURLを詳細表示します。

**このセクションの出典:**
- [14] [git-remote - Git Documentation](https://git-scm.com/docs/git-remote)

---

### git fetch
```bash
git fetch <リモート名>
```
> **Git 公式の定義**:
> 「別のリポジトリからオブジェクトと参照をダウンロードします。」 [15]

**コマンド説明**
リモートリポジトリの最新情報をローカルに取得します。現在のブランチのファイル内容には反映（マージ）されません。

**オプション説明**
- `--prune`: リモートで削除されたブランチを、ローカルの追跡ブランチからも削除します。

**このセクションの出典:**
- [15] [git-fetch - Git Documentation](https://git-scm.com/docs/git-fetch)

---

### git pull
```bash
git pull <リモート名> <ブランチ名>
```
> **Git 公式の定義**:
> 「別のリポジトリやローカルブランチから取得し、現在のブランチに統合します。」 [16]

**コマンド説明**
リモートリポジトリの変更を取得し、さらに現在のブランチにマージします（`fetch` + `merge`）。

**オプション説明**
- `--rebase`: マージの代わりにリベースを使用して履歴を統合します。

**このセクションの出典:**
- [16] [git-pull - Git Documentation](https://git-scm.com/docs/git-pull)

---

### git push
```bash
git push <リモート名> <ブランチ名>
```
> **Git 公式の定義**:
> 「関連するオブジェクトとともに、リモートの参照を更新します。」 [17]

**コマンド説明**
ローカルで作成したコミットをリモートリポジトリに送信して反映させます。

**オプション説明**
- `-u` (または `--set-upstream`): 次回から `git push` だけで済むように、リモートブランチを追跡対象として設定します。
- `-f` (または `--force`): リモートの履歴を強制的に上書きします。**※共有リポジトリでは非推奨。**

**このセクションの出典:**
- [17] [git-push - Git Documentation](https://git-scm.com/docs/git-push)

---

### git stash
```bash
git stash
```
```bash
git stash pop
```
> **Git 公式の定義**:
> 「汚れたワーキングディレクトリの変更を一時的に保存します。」 [18]

**コマンド説明**
作業途中の変更を一時的に別の場所に退避させます。ブランチを切り替えたいが、まだコミットしたくない場合に便利です。

**オプション説明**
- `list`: 退避させている作業の一覧を表示します。
- `apply`: 退避させた作業を、リストから削除せずに現在のブランチに適用します。
- `pop`: 最新の退避作業を現在のブランチに適用し、リストから削除します。

**このセクションの出典:**
- [18] [git-stash - Git Documentation](https://git-scm.com/docs/git-stash)

---

## 4. チーム開発における重要トピック

チームでGitを運用する際に不可欠な知識と注意点です。

### プルリクエスト (Pull Request)
> **GitHub の定義**:
> 「プルリクエストは、GitHub上のリポジトリにあるブランチにプッシュした変更について、他の人に知らせるための機能です。」 [19]

**概要と流れ**
プルリクエスト（PR）は、自分の変更をメインのブランチに統合してもらうための「リクエスト」です。
1.  **ブランチ作成**: 機能ごとに新しいブランチを作成します。
2.  **プッシュ**: 変更をコミットし、リモートにプッシュします。
3.  **PR作成**: GitHub等の管理画面からPRを作成し、レビューを依頼します。
4.  **レビュー・修正**: 他のメンバーがコードを確認し、必要に応じて修正を行います。
5.  **マージ**: 承認されたら、メインブランチに統合されます。

**このセクションの出典:**
- [19] [About pull requests - GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

---

### コンフリクト（衝突）の解消
> **Git 公式の定義**:
> 「同じ行に対して異なる変更が行われた場合、Gitはそれらを自動的にマージできず、コンフリクトが発生します。」 [13]

**解消手順**
1.  **発生の検知**: `git merge` 等の際に「CONFLICT」というメッセージが表示されます。
2.  **ファイルの編集**: 衝突箇所には以下のマーカーが挿入されます。
    ```text
    <<<<<<< HEAD
    自分の変更内容
    =======
    取り込みたい相手の変更内容
    >>>>>>> branch-name
    ```
3.  **修正**: どちらのコードを残すか（あるいは両方を統合するか）を手動で編集し、マーカーを削除します。
4.  **完了**: `git add <修正したファイル>` を行い、`git commit` でマージを完了させます。

**中止する場合**
- `git merge --abort`: マージ作業を中断し、開始前の状態に戻します。

---

### マージの際の注意点
チーム開発で安全にマージを行うためのチェックリストです。

- **事前に最新化する**: マージを行う前に、必ず `git pull` でリモートの最新状態を手元に反映させてください。
- **ブランチを確認する**: 現在どのブランチにいるか（`git branch`）を必ず確認してから実行してください。
- **ローカルでテストする**: マージ直後にコードが正常に動作するか、ローカル環境で確認してから `push` してください。

---

### 「取り消し」コマンドの注意点
不用意な取り消しは、チームメンバーの作業を壊す可能性があります。

- **共有履歴のリセット禁止**: すでにリモートに `push` され、他のメンバーが共有しているコミットに対して `git reset` を行ってはいけません。履歴が食い違い、大きな混乱を招きます。
- **`git revert` の活用**: 共有済みの変更を取り消したい場合は、履歴を削除する `reset` ではなく、打ち消し履歴を残す `revert` を使用するのがチーム開発の鉄則です。
- **`--hard` のリスク**: `git reset --hard` は未コミットの作業もすべて消去します。実行前に `git stash` で避難させるか、本当に消して良いか再確認してください。

---

## 出典・参考文献（整理）

1. [git-init - Git Documentation](https://git-scm.com/docs/git-init) [1]
2. [git-clone - Git Documentation](https://git-scm.com/docs/git-clone) [2]
3. [git-add - Git Documentation](https://git-scm.com/docs/git-add) [3]
4. [git-commit - Git Documentation](https://git-scm.com/docs/git-commit) [4]
5. [git-status - Git Documentation](https://git-scm.com/docs/git-status) [5]
6. [git-log - Git Documentation](https://git-scm.com/docs/git-log) [6]
7. [git-diff - Git Documentation](https://git-scm.com/docs/git-diff) [7]
8. [git-restore - Git Documentation](https://git-scm.com/docs/git-restore) [8]
9. [git-reset - Git Documentation](https://git-scm.com/docs/git-reset) [9]
10. [git-revert - Git Documentation](https://git-scm.com/docs/git-revert) [10]
11. [git-branch - Git Documentation](https://git-scm.com/docs/git-branch) [11]
12. [git-switch - Git Documentation](https://git-scm.com/docs/git-switch) [12]
13. [git-merge - Git Documentation](https://git-scm.com/docs/git-merge) [13]
14. [git-remote - Git Documentation](https://git-scm.com/docs/git-remote) [14]
15. [git-fetch - Git Documentation](https://git-scm.com/docs/git-fetch) [15]
16. [git-pull - Git Documentation](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository#pulling-changes-from-a-remote-repository) [16]
17. [git-push - Git Documentation](https://git-scm.com/docs/git-push) [17]
18. [git-stash - Git Documentation](https://git-scm.com/docs/git-stash) [18]
19. [About pull requests - GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) [19]
