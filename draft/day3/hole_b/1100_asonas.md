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


## RailsはRESTful

DHHが7年前にRailsはRESTfulであると言っていた。

resources :users

と書くことによってCRUDに必要なものができてくる


## Railsを使うこと

Railsを使う時点で私達が重要視しなければならないことは、その設計がRESTfulであるか？ということだと思います。
その中でパターンにできるほど良く使うものを紹介されていました。

## RESTfulにするメリット

RESTfulな設計にすることでどういうメリットがあるか？

* 一貫性がとれる
* 簡単
* HTTPのメリットに乗れる

ということが一般的に言える。
それはRailsでも同じで、同じことを開発者に強いるので理解がしやすい。誰が設計をしても同じようになる。
Railsにうまく乗っかることができればRailsエンジニアはリソースの名前を決めるだけでいいことになる。


2006年に発表していからそんなに単純でよいのか？と言われていたが、今日でもRailsはresouces を採用していることから、resourcesはよいものということ。


同じくDHHは「制約が自由をもたらす」と言っていた。とてもいい言葉だが、その制約でもやはり難しいものがある。
それは

認証
検索
状態経か
手続き的
リスト
ウィザード系の画面

こういうのをどうやってresources で表現するのか？

振り返り、resourcesはパターンと言えて設計のしやすさと結びつく
コレを基本のパターンとして、もっと小さなものや大きなもののパターンを実装するとすればもっと簡単になるのではないか？

また、resourcesというパターンを乗っているか
特定のパターンを実装しているといえるならば、

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

