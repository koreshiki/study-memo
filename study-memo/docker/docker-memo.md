# Dockerメモ

## 基本操作

### コンテナを起動する
* docker run イメージ名
	実際には次のコマンドがまとめて実行されている
	* dockder pull 	: イメージの取得
	* docker create	: コンテナの作成
	* docker start	: コンテナの起動

### コンテナ内で呼び出すコマンドを指定して起動する
* docker run イメージ名 コマンド

### ダウンロード済みのイメージ一覧を取得する
* docker images
	REPOSITORY	: イメージのリポジトリ名
	TAG			: タグ、明示的に指定しなければlatestとなる
	IMAGE ID	: イメージを一意に識別するID

### イメージの詳細を確認する
* docker inspect REPOSITORY:TAG
	MySQLなど起動したときのIPを確認できる

### イメージを削除する
* docker rmi REPOSITORY:TAG
* docker rmi IMAGE_ID
	イメージを使用したコンテナが存在する場合は先にコンテナを削除する必要がある
	もしくは-fフラグで強制的に削除することもできる

### コンテナを表示する
* docker ps (-a)
	CONTAINER ID	: コンテナを一意に識別するID
	IMAGE			: 元となったイメージ名
	STATUS			: 現在のステータス
	NAMES			: コンテナ名
	-aオプションをつけると停止しているコンテナも表示される

### コンテナを削除する
* docker rm NAMES
* docker rm CONTAINER_ID

## Dockerfile関連

### Dockerfileによるビルド
* docker build -t イメージ名:タグ名 .
	デフォルトではビルドコンテキスト上に置いてあるDockerfileという名前のっファイルが使用される
	--fileフラグで任意のファイルを指定することができる
	**ビルドコンテキスト上に存在しないファイルはDockerイメージに取り込むことができない**

## Docker Hub関連

### ログイン
* docker login
	ユーザー名とパスワードを要求されるので入力、成功するとLogin Suceededと表示される

### タグ付け
* docker tag IMAGE_ID イメージ名:タグ名
	タグ名を省略した場合はlatestが使用される

### レジストリへイメージをpush
* docker push イメージ名:タグ名
	イメージ名はDocker_ID/イメージ名という形式をとること
