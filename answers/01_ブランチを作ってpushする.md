# 解答01 ブランチを作って push する

## 手順
### 1〜2. develop を作り、ファイルを commit する(pc)
```bash
cd ~/git-practice/pc
git switch -c develop
echo "services:" > compose.staging.yml
git add compose.staging.yml
git commit -m "初期状態"
```

| コマンド | 意味 |
| --- | --- |
| `git switch -c develop` | `develop` ブランチを作り、切り替える |
| `echo "services:" > compose.staging.yml` | 1行だけのファイルを作る(`>` は上書き) |
| `git add …` | ファイルを commit の対象に加える |
| `git commit -m "…"` | 変更を、説明を付けて記録する |

期待結果： `[develop <英数字>] 初期状態` と表示される。

### 3. push する前に GitHub を見る(GitHub)
リポジトリのページで、左上のブランチ名のボタン(`main`)を押す。

期待結果： 一覧にあるのは `main` だけ。`develop` はない。

### 4〜5. push する(pc)
```bash
git push -u origin develop
git switch -c staging
git push -u origin staging
```

| コマンド | 意味 |
| --- | --- |
| `git push -u origin develop` | `develop` を GitHub(`origin`)に送る |
| `git switch -c staging` | 今の内容から `staging` を作り、切り替える |

期待結果： それぞれ `branch '…' set up to track 'origin/…'` と表示され、GitHub のブランチの一覧に `develop` と `staging` が出る。

### 6. サーバー役を作る(自分のPC)
```bash
cd ~/git-practice
git clone -b staging <複製したリポジトリのURL> server
cd server
git branch --show-current
cat compose.staging.yml
```

| コマンド | 意味 |
| --- | --- |
| `git clone -b staging <URL> server` | リポジトリを `server` フォルダに写し、`staging` ブランチにする |
| `git branch --show-current` | 今のブランチの名前を表示する |

期待結果： ブランチが `staging`、ファイルの中身が `services:` の1行。

### 7. 「初期状態」の commit を控える(pc)
```bash
cd ~/git-practice/pc
git log --oneline -1
```
期待結果： `<英数字> (HEAD -> staging, origin/staging, origin/develop, develop) 初期状態`。先頭の英数字を控える。

## 問いの答え
- A1. なかった。commit は自分のPCの中に記録するだけで、GitHub には何も送らない。push して初めて GitHub に届く
- A2. このブランチが GitHub のどのブランチと対になるかを覚えさせるため。次からは `git push`・`git pull` だけで同じ先を使える
- A3. 変わらない。`pc` と `server` は別々の clone で、互いを知らない。どちらも GitHub とだけやり取りする。`server` が変わるのは、`server` の中で pull などをしたときだけ
