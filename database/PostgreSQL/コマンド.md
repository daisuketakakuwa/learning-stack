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

# ID自動生成
- `SERIAL`, `BIGSERIAL` をつけると、自動的にシーケンスを作成して、そのシーケンスから値を生成してくれる。
- SERIALは、32ビット整数の範囲内でシーケンスを生成します -> 参照する側は`INTERGER`
- BIGSERIALは、64ビット整数の範囲内でシーケンスを生成します -> 参照する側は`BIGINT`

```sql
-- organizerテーブル
CREATE TABLE organizer (
    id SERIAL PRIMARY KEY,
    organization_name VARCHAR(300)
);

-- eventテーブル
CREATE TABLE event (
    id BIGSERIAL PRIMARY KEY,
    organizer_id INTEGER REFERENCES organizer(id),
    event_name VARCHAR(300),
    starts_at TIMESTAMP,
    ends_at TIMESTAMP
);
```
