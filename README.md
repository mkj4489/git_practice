# gitコマンド
## １行で表示する

```
git log --oneline
```
## ファイル指定で変更差分を表示する
```
git log -p index.html
```
## 表示するコミット数を制限する
```
git log -n <コミット数>
```
## ローカルとリポジトリからファイルを削除する
```
git rm <ファイル名>
git rm -r <ディレクトリ名>
```
## リモートリポジトリ（github）を新規追加する
```
originというショートカットでurlのリモートリポジトリを登録する
git remote add origin htttps://github.com/user/repo.git
```
## ステージした変更を取り消す
- ステージから取り消すだけなので、ローカルのファイルには影響を与えない
```
git reset HEAD <ファイル名>
git reset HEAD <ディレクトリ名>

全変更を取り消す
git reset HEAD .
```
## リモートから情報をローカルに取得する 　ワークツリーには反映されない
```
git fetch <リモート名>
git fetch origin
ワークツリーに反映する場合は、
git merge <反映するブランチ名>
```
## fetch + merge = pull （注意：pullのときにマージされるのは今いるブランチ）
```
git pull
```
## ブランチの一覧を表示する
```
すべてのブランチを表示する（リモートブランチも）
git branch -a 
リモートブランチからブランチを作成
git branch master origin/master
```
## ブランチを切り替える
```
ブランチを切り替える
git checkout <既存ブランチ名>
ブランチを新規作成して切り替える
git checkout -b <新ブランチ名>
```
## 変更履歴をマージする
```
git merge <ブランチ名>
git merge <リモート名/ブランチ名>
リモートブランチを作業中のブランチにマージする
git merge origin/master 
```
## コンフリクト
- コンフリクトが起きにくくするには
  - 複数人で同じファイルを変更しない
  - pullやmergeする前に変更中の状態をなくしておく（commitしておく）
  - pullするときには、pullするブランチに移動してからpullする
```
コンフリクトが起きた場合に確認
git status
コンフリクトしたファイルを修正後
git add <ファイル名>
git commit 
```
## ローカルブランチを変更・削除する
```
自分が作業しているブランチのブランチ名を変更する
git branch -m <ブランチ名>
ローカルブランチを削除する
git branch -d <ブランチ名>
強制的に削除する
git branch -D <ブランチ名>
```
## ブランチを利用した開発の流れ
- masterブランチをリリース用のブランチとして、開発はトピックブランチを作成して進めるのが基本

## GitHub Flow
- masterブランチからブランチを作成
- ファイルを変更しコミット
- 同名のブランチをgithubへプッシュ
- プルリクエストを送る
- コードレビューをし、masterブランチにマージ
- masterブランチをデプロイ
  - masterブランチは常にデプロイできる状態を保つ
  - 新開発はmasterブランチから新しいブランチを作成してスタート
  - 作成した新しいブランチ上で作業しコミットする
  - 定期的にpushする
  - masterにマージするためにプルリクエストを使う
  - 必ずレビューを受ける
  - masterブランチにマージしたらすぐにデプロイする
    - テストとデプロイ作業は自動化

## rebase　履歴を整えた形で変更を統合する
- githubにpushしたコミットをリベースすると、pushできなくなるのでしてはいけない。
- githubにpushしたコミットをリベースするのは、NG
- git push -f は絶対NG
```
git rebase <ブランチ名>
```
## マージとリベース
- プッシュしていないローカルの変更には、rebaseを使い、プッシュした後は、margeを使う
- コンフリクトしそうなら、マージを使う

## pullの設定をリベースに変更する
- pullには、マージ型とリベース型が存在する
- pullのマージ型
```
pullのマージ型　マージのコミットが残る
git pull origin master
pullのリベース型　マージのコミットが残らない
git pull --rebase origin master
pullをリベース型に設定する
git config --global pull.rebase true
masterブランチでpullするときのみ設定する
git config branch.master.rebase true
```
## コミットを並び替える、削除する
```
コミットを３つ変更する（git logを逆の順で表示される。一番上が一番古いコミット）
git rebase -i HEAD~3
立ち上がったエディタで、コミットの順番を入れ替えるか、削除したい場合はコミットログを削除する。

コミットを１つにまとめる
pickの部分を、squashに変更する

コミットを分割する
pickの部分を、editに変更する
```
## タグ
```
tagのデータを表示する
git show <タグ名>
git show 20180923_01
tagを追加する
git tag <タグ名>
タグをリモートリポジトリに送信する
git push origin <タグ名>
```
## 作業を一時避難する
```
ワークツリーとステージの変更をなかったことになる
git stash
避難した作業を確認する
git stash list
避難した作業を復元する
最新のを復元
git stash apply
特定の作業1を復元する
git stash apply stash@{1}
一時避難した作業を削除する
git stash drop
特定の作業１を削除する
git stash drop stash@{1}

```
