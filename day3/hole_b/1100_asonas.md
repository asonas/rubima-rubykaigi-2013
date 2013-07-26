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
