# 解答04 staging に取り込む

## 手順
### 1. 取り込まれる commit を見る(server)
```bash
cd ~/git-practice/server
git fetch
git log --oneline HEAD..origin/staging
```

| コマンド | 意味 |
| --- | --- |
| `git fetch` | GitHub の最新の情報を取ってくる。自分のファイルはまだ変わらない |
| `git log --oneline HEAD..origin/staging` | 今いる場所(`HEAD`)にはなく、GitHub の `staging` にはある commit を一覧で表示する |

期待結果： `git fetch` の表示に `develop -> origin/develop` の行はあるが、`staging` の行はない。`git log` は何も表示しない。

### 2. develop を staging に入れる(GitHub)
1. 「Pull requests」→「New pull request」を開く
2. 「base」を `staging`、「compare」を `develop` にする
3. 「Able to merge.」を確かめ、「Create pull request」を押す
4. 「Merge pull request」→「Confirm merge」を押す

期待結果： 2.の画面の下に「2 commits」「1 file changed」と表示される。4.のあと「Pull request successfully merged and closed」と表示される。

### 3. もう一度見る(server)
```bash
git fetch
git log --oneline HEAD..origin/staging
```
期待結果： `git fetch` の表示に `staging -> origin/staging` の行が出る。`git log` に commit が3つ並ぶ。

## 問いの答え
- A1. 何も表示されなかった。この時点では GitHub の `staging` に何も入っておらず、サーバー役と同じ状態だから
- A2. 3つ。「Trivyの定期診断を追加」、問題03のマージの commit、今回(`develop` → `staging`)のマージの commit
- A3. `develop` と `staging` は別のブランチで、`develop` にマージしても `staging` は動かない。サーバー役が見ているのは `staging` なので、`staging` に取り込まれるまで届かない。「マージされたのにサーバーに来ない」ときは、まずここを疑う
