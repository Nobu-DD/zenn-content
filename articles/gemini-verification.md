---
title: "RailsでGeminiAPIを実装してみた(超初学者向け)"
emoji: "💻"
type: "tech"
topics: ["ruby", "rails", "gemini", "生成ai"]
published: false
---
## この記事の目的
こちらはスクールの個人開発にてGeminiAPIを使用した生成AI機能を実装した内容をまとめてみた記事になります。
今回は以下の記事を参考にして実装＆記事作成を行いました。今回私が書く記事よりもわかりやすくまとめられているので、実装するのが目的であれば先にこちらを見ていただいた方がいいかと思います。
https://zenn.dev/nir_nmttg/articles/464519457cf818
https://qiita.com/mmaumtjgj/items/1e2a9d5976371e6867af
この記事は実装していてわからない概念や技術を深堀りするために作成しました。解釈が間違っている箇所があれば指摘いただけると幸いです。
## 対象者
- RailsでGeminiAPIを使用したい方
- APIとのやり取りの仕組みを簡単に知りたい方
## 前提条件
- フロントエンド
   - Hotwire(Stimulus)使用
- バックエンド
   - Ruby on Rails 7.0
## OpenAPIとGeminiAPIでの実装の違い
まずOpenAPIを実装しようとした場合、gem`ruby-openai`を使用することで、OpenAPIとのリクエスト・レスポンスを簡単にやり取りすることが出来ます。
しかしGeminiAPIの場合、専用のgemが存在していないそうです。(気付いてないだけでもしかしたら存在してるかも)
なのでGeminiAPIにリクエストする処理、レスポンスを受け取る処理を自前で作成していく必要があります。
## 実装
では早速実装の手順を書いていきます。
流れとしては以下の通りになります。
1. GeminiServiceを定義
2. 対象のコントローラーにAPIの呼び出しを実行するメソッドを追加
3. クライアントが操作するAIボタンをviewに記述
4. stimulusを使用したJSでの処理
### GeminiAPIとやり取りするサービスクラスを定義