# Dockerメモ

## 基本操作

### コンテナを起動する
* docker run イメージ名
	実際には次のコマンドがまとめて実行されている
	* dockder pull 	: イメージの取得
	* docker create	: コンテナの作成
	* docker start	: コンテナの起動

### コンテナに入る

* docker exec -i t コンテナID/コンテナ名 bash
* docker attach  コンテナID/コンテナ名

docker attachはPID 1で動作するためexitするとそのコンテナも終了してしまう

これを避けるためにはCtrl+p+qでコンテナを抜ければよい

### コンテナ上のファイルをローカルに持ってくる

* docker cp コンテナID:<コンテナ上のパス> <ローカルのパス>（逆すなわちローカルからコンテナにコピーすることも可能）

  ※Docker for Windowsの場合は、管理者としてPower shellを実行すること



### ポートフォワーディング

* docker run -p ホストのポート:コンテナのポート イメージ名

	

例えばnginxなどで使う

docker run -p 8080:80 nginx

### dettachedモードで起動する
* docker run -d イメージ名

	

nginxのようにコンテナ上で常に動作し続けるプログラムの場合は-dでdettachedモードで起動したほうが良い


### 仮想マシンのIPアドレスを調べる（Docker Toolbox）
* docker-machine ip default
* docker-machine ip <Dockerホスト名>

### ネットワークの一覧を表示する

* docker network ls

### ネットワークの詳細を表示する

* docker network inspect <ネットワーク名>

### ユーザ定義のブリッジネットワークを作成する

* docker network create <ネットワーク名>

### 作成したネットワークにコンテナを接続する

* docker network connect <ネットワーク名> <コンテナ名>

### ネットワークを切断する

* docker network disconnect <ネットワーク名> <コンテナ名>

### コンテナ内で呼び出すコマンドを指定して起動する

* docker run イメージ名 コマンド

### ダウンロード済みのイメージ一覧を取得する
* docker images
	* REPOSITORY	: イメージのリポジトリ名
	* TAG			: タグ、明示的に指定しなければlatestとなる
	* IMAGE ID	: イメージを一意に識別するID

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
	* CONTAINER ID	: コンテナを一意に識別するID
	* IMAGE			: 元となったイメージ名
	* STATUS			: 現在のステータス
	* NAMES			: コンテナ名
	
	-aオプションをつけると停止しているコンテナも表示される

### コンテナを削除する
* docker rm NAMES
* docker rm CONTAINER_ID

## Dockerfile関連

### Dockerfileによるビルド
* docker build -t イメージ名:タグ名 .

	デフォルトではビルドコンテキスト上に置いてあるDockerfileという名前のファイルが使用される

	--fileフラグで任意のファイルを指定することができる

	**ビルドコンテキスト上に存在しないファイルはDockerイメージに取り込むことができない**

## Docker Machine

### Dockerホストを確認する

* docker-machine ls

現在ActiveとなっているホストはACTIVE列に\*が表示される

### Dockerホストを作成する

* docker-machine create <ホスト名>

### Dockerホストの環境変数を確認する

* docker-machine env <ホスト名>

最後の行に表示されるevalコマンドを実行するとそのホストをActiveにすることができる

### ActiveになっているDockerホストをNonActiveにする

* docker-machine env --unset

正確にはこのコマンド実行後に最後の行に表示されるenvコマンドを実行する

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
