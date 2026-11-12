# git-practice

git と GitHub の操作を、問題を解きながら練習するためのリポジトリ。作業用ブランチで変更し、プルリクエストで `develop` に入れ、`staging` に取り込み、サーバーに反映し、切り戻すまでの流れを扱う。

## 始め方
1. このリポジトリのページで「Use this template」→「Create a new repository」を押し、自分のアカウントに複製を作る(名前は自由、公開範囲は Public。「Include all branches」にはチェックを入れない)
2. 複製したリポジトリを、自分のPCに clone する

```bash
mkdir ~/git-practice
cd ~/git-practice
git clone <複製したリポジトリのURL> pc
cd pc
git remote -v
```

`git remote -v` に、自分の複製の URL が表示されることを確かめる。以降のコマンドは `~/git-practice` の下だけで打つ。

## 登場するもの

| もの | 役 |
| --- | --- |
| GitHub の自分の複製 | みんなで共有するリポジトリの役 |
| `~/git-practice/pc` | 自分のPCの役。変更して push する |
| `~/git-practice/server` | サーバーの役。GitHub から pull するだけ(問題1で作る) |

ブランチは `develop`(各自の変更を集める)と `staging`(サーバーに反映する)を使う。変更は「作業用ブランチ → `develop` → `staging` → サーバー」の順に流す。

## 進め方
- `problems/` の問題を、番号の順に解く。前の問題の結果を次の問題で使う
- 先に自分で打ってみて、表示を見てから「問い」に答える
- `answers/` は、解いたあとか、行き詰まったときに開く
- 期待した表示と違ったら、先へ進まずに原因を調べる

## 問題の一覧

| 番号 | 題 | 学べること |
| --- | --- | --- |
| 01 | ブランチを作って push する | commit と push の違い |
| 02 | 表示を読む | `git log`・`git diff`・`git status` の読み方 |
| 03 | プルリクエストを出して取り込む | base と compare、マージで増える commit |
| 04 | staging に取り込む | ブランチごとに届く範囲が違うこと |
| 05 | サーバー役に反映する | `git pull --ff-only` |
| 06 | reset で切り戻す | `git reset --hard` と強制 push |
| 07 | サーバー役を戻す | pull では戻らないこと |
| 08 | 入れ直して、revert で切り戻す | `git revert` と、reset との違い |
| 09 | ブランチ保護 | 強制 push を拒否する設定 |
| 10 | コンフリクトを解消する | 同じ所を変えたときの直し方 |

## 注意
- 問題08〜10の解答は、表示される文言や画面の項目名をまだ実機で確かめていない。違っていた箇所は、気づいたら直してほしいな
- GitHub の画面は変わることがある。ボタンや項目の名前が解答と違っていても、同じ意味のものを探す

## やり直すとき
`develop` と `staging` を問題1の「初期状態」の commit に戻せば、問題2からやり直せる(戻し方は問題6・7)。最初からやり直すときは、複製したリポジトリと `~/git-practice` を消して、作り直す。
