# 解答08 入れ直して、revert で切り戻す

## 手順
### 1. もう一度入れる(GitHub)
1. 「Pull requests」→「New pull request」を開き、「base」を `develop`、「compare」を `feature/add-trivy` にする
2. 「Create pull request」→「Merge pull request」→「Confirm merge」を押す
3. 「merged commit <英数字> into develop」の英数字を控える

### 2〜3. revert する(pc)
```bash
cd ~/git-practice/pc
git switch develop
git pull
git log --oneline -3
git switch -c revert/add-trivy
git revert -m 1 --no-edit <1.で控えたマージの commit>
git show --stat HEAD
cat compose.staging.yml
git push -u origin revert/add-trivy
```

| コマンド | 意味 |
| --- | --- |
| `git revert <commit>` | 指定した commit の変更を打ち消す、新しい commit を作る。もとの commit は消えない |
| `-m 1` | マージの commit を打ち消すときの指定。1番目の親(取り込まれた側。ここでは `develop`)を基準にして、取り込んだ変更を打ち消す |
| `--no-edit` | commit の説明を編集する画面を開かず、自動で付く説明(`Revert "…"`)をそのまま使う |
| `git show --stat HEAD` | 今作った commit で変わったファイルを表示する |

期待結果：
- `git show` に `Revert "Merge pull request #… "` と `compose.staging.yml | 1 -` が出る
- ファイルの中身が `services:` の1行

### 4. revert を取り込む(GitHub)
「base」を `develop`、「compare」を `revert/add-trivy` にしてプルリクエストを作り、マージする。

### 5. 確かめる(pc)
```bash
git switch develop
git pull
git log --oneline -6
cat compose.staging.yml
```
期待結果： `git pull` は `Fast-forward`(強制 push は要らなかった)。`git log` は上から、revert のプルリクエストのマージ、`Revert "Merge pull request #…"`、1.のマージ、「Trivyの定期診断を追加」、「初期状態」の順。ファイルの中身は `services:` の1行。

### 6. もう一度入れようとする(GitHub)
「New pull request」で「base」を `develop`、「compare」を `feature/add-trivy` にする。

期待結果： 「There isn't anything to compare.」のように表示され、プルリクエストを作れない。

## 問いの答え
- A1. reset は `develop` から commit を外しただけで、`feature/add-trivy` にはその commit が残っている。`develop` から見ると「まだ取り込んでいない commit」に戻ったので、もう一度プルリクエストにできる
- A2. マージの commit には親が2つある(取り込まれた側と、取り込んだ側)。どちらを基準にして打ち消すかを決めないと、何を消せばよいかが決まらない
- A3. 入れた commit も、打ち消した commit も、すべて残っている。ファイルの中身は「初期状態」と同じ。履歴は進んだが、中身は戻った
- A4. 作れなかった。`feature/add-trivy` の commit は `develop` の履歴に入ったままなので、git は「取り込み済み」と見なす(中身が打ち消されていても)。入れ直すには、revert の commit をさらに revert するか、同じ変更を新しい commit として作り直す
- A5.

| 観点 | reset | revert |
| --- | --- | --- |
| 履歴に「入れた」記録が残るか | 残らない(プルリクエストの記録だけ残る) | 残る(入れた commit と打ち消した commit の両方) |
| GitHub への反映に強制 push が要るか | 要る | 要らない(通常の push とプルリクエスト) |
| サーバー側は `git pull` で戻るか | 戻らない(サーバーでも reset が要る) | 戻る |
| 同じブランチから入れ直せるか | 入れ直せる | 入れ直せない(revert の revert が要る) |

共有しているブランチでは、履歴を書き換えない revert が基本。reset は、強制 push ができて、ほかの人がそのブランチをまだ取り込んでいないときに限って使える。
