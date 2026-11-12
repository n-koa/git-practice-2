# 問題06 reset で切り戻す

`develop` と `staging` を、Trivy を入れる前(問題1の「初期状態」)に戻す。

## やること
1. `pc` で `develop` を最新にし、`git log --oneline -5` で commit の並びを確かめる
2. `git reset --hard` で、`develop` を「初期状態」に戻す
3. **まず普通の `git push` を打ち**、何が起きるかを見る
4. `git push --force-with-lease` で GitHub に反映する
5. `staging` でも 1.〜4.と同じことをする(3.は飛ばしてよい)
6. GitHub で、`develop` と `staging` の commit の一覧と、プルリクエストの一覧(Closed)を見る

## 問い
- Q1. `git reset --hard` に渡す英数字は、一覧のどの commit のものか
- Q2. reset の前に `git log` で確かめておくべきことは何か
- Q3. 3.の `git push` はどうなったか。それはなぜか
- Q4. `--force` ではなく `--force-with-lease` を使う利点は何か
- Q5. reset のあと、「Trivyの定期診断を追加」の commit はどこにも残っていないか
- Q6. 問題03・04のプルリクエストは、GitHub でどう表示されているか
