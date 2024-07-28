# ログインコマンド
```
psql -p 5433 -U postgres -d postgres

  -h, --host=HOSTNAME
  -p, --port=PORT
  -U, --username=USERNAME
  -w, --no-password        never prompt for password
  -W, --password           force password prompt (should happen automatically)
```

|操作|コマンド|
|----|----|
|テーブル一覧表示|¥d|
|テーブル詳細表示|¥d ${table_name}|
