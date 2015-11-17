---
layout: post
title: "twitter/twitter.github.com Loading repositories on demand when scrolling #14"
description: ""
category: 
tags: ["Open source organizations"]
---
{% include JB/setup %}

original: https://github.com/twitter/twitter.github.com/issues/14

## Loading repositories on demand when scrolling

スクロール中必要に応じたリポジトリのロード

#### jeroenooms commented on 15 Jan 2014

    It would be really cool if the repo "pages" (100 repos per API call) 
    were loaded as the user scrolls down, instead of loading them all at 
    page load. Give a similar experience as instagram or facebook or whatever.
    
    I would like to use this template for an org that has 1500+ repositories,
    but currently it takes forever to load, and then you hit the github API 
    limit :-) It would be cool to only load the first 100, and then dynamically
    add more when scrolling down the page.


ユーザが下にスクロールして行った時にリポジトリの「ページ」（API呼び出し
当たり100リポジトリ）がロードされたら本当coolになりそうだね、ページがロード
される時に全部読んでしまうのではなくて。

このテンプレートを1500以上のリポジトリを持つある組織に使いたいんだけど、
今は延々とロードにかかってしまうし、github APIの制限にもかかってしまうんだ。
最初の100件だけロードして、その後はページを下にスクロールした時に動的に
もっと追加するとクールになりそうだよ。

#### caniszczyk commented on 15 Jan 2014

    I'm all for contributions :)

僕は貢献のためのすべてだよ(訳不安)


#### jeroenooms commented on 15 Jan 2014

    Are you OK with using jQuery plugins? E.g. http://imakewebthings.com/jquery-waypoints/

jQueryのプラグインを使ってもOKですか？例えば http://imakewebthings.com/jquery-waypoints/


#### caniszczyk commented on 15 Jan 2014

    Yep, it's under the MIT license so we're OK with it:
    https://github.com/imakewebthings/jquery-waypoints/blob/master/licenses.txt

うん、MITライセンス下にあるから、それなら僕たちはOKだよ。
https://github.com/imakewebthings/jquery-waypoints/blob/master/licenses.txt



#### augbog commented on 2 May 2014

    If we decide to do this, how would we handle sorting? As of right now, 
    I believe it sorts all the repos before showing them. If we pull data
    based on scroll, would we want it to resort them everytime?

これをやるって決めるとして、ソートはどうやって制御する？ 今は全てのリポジトリを
表示する前にソートしていると思うんだけど。スクロール時にデータを取るなら、
その度にそれらを再ソートしたくなるはず？



#### mbad0la commented 7 days ago

    @augbog @caniszczyk @jeroenooms Hey guys! I know this thread has been open
    for a really long time but i've recently started to explore github and try
    to contribute. Hence, i came upon this Repo and would like to help in this
    issue. Are you guys still interested in this?
    
    About the issue, dont flame because im really a newbie :P 
    But i went through the code and previous comments in this thread and i think we have two solutions -
    1) Store the repo object and append next 30 or something repo data on
       detecting page end -> Though this method does implement the UI aspect
       of loading info after reaching the end of the page, it defeats the purpose
       of avoiding rate limiting because as mentioned earlier for orgs with a huge
       amount of data, the user might not even go through all of it therefore,
       we have some wasted API calls.
    2) Initiate api call to append next 30 or something repo data on detecting page end
       -> This method implements the UI Element and helps optimisation of api calls but
          in the process we end up trading our hotness sorting functionality for this 
          implementation. But we do have every 30 (or whatever threshold we set) repos
          sorted according to our hotness functionality.
    A small deviation to this method can be to add every iterations' data to the repo list,
    sort the list again and then clear the "#repo" div and append the whole list into it again.
    Though this maintains the hotness sort and limits Api calls along with the UI functionality,
    it has a big flaw, of the user to miss out on some newly added repos if they attain positions
    for which user has to scroll up.
    
    Please let me know what you think about this.
    I'd like to submit a pull-request once we reach a conclusion.

@augbog @caniszczyk @jeroenooms Hey guys! このスレッドは長い間開きっぱなしだけど、
僕は最近githubを歩き回ってて貢献しようとしてる。だからこのレポジトリにきて、
このIssueで助けになりたい。みんなはまだコレに関心はある？

このIssueなんだけど、僕は本当に初心者だから怒らないでね。
でも(ソース)コードとこのスレッドの以前のコメントをざっと読んだんだけど、
２つの解決方法があると思うんですよ。

1). リポジトリオブジェクトをとっておいて、ページの最後で次の30件くらいのリポジトリを追加する。
    -> この方法はページの最後にたどり着いた後で情報をロードするUIのアスペクトを実装するんだけど、
       頻度の制限を避けるという目的に反して、以前言われていた通り大量のデータを持っている組織だと、
       ユーザは全てを見て回らないかもしれないから、僕たちは不要なAPI呼び出しを持つことになる。
2) ページの最後で次の30件くらいのリポジトリを追加するためにAPI呼び出しを初期化する。
    -> この方法はUIのElementを実装して、API呼び出しの最適化をするけど、このプロセスで最終的には
       この実装の代わりに(リポジトリの)ホットさによるソートの機能を諦めることになる。
       でも僕らは(リポジトリの)ホットさ（によるソート）の機能をによってソートされた30件（あるいは
       僕らが設定した件数）毎のリポジトリを取得することはできる。


## 背景

twitter/twitter.github.com は twitter社の公開しているリポジトリをまとめている
サイト http://twitter.github.io/ のソースのリポジトリです。

twitter/twitter.github.com is the repository of the web site http://twitter.github.io/
which summarize twitter's public repositories.


このサイトはGitHubの [GitHub Pages](https://pages.github.com/)という機能を使って公開されています。
これはUserやOrganization名と同じ名前のリポジトリを作っておくと、そのリポジトリへpushするとリポジトリの
ファイルを静的なWebページとして公開するものです。あるいはproject siteも作成可能です。

This site is published by using a GitHub function [GitHub Pages](https://pages.github.com/) .
If you make a repository with same name as the user or organization, [GitHub Pages](https://pages.github.com/) 
publish the files in the repository as static web page and its assets. Or you an make it for your project.


## 概要

このissueは、一覧のページのロードに時間がちょっとかかっているから
遅延ロードに変更してサクサク動くといいよねという提案なのですが、
遅延ロードだとソートはどうすんのよ？というツッコミが入り、1年ほど放置されてたら俺やるよ？という人が現れました。

This issue says ""


## 個人的な意見

twitterがこんなにたくさんのOSSプロダクトを作ってるとは知りませんでした。

[GitHub Pages](https://pages.github.com/) はこのサイトでも使っていますが、サーバを用意しなくて済むので非常に助かります。

遅延ロードとソートの話ですが、「遅延ロードでサクサク見せたい」という場合と
「デフォルトとは違った順で見たい」というのは画面は似ているけど、違う機能なんじゃないかなと思います。
前者は初めてサイトを訪れたは人を、後者はその後に更に調べたい人だと思います。
調べたい人にはソートの条件を変更した際に再度最初からページをロードしてもいいんじゃないかな。
