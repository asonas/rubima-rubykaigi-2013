# High Parformance Rails

RubyKaigi 2013 2日目のホールA。トップバッターは
日本のキッチンを支えるCookpadのインフラやパフォーマンスの改善を担っている成田(@mirakui)さん。
その成田さんの経験から語られるRailsアプリケーションのパフォーマンスについての発表でした。

他の言語・フレームワークの組み合わせと比べ、決して高速とはいえないRailsですが
それでもCookpadではRailsを採用し、サービスを運用しています。
開発のしやすさとのトレードオフなので、
このパフォーマンスを問題にRailsで開発することを諦めてはいけないと言っていました。

Railsのアプリケーションを高速化させるためにいくつかのコツを教えてもらいました。

まずは極力Rubyに処理をさせないこと
次にRubyは極力新しいバージョンを使うこと


また、経験上特にRailsが遅いところはActiveRecordのオブジェクトを生成するところとRoutingの解決であると言います。
特にRoutingの話は会場を沸かせていました。

Cookpadのように大規模なWebアプリケーションになるとroutesが大きくなり、ルーティングのコストが上がると言います。

私も気になったので、測ってみたところ以下の順番で実行速度が速かったです。

```
# config/routes.rb
resouces :users

url_for("/users")
url_for(users_path)
url_for(:users)
url_for(controller: 'users', action: 'index')
```

パフォーマンスに配慮したよいアプリケーションを作って行きたいですね!

他にTemplate Engine(ERBやHamlなど)の話や、GCを止める話など
多くのRailsアプリケーションを作る人の役に立ちそうな話が目白押しでした。

ぜひRailsアプリケーションを作っている人はスライドや発表動画を御覧ください!
※スライドは発表当時より内容が増えているので、発表を聞いた方もぜひ御覧ください!



------------------
memo



## Rails高速化をなぞった形

- 1003 models
- 236 controllers
- 2871 view templates
- 1978 lines

## ProductionLogの時間とX-Runtime

## 開発のしやすさとのトレードオフ


## 早くする大前提
### Rubyに処理をさせないこと

Nginx => Unicorn

## Rubyのバージョンによってレスポンスタイムが違う
- REE 1.8.7
- REE 1.9.3 100ms down
- REE 2.0.0

## Railsのボトルネック
SQL - Ruby (80%)

## Railsの遅いところ
- ARのオブジェクト発生
- Routing

## hello_index_urlと{controller: hello, action: index}では後者のほうがかなり遅い

## Template Engine
- コンパイル時間は重要じゃない
- コンパイルしたソースの速度が重要だ
 - ERB, Haml, SlimではHamlが一番遅い
 - CookpadではHamlを使っている
 - ここの時間は些細なもので、ARのほうが大きい

## UnicornのGCをとめよう


## Slow Query
- Arploxy => fluentd => 自社ツールで測定

## Trade-off

Developers <=> Users, Servers


## ARオブジェクトを作らなくするには
- AR#pluckで特定のデータをひっぱる

## ActiveSupport::Benchmark
