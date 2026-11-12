# 問題08 入れ直して、revert で切り戻す

問題06では reset で戻した。今度は同じ変更をもう一度入れ、revert で戻して、違いを比べる。この問題は `develop` だけで行う。

## やること
1. GitHub で、`feature/add-trivy` を `develop` に入れるプルリクエストをもう一度作り、マージする。マージの commit を控える
2. `pc` で `develop` を最新にし、作業用ブランチ `revert/add-trivy` を作る
3. 1.のマージの commit を `git revert` で打ち消し、push する
4. GitHub で、`revert/add-trivy` を `develop` に入れるプルリクエストを作り、マージする
5. `pc` で `develop` を最新にし、`git log --oneline -6` と `compose.staging.yml` の中身を確かめる
6. GitHub で、`feature/add-trivy` を `develop` に入れるプルリクエストを、さらにもう一度作ろうとしてみる

## 問い
- Q1. 1.で、reset のあとなのに同じブランチからプルリクエストを作り直せたのはなぜか
- Q2. マージの commit を revert するとき、`-m 1` が必要なのはなぜか
- Q3. 5.の `git log` には何が並んでいるか。ファイルの中身はどうなっているか
- Q4. 6.はどうなったか。それはなぜか。revert したものを入れ直すには、どうすればよいか
- Q5. 次の表を埋めよ

| 観点 | reset | revert |
| --- | --- | --- |
| 履歴に「入れた」記録が残るか |  |  |
| GitHub への反映に強制 push が要るか |  |  |
| サーバー側は `git pull` で戻るか |  |  |
| 同じブランチから入れ直せるか |  |  |
