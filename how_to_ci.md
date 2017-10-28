# CircleCIを使ってのTeXのPDF生成
ついでにmaster branchの場合はSlackに投げるところまでやってみる

## 目的
せっかくGitHub使って研究の論文等を整理するなら，
無料で使えるCIのサービスを使っていつでもPDFにしたものが見れるほうが確認しやすくて便利なのではと思ったのでやってみた次第．


## やること
### GitHubにリポジトリを立てて論文を入れる
当たり前だよなぁ？

### CircleCIに登録
[CircleCI](https://circleci.com/)にアクセスしてGitHubアカウントでSign Up

ProjectsのタブでAdd ProjectをしてビルドしたいリポジトリをCircleCIに追加

恐らくCircleCI側の設定はこれで終わり．開きっぱなしにしておいてGitHub側のリポジトリの用意をする．

よく分からなかったらググって下さい

### リポジトリにciの設定を用意する
リポジトリディレクトリの直下に.circleci/config.ymlを作る．中身は以下をよしなに変更
```.circleci/config.yml
version: 2
jobs:
    build:
      docker:
        - image: kalium40/ubuntu-ja-latex:latest
      steps:
        - checkout
        - run: uplatex main.tex & uplatex main.tex & uplatex main.tex
        - run: dvipdfmx main.dvi
        - store_artifacts:
            path: main.pdf
            destination: paper.pdf

    deploy:
      docker:
        - image: kalium40/ubuntu-ja-latex:latest
      steps:
        - checkout
        - run: uplatex main.tex & uplatex main.tex & uplatex main.tex
        - run: dvipdfmx main.dvi
        - run: curl -F file=@main.pdf -F channels=#devnull -F token=${SLACK_BOT_API_TOKEN} https://slack.com/api/files.upload

workflows:
  version: 2
  build-n-deploy:
    jobs:
      - build:
          filters:
            branches:
              ignore: master
      - deploy:
          filters:
            branches:
              only: master

```

masterの場合のみdeployが，それ以外はbuildの方が行われるようにした．

### Slack用Tokenの設定
CircleCIがSlackに投稿できるように，Slack BotのACCESS TOKENを設定する．
ProjectのSettingsを開き，Environment Variablesで"SLACK_BOT_API_TOKEN"の名前でTokenを追加．


### 動作確認
.circleci/config.ymlを追加してCommit,GitHubにPush

これでBuildsになんか増えて，上手く行ってればSuccessになるはずです．

![Success Image](/images/build_list.png)


