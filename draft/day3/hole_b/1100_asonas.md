# Rails Gems realize RESTful modeling patterns
http://rubykaigi.org/2013/talk/S78
http://www.ustream.tv/recorded/33609885

## 概要
* RailsがRESTfulにつくれるように設計されています
* さらにそれを実践的に使うために幾つかのパターンと共に紹介
* それぞれのパターンについて、自分の実際の体験を元に書きます


## Rails Gems realize RESTful modeling patterns

多くの人が使っているRailsはRESTfulな設計、開発ができるように開発されています。
そのRailsをよりRESTfulに扱うために7つのパターンを記載しているブログをもとにその中から、いくつか私達がよく使うパターンについて発表されました。

## RailsはRESTfulである

RailsはRESTfulなフレームワークであると、DHHが7年前に言っていました。
RailsでどういうところがRESTfulであるかというと

```
# config/routes.rb
resources :users
```

と1行だけ書くことによってCRUDに必要なものが用意されることは、日頃からRailsでWebアプリケーションを書いている人はすぐにわかると思います。
このCRUDを表にすると以下のようになります。

| - | GET | POST | PUT | DELETE |
| --- |--- |--- | --- | --- |
| /users | index | create  | - | - |
| /users/:id  | show | - | update | destroy |


## RESTfulにするメリット

RESTfulな設計にすることでいくつかのメリットを享受することができます。

* 一貫性がとれる
* 簡単
* HTTPのメリットに乗れる

ということが言えます。
それはRailsでも同じで、同じことを開発者に強いるので理解がしやすく、設計をしても誰がやっても同じような設計ができあがります。
tkawaさんの発表の中で幾つかのRESTfulのパターンを紹介されていましたが、このレポートではその中から「認証」について紹介したいと思います。

認証をするときのURLやリソースはなんでしょうか？例えば、私達がよく認証機能を実装するときに使うDeviseをひとつの例としてみてみると

```
GET    /users/sign_in devise/sessions#new
POST   /users/sign_in devise/sessions#create
DELETE /users/sign_in devise/sessions#destroy
# snip
```

この例からDeviseはsessionに対してnewしたりcreateしていることがわかります。つまりsessionをリソースとしていみなしていて、ここで注目したいのは、実際にはsessionはモデルとしては存在しないし、データベースにも保存をしないようになっていません。

つまり単一のリソースに対してのCRUDなので、Railsで表現するならば

``` 
# config/routes.rb
resource :session
```

と書くことによって

| - | GET | POST | PUT | DELETE |
| --- |--- |--- | --- | --- |
| /session  | show | create | update | destroy |

と、表現できます。なにをリソースとするのかを考えることによって、自然なURLが生成されましたね。

このあともtkawaさんはauthlogickやkaminariといった有名なGemがどういうRESTfulなパターンを持っているのかを紹介していました。

このように、RailsのRESTfulなパターンは他のGemを参考にすることで解決することが多いように思いました。認証の例でもありましたが、リソースの名付けが難しい場合時にはなにをリソースと見なすか？ということに焦点を当てるとRailsにおける開発の基本なのかもしれませんね。

このレポートで紹介しきれなかった他のパターンについては[こちらのブログ](http://rest-pattern.hatenablog.com/)で綴られています。[tkawaさんのスライド](http://www.slideshare.net/tkawa1/rubykaigi2013-rails-gems-realize-restful-modeling-patterns)と合わせて読んでみてはどうでしょう？


## メモ
tkawa

SendagayaRB
http://rest-pattern.hatenablog.com/

リソースが大事だよ

HTTPの動詞
resourcesが便利

RESTの絵mリット
簡単
consisitent一貫性
HTPのメリットを則っている
一般的にはいえる
とりあえけRailsのResourcesにいうと
簡単に設計ができる
理解が簡単


user/1
users/1
users/user/1
users/user-1

汎用的に使いやすいものを選んでいる
ベタープラクティス
開発者が決めることはリソースの名前をきめるだけで住む

開発者達との連携が簡単になる
APIを作っているとそれを使う人がいればそのresourcesをみれば
リソースの名前がわかるということは、コントローラ、モデルがあることが示唆される
共同に開発するとことはかなりだいし

dhhが2006年に提唱して
Constraints are liverating

authentication

search

state changes exectuin og
レスとのれないパタ0んがつらい
resourcesがパターンであってそれがデザインと理解がしやすい
resourcesは基本のパターン
その上に小さなものがあればよい
それにそって設計することが大事


万能っていうわけでもにない
デザインパラターン。コストがかかるかもしれない問題解決を実際に行う会え先行調査として大変役になっつ
パターンに名前がつくｔことで議論ギアしやすい

同じようなパターンを設計することはRaiksに近い

Gemが特定パターンを提供しているんだったら
routesにはそれを書く場所になっているかもしれない
それが理想的な形

いくつかの　パターンを紹介する

認証
ログイン・ログアウト
どうやって表現するか？

## 認証　

ログインのリソースをどうやって表現するのか？
Deviseのを例にして見るとsessionという仮のリソースをを扱っている。
つまり

GET  /session
PUT(POST) /session
DELETE

resource :user
resource :session

リソースがひとつしかない場合にはこのパターンが使える


AuthlogicというGemもまたDBにレコードをもたいないがセッションをリソースとして扱いサインインサインアウトを実現している
認証のための処理がモデルによるのでコントローラがとてもシンプルになる

検索

コントローラに
Query Stringで検索のクエリを渡す
Flilterdコレクションパターン

クエリパターンで渡すようにする

Devise
sessions#create
sessions#destroy
Sessionsというリソースを表現している
モデルとコントローラが一対ではないのが大事
セッションは単数である。
それはセッションがユーザにひとつにしかならない

/session get
/session put, post
/session delete

セッションがリソースと考えるのは難しい

resources :users
resources :user
resources :session

単数のリソースもひとつのパターン

Authlogic
Rails2のころからあるやつのながれをくんでいる

class Session < Authlogic::Session::Base
認証の処理がモデルに寄った
I with Devise wre like that

検索
名前の検索や日付の条件を満たす検索
Ransack

/users?q[name_cont]=tkawa
/users?q[created_at_lt]=2013-01-01

検索のはフィルタリング

kaminari
雷も一種のフィルター
Filtered Collection Pattern

iwish  icloud use rquret

wizard
wicked

リソースのなかに特定の属性を取り出している

Partial Resource Pattern

transactionパターン
transactionを生成する
最後にcommtedすることでリソースが一発で生成される

resourcesはとても大事になっている
ぴったりGemのパターンがあうことはないかもしれないけど
resourcesから外れるのは最後のパターンにする
Gemｗ作る人はどういうふうに使割れるかを考えていると
パターンを実現することになるよ

