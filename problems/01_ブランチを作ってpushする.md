# 問題01 ブランチを作って push する

## やること
1. `pc` で `develop` ブランチを作る
2. `compose.staging.yml` というファイルを、中身が `services:` の1行だけで作り、「初期状態」という説明で commit する
3. **push する前に**、GitHub のブランチの一覧を見る
4. `develop` を GitHub に push し、もう一度ブランチの一覧を見る
5. 今の内容から `staging` ブランチを作り、push する
6. `~/git-practice/server` に、`staging` ブランチの状態で clone する(サーバー役)
7. 「初期状態」の commit の英数字を控える(問題6で使う)

## 問い
- Q1. 3.の時点で、GitHub に `develop` はあったか。それはなぜか
- Q2. `git push -u origin develop` の `-u` は何のために付けるか
- Q3. `pc` と `server` は同じPCの中にある。`pc` で commit すると、`server` の中身は変わるか
