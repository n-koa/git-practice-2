# 解答06 reset で切り戻す

## 手順
### 1〜4. develop を戻す(pc)
```bash
cd ~/git-practice/pc
git switch develop
git pull
git log --oneline -5
git reset --hard <初期状態の commit>
git log --oneline -3
git push
git push --force-with-lease
```

| コマンド | 意味 |
| --- | --- |
| `git log --oneline -5` | 最近の commit を5つ表示する。戻す先より後にある commit を確かめる |
| `git reset --hard <commit>` | ブランチを、指定した commit の状態に戻す。それより後の commit は、このブランチからなくなる。`--hard` はファイルの中身も戻す |
| `git push --force-with-lease` | 戻した状態を GitHub に送る。GitHub 側が、自分の把握している状態と違うときは止まる |

期待結果：
- 1回目の `git log` が、上から「Merge pull request #1 …」「Trivyの定期診断を追加」「初期状態」「Initial commit」の順に並ぶ
- `git reset` のあと `HEAD is now at <初期状態の commit> 初期状態` と表示される
- 2回目の `git log` の一番上が「初期状態」
- `git push` は拒否される(`! [rejected] develop -> develop (non-fast-forward)`、`Updates were rejected because the tip of your current branch is behind`)
- `git push --force-with-lease` で `+ <英数字>...<英数字> develop -> develop (forced update)` と表示される

### 5. staging を戻す(pc)
```bash
git switch staging
git pull
git log --oneline -5
git reset --hard <初期状態の commit>
git push --force-with-lease
```
期待結果： `HEAD is now at <初期状態の commit> 初期状態`、`+ … staging -> staging (forced update)` と表示される。

### 6. GitHub で確かめる(GitHub)
1. 「Code」タブで、左上のブランチ名のボタンから `develop` を選ぶ
2. ファイル一覧のすぐ上の帯の右端にある、時計の印の「○ Commits」を押す(画面の幅が狭いと、時計の印だけになる)
3. `staging` でも同じように見る
4. 「Pull requests」を開き、「Closed」を押す

期待結果：
- どちらのブランチも「2 Commits」で、一番上が「初期状態」
- 問題03・04のプルリクエストが、紫の「Merged」の印のまま残っている

## 問いの答え
- A1. 戻りたい先の commit。ここでは「初期状態」。一番上の commit は今いる場所なので、それを指定しても何も変わらない
- A2. 戻す先より後にある commit が、消してよいものだけであること。reset は、指定した commit より後をすべてこのブランチから外す。途中にほかの人の commit が入っていると、それも一緒に消える
- A3. 拒否された。自分の `develop` は GitHub の `develop` より「後ろ」にいて、そのまま受け入れると GitHub 側の commit が消えるため、git は通常の push では受け付けない
- A4. `--force` は GitHub 側がどうなっていても上書きする。`--force-with-lease` は、最後に確認したあとでだれかが push していたら止まるので、ほかの人の commit を知らずに消すことを防げる
- A5. 残っている。`feature/add-trivy` ブランチには触れていないので、自分のPCにも GitHub にも、その commit がある。reset が外したのは `develop` と `staging` からだけ
- A6. 「Merged」のまま残っている。ブランチの履歴からは消えても、プルリクエストの記録は消えない。reset で戻した場合、「一度入れた」ことが分かるのはここだけになる
