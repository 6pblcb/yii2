Yii 2 寄稿者のための Git ワークフロー
=====================================

Yii の開発に寄稿したい、ですって? すばらしい! けれども、あなたの修正案が速やかに採用されるチャンスを増やすためには、
どうか以下のステップを踏むようにしてください (最初の二つのステップは最初に寄稿するときにだけ必要になります)。
あなたが git と github については初めてだという場合は、最初に [github help](http://help.github.com/) や [try git](https://try.github.com) を精査したり、[git internal data model](http://nfarina.com/post/9868516270/git-is-simpler) についていくらか学習したりする必要があるかもしれません。

### 1. github で Yii リポジトリを [Fork](http://help.github.com/fork-a-repo/) し、あなたのフォークをあなたの開発環境にクローンする

```
git clone git@github.com:YOUR-GITHUB-USERNAME/yii2.git
```

Linux において、GitHub で GIT を設定するのに問題が生じたり、"Permission Denied (publickey)" のようなエラーが発生したりする場合は、
[setup your GIT installation to work with GitHub](http://help.github.com/linux-set-up-git/) に従ってください。

### 2. メインの Yii リポジトリを "upstream" という名前でリモートリポジトリとして追加する

Yii をクローンしたディレクトリ、すなわち "yii2" に入って、以下のコマンドを打ち込みます:

```
git remote add upstream git://github.com/yiisoft/yii2.git
```

### 3. あなたが取り組んでいる問題が、修正するために著しい努力を要求するものである場合は、それに対する issue が作成されていることを確認する

全ての新機能とバグ修正は、議論とドキュメンテーションのための単一の参照ポイントを提供するために、それに結びつく issue を持たなければなりません。
数分間時間を取って、既存の issue リストに目を通し、あなたが寄稿しようとしている問題に合致するものが無いかどうか調べてください。
もし issue リストにすでに挙っているのを見つけた場合は、その issue にコメントを残して、あなたがその項目について取り組もうとしていることを示してください。
あなたが取り組もうとしている問題に合致する既存の issue が見つからなかった場合は、新しい issue を作成してください。単純な修正であれば、直接にプルリクエストをしてください。
こうすることで、開発チームはあなたの提案をレビューし、将来にわたって適切なフィードバックを提供することが可能になります。

> 小さな変更や、ドキュメンテーションの問題、または単純な修正については、issue を作成する必要はありません。それらについては、プルリクエストだけで十分です。

### 4. メインの Yii ブランチから最新のコードをフェッチする

```
git fetch upstream
```

すべての新しい寄稿について、最新のコードに対して作業することを確実にするために、この作業から開始しなければなりません。

### 5. 現在の Yii のマスターブランチに基いて、あなたの寄稿のための新しいブランチを作成する

> これは非常に重要です。なぜなら、master ブランチを使うと、あなたのアカウントからは一つ以上のプルリクエストを送信することが出来なくなるからです。

独立したバグ修正や変更は、各々、それ自身のブランチに入れなければなりません。
ブランチの名前は内容を説明するものであり、あなたのコードが関係する issue の番号で始まるものでなければなりません。
特定の issue を修正するものでない場合は、番号を省略してください。
例えば:

```
git checkout upstream/master
git checkout -b 999-name-of-your-branch-goes-here
```

### 6. 魔法を使ってあなたのコードを書く

動くことを確認してくださいね :)

ユニットテストは常に歓迎されます。テストされ、十分にカバーされたコードは、あなたの寄稿をチェックするタスクを非常に単純化してくれます。
ユニットテストの失敗を中身とする issue を立てることも許容されています。

### 7. CHANGELOG を更新する

CHANGELOG ファイルを編集して、あなたの修正を追加します。
新しい行は、ファイル冒頭の "Work in progress" 見出しの下に挿入しなければなりません。
チェンジログの行は、下記のどちらかのように書かなければなりません:

```
Bug #999: バグ修正の内容説明 (あなたの名前)
Enh #999: 機能拡張の内容説明 (あなたの名前)
```

`#999` は `Bug` または `Enh` が示している issue 番号です。
チェンジログはタイプ(`Bug`, `Enh`)によってグループ化され、issue 番号順に並ばなければなりません。

非常に小さな修正、すなわち、タイポやドキュメンテーションの修正については、CHANGELOG を更新する必要はありません。

### 8. 修正をコミットする

以下のコマンドを使って、コミットしたいファイルや変更を [staging area](http://gitref.org/basic/#add) に追加します:

```
git add path/to/my/file.php
```

`-p` オプションを使って、コミットに含める変更を選択することも出来ます。

説明的なコミットメッセージをいれて修正をコミットしてください。
github があなたのコミットを自動的にチケットとリンクするように、`#XXX` という書式でチケット番号に言及することを忘れないでください:

```
git commit -m "A brief description of this change which fixes #42"
```

### 9. 最新の Yii コードを upstream からあなたのブランチに pull する

```
git pull upstream master
```

このステップは、プルリクエストを出す前にあなたのブランチが最新のコードを持っていることを確実にするためのものです。
もし何かマージ衝突がある場合は、ここでそれを修正してから再度変更をコミットしなければなりません。
こうすると、Yii 開発チームがワンクリックであなたの変更をマージすることが確実に出来るようになります。

### 10. 衝突をすべて解消したら、あなたのコードを github にプッシュする

```
git push -u origin 999-name-of-your-branch-goes-here
```

`-u` パラメータによって、あなたのブランチが今後は自動的に github のブランチに対してプッシュおよびプルを行うようになります。
つまり、次回 `git push` とタイプしたときには、git は既にどこにプッシュすればよいか知っている、ということです。

### 11. upstream に対してプルリクエスト ( [pull request](http://help.github.com/send-pull-requests/)) を発行する

github 上のあなたのリポジトリに入って、"Pull Request" をクリックし、右側にあるブランチを選び、コメントボックスにもう少し詳細を記述します。
プルリクエストを issue とリンクさせるために、プルコメントのどこかに `#999` という書式で issue 番号を記載します。

> 各プルリクエストは単一の issue を修正すべきものであることに注意してください。

### 12. 誰かがあなたのコードをレビューする

誰かがあなたのコードをレビューします。あなたは修正を求められるかも知れません。その場合は、ステップ #6 に戻ってください 
(現在のプルリクエストが open である限りは、別の新しいプルリクエストをする必要はありません)。
あなたのコードが受け入れられた場合は、コードはメインブランチにマージされて、次回の Yii のリリースの一部となります。
受け入れられなくても、落胆しないでください。
必要とする機能は人によってさまざまに異なります。
Yii は全ての人に対して全てを提供することは出来ません。
その場合でも、あなたのコードは github 上で公開され続けて、それを必要とする人々が参照することが出来ます。

### 13. クリーンアップする

あなたのコードが受け入れられるか却下されるかした後、あなたが作業してきたブランチをローカルレポジトリおよび `origin` から削除することが出来ます。

```
git checkout master
git branch -D 999-name-of-your-branch-goes-here
git push origin --delete 999-name-of-your-branch-goes-here
```

### 注意:

退行 (regression) を早期に発見するために、github 上の Yii コードベースへのマージは、すべて [Travis CI](http://travis-ci.org) に取り上げられて、自動化されたテストにかけられます。
コアチームとしては、このサービスに過大な負担をかけたくないために、
以下の場合にはマージの説明に [`[ci skip]`](http://about.travis-ci.org/docs/user/how-to-skip-a-build/) が含まれるようにします。
すなわち、プルリクエストが:

* javascript、css または画像ファイルだけに影響する場合
* ドキュメンテーションを更新する場合
* 固定の文字列だけを修正する場合 (例えば、翻訳のアップデート)

がそうです。

このようにすることによって、そもそもテストによってカバーされない変更に対しては、最初から travis がテストランを開始しないようにしています。

### コマンド概要 (上級の寄稿者向け)

```
git clone git@github.com:YOUR-GITHUB-USERNAME/yii2.git
git remote add upstream git://github.com/yiisoft/yii2.git
```

```
git fetch upstream
git checkout upstream/master
git checkout -b 999-name-of-your-branch-goes-here

/* 魔法を使い、必要なら changelog を更新 */

git add path/to/my/file.php
git commit -m "A brief description of this change which fixes #42 goes here"
git pull upstream master
git push -u origin 999-name-of-your-branch-goes-here
```
