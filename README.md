# 2023Portofolio-readme

<br>

## [https://d2lv2q5x40zv09.cloudfront.net/](https://d2lv2q5x40zv09.cloudfront.net/)

<br>

### frontend: https://github.com/Takuya-Ishida-Axhiz/202305Portofolio-frontend

### backend: https://github.com/Takuya-Ishida-Axhiz/202305Portofolio-backend

<br>
<br>

# About

本ポートフォリオは、Web 開発 3 年目のわたくしによるポートフォリオです。

制作目的は以下です。

- 直近半年間で学んだことをアウトプットし自身の知識として定着させること
- 自身のエンジニアとしての価値を評価してもらう材料にすること

<br>

そのため本 ReadMe には、備忘録的に使用した技術の説明とポイントの解説を入れるようにしています。

<br>
<br>

_「フロントエンドが得意で、より無駄のない実装ができそう」_

_「バックエンド・インフラの構築もよくある構成ならできそう」_

_「ユーザー視点でどうすれば使いやすいかを常に考えながら動いてくれそう」_

_「ビジネスサイドのコスト・スピード感も考えながら提案・実装してくれそう」_

<br>
<br>

こんな感じの印象付けができたなら万々歳です。

<br>

ちなみに、

良さげな題材が思いつかなかったので先日大学時代の OB・OG で行ったライブの URL を CRUD できる管理画面のようなものを作ってみました。

私はオープニングアクトとキタニタツヤをやらせてもらいました。楽しかったです。

<br>
<br>

では、いってみましょう。

<br>
<br>
<br>

# Fundamental Technology

<br>

- Frontend
  - react
  - recoil
  - antd
  - dataloader
  - vite
- Backend
  - NestJS
  - DynamoEasy
  - Serverless Framework
- Infra
  - S3
  - CloudFront
  - Lambda
  - DynamoDB
  - APIGateway
  - CloudFormation

<br>
<br>
<br>

# Point

<br>

## recoil × dataloader でリクエストを最適化し無駄のない APICall を行う

<br>

一般的なリストを DB から取得する API を作る上で、以下の点に注意する必要があります。
<br>

- DynamoDB を使う場合、データ読み込み時に都度 Scan をしていないか
- レコードが多い場合、一件ずつ取得してしまっていないか
  <br>

DynamoDB では Scan はテーブル内のすべてのデータを取得するので、

一部編集した後一覧画面に戻った場合などでも都度 Scan を叩くと読み込みキャパシティを消費しすぎてしまい、

処理速度的にも利用料金的にもよろしくありません。

よって、recoil × dataloader を使い、`ids: array[]` を引数に BatchGet するように組むことで、引数の ids の中で変更があったもののみを変更をかけています。

これは PK を起点とした Query になるので読み込みキャパシティを無駄にしません。

また、id の数だけリクエストを送りつけることなく、dataloader がひとつのリクエストとしてとりまとめてくれるため APICall にも無駄がありません。

<br>
<br>
<br>

## antd でより高速に一般的に求められる綺麗な UI を提供

antd は UI ライブラリのひとつで、簡単に一定基準の品質のテーブルやモーダルなどを提供できます。

※デザインは綺麗で好きなのですが、テーブルの扱いなどに少しクセがある気がするのと、あまり公式ドキュメントが親切でないところがあります。（今勤めている会社で標準技術なので使っています）

個人的には ChakraUI などの方が直感的で好きかもしれません。TailWind や Material UI も触ったことがある程度なので比較してみたいところ。

<br>

UI ライブラリを使った一般的な管理画面を早く作るのは得意なのですが、

そのプロダクトが持つブランド性を決めるようなデザイン・フォント・UI の配置などを追求することにおいては完全に素人なので、今後はそういった方面を伸ばしていきたいと思っています。

<br>
<br>
<br>

## vite でホットリロード&高速デプロイ

vite を使うことでホットリロードや高速デプロイを実現しており、メンバーによりよい開発者体験をもたらし、プロジェクトにスピード感を持たせています。

[https://ja.vitejs.dev/](https://ja.vitejs.dev/)

<br>
<br>
<br>

## DynamoEasy でより簡易に DB とやりとりする

AWSSDK で直接 DynamoDB とやりとりすることもできますが、すこし書きにくいことがあります。

DynamoEasy を使うことで直感的に記述することができます。

https://github.com/shiftcode/dynamo-easy

<br>
<br>
<br>

## S3 + CloudFront でコストパフォーマンスの高いフロントエンドデプロイ

S3 に置いたビルドファイルでフロントエンドをデプロイしているので、より低コストでサービスを運用することができます。
CloudFront も使っているのでより高速に配信可能となります。

<br>
<br>
<br>

## Serverless Framework を使い、Lambda 上に NestJS アプリケーションをデプロイ

ECS、Fargate を用いた NestJS のデプロイが主流かと思いますが、よりミニマムなサービスを想定して Serverless にデプロイをしてみました。

Lambda なので使った分だけの請求になり、スタートアップなどではよりコストを抑えた開発を行うことができます。
