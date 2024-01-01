# go-intermediate
「ブログサービスのバックエンドとなる API」を提供するGoベースのWebアプリです。
[電子書籍「APIを作りながら進むGo中級者への道」](https://techbookfest.org/product/jXDAEU1dR53kbZkgtDm9zx?productVariantID=dvjtgpjw8VDTXNqKaanTVi)を実際に動かしてみることを主題とするものです。
なお、手元で簡易に動作を試してみることを目的とするので、「第12章 ユーザー認証」に関わるコードは本リポジトリ上では削除しております。

## サポートするサービス
### 主な機能
- ブログ記事を投稿する
- ブログ記事に「いいね」をつける
- ブログ記事にコメントを残す

### ブログ API の仕様
今回作るブログ API には、以下のような機能を搭載する。
- ブログ記事を投稿するリクエストを受ける
- 投稿一覧ページに表示させるデータを要求するリクエストを受ける
- ある投稿のデータを要求するリクエストを受ける
- ある投稿に「いいね」をつけるリクエストを受ける
- ある投稿に対してコメントをするリクエストを受ける

## 環境準備

### (1) .envファイルを配置する

	ROOTUSER=root
	ROOTPASS=xxxx
	DATABASE=sampledb
	USERNAME=docker
	USERPASS=XXXX

### (2) コンテナをビルドする

	% docker-compose build sample-app

### (3) コンテナを起動する

	% docker-compose up -d

### (4) DBスキーマを登録する

	% docker exec -it sample-app bash
	# mysql -h db-for-go -udocker -p sampledb < repositories/testdata/setupDB.sql
	Enter password: XXXX

### (5) プログラムを起動する

	# go run main.go


## 実際に使ってみる

### (1) ブログ記事を投稿する

	curl http://localhost:8080/article -X POST \
	-H "Content-Type: application/json" \
	-d @- << EOF | jq .
	{
		"title": "first post",
		"user_name": "ttsubo",
		"contents": "This is my first blog."
	}
	EOF

### (2) ブログ記事一覧を取得する

	curl http://localhost:8080/article/list -X GET | jq .

### (3) 投稿したブログ記事を参照する

	curl http://localhost:8080/article/3 -X GET | jq .

### (4) ブログ記事に「いいね」をつける

	curl http://localhost:8080/article/nice -X POST \
	-H "Content-Type: application/json" \
	-d @- << EOF | jq .
	{
		"article_id": 3
	}
	EOF

### (5) 先ほど投稿したブログ記事に、コメントする

	curl http://localhost:8080/comment -X POST \
	-H "Content-Type: application/json" \
	-d @- << EOF | jq .
	{
		"article_id": 3,
		"message": "nice article!"
	}
	EOF

### (6) 再度、投稿したブログ記事を参照する

	curl http://localhost:8080/article/3 -X GET | jq .
