# MySQL ローカル開発環境

Docker Compose で MySQL 8.3.0 をローカルに起動するための環境です。

## 必要なもの

- Docker
- Docker Compose v2(`docker compose` コマンド)

> 旧来の `docker-compose`(v1)を使う場合は、以下のコマンドの `docker compose` を `docker-compose` に読み替えてください。

## 接続情報

| 項目 | 値 |
| --- | --- |
| ホスト | `localhost` |
| ポート | `3306` |
| データベース | `mysql` |
| ユーザー | `mysql` |
| パスワード | `mysql` |
| root パスワード | `root` |
| コンテナ名 | `mysql_test` |
| タイムゾーン | `Asia/Tokyo` |
| 文字コード | `utf8mb4`(照合順序 `utf8mb4_unicode_ci`) |

> これらの認証情報はローカル開発専用です。共有環境や本番環境では使用しないでください。

## 使い方

### 起動(バックグラウンド)

```bash
docker compose up -d
```

### 停止

```bash
docker compose stop
```

### 停止したコンテナの再開

```bash
docker compose start
```

### MySQL クライアントで接続

```bash
docker compose exec db mysql -u mysql -p mysql
```

パスワードを聞かれたら `mysql` を入力します。root で接続する場合は `-u root` を指定してください。

### ログの確認

```bash
docker compose logs -f db
```

### 削除(データも含めて完全に削除)

```bash
docker compose down --volumes
```

> `--volumes` を付けると名前付きボリューム `db-data` も削除され、データベースの内容はすべて失われます。コンテナだけを削除してデータを残したい場合は `--volumes` を外してください。

## 設定の変更

MySQL の設定は `conf.d/my.cnf` に記述します。このディレクトリはコンテナ内の `/etc/mysql/conf.d` にマウントされているため、変更後に再起動すれば反映されます。

```bash
docker compose restart db
```

なお、`MYSQL_DATABASE` や `MYSQL_USER` などの環境変数は**初回起動時(ボリュームが空のとき)のみ**有効です。変更を反映するには `docker compose down --volumes` でデータを削除してから起動し直してください。

## ファイル構成

```
.
├── compose.yaml      # Docker Compose 定義
├── conf.d/
│   └── my.cnf        # MySQL 設定(文字コードなど)
├── .dockerignore
├── .gitignore
└── README.md
```
