# インストール(mac)
```
brew install mysql
```

# コマンド操作系

## 初期化
```
mysqld --initialize --console
```
rootユーザに対する一時的なパスワードが発行されるので、最初はこのパスワードでログインする。
![](https://storage.googleapis.com/zenn-user-upload/57a6c7bf72ce-20221213.png)

## デーモン起動
```
mysqld
```

## ログイン as root
```
mysql --user root -p -P3306
```

## rootの初期パスワードを「root」に変更
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

## データベース一覧表示
```
show databases
```

## データベース作成
```
create database hogefuga
```

## ユーザ作成
・ユーザ名の書式は`ユーザ名@ホスト名`
・identified by 'パスワード'
```
create user 'crml1206'@'localhost' identified by 'passpasswordword';
```

## ログイン as 作成したユーザ
```
mysql -u/--user crml1206 -h localhost -p
```

## 現在ログイン中のユーザのパスワード変更
```
SET PASSWORD = 'new_password'
```

## 一般ユーザにSUPERUSER権限付与
```
UPDATE mysql.user SET Super_priv='Y' WHERE user='hoge';
```

# SQL系

## CREATE TABLE
```

```

## INSERT
・値の文字列はシングルクオーテーションで括ること
```sql
INSERT INTO user VALUES
(
    1,
    'Anthony',
    20,
    'male',
    '1990-01-01',
    '2022-04-01 12:34:56',
    '2022-04-01 12:34:56',
    0
);
```

## カラム名変更
```
ALTER TABLE <テーブル名> CHANGE COLUMN <変更前> <変更後> <データ型>;
```

# データ型

## 文字型
CHAR(N), VARCHAR(N)

・最大文字数を指定する必要がある。

## 整数型
TINYINT【8bit / 2^8】<br>
SMALLINT【16bit / 2^16】<br>
MEDIUMINT【32bit / 2^32】<br>
INT【64bit / 2^64】<br>
BIGINT【128bit / 2^128】

「8bitもいらん。1bitだけでいい」というときには`TINYINT(1)`と指定できる。<br>
ちなみに`TINYINT(1)`は後述する`BOOLEAN`と同義。

## 日付型
DATE, TIME, DATETIME, TIMESTAMP, YEAR

DATE【フォーマット ： 'YYYY-MM-DD'】<br>
TIME【フォーマット ： 'HH:MM:SS[.fraction]' 】<br>
DATETIME【フォーマット ： 'YYYY-MM-DD HH:MM:SS[.fraction]'】※タイムゾーンの影響なし<br>
TIMESTAMP【フォーマット ： 'YYYY-MM-DD HH:MM:SS[.fraction]'】※タイムゾーンの影響あり

TIMESTAMPは「2038年問題」というものがあるっぽいので、DATETIMEを使うのが無難そう。
https://pisuke-code.com/mysql-way-to-avoid-2038-promblem/

## 真理値
BOOLEAN<br>
・「true/false」「1/0」で表現される。<br>
・内部的には「tinyint(1)」として扱われ、初期値はNULL<br>
　＝ **MySQLでBOOLEAN型とTINYINT(1)は等価**<br>
　＝ TINYINT(1)の(1)は1bitのこと

## 列挙型
ENUM

・文字列定数のリストを定義できる。
```sql
CREATE TABLE USER (
  name   VARCHAR,
  age    TINYINT,
  gender ENUM('male','female','other')
);
```
・リストに定義されていない値を格納すると、SQLモードがstrictモードの場合はエラーとなる。<br>
・なるべくエラーにならないように処理を続ける方針に沿うなら、DB側ではENUMではなくただのVARCHARにして、アプリ側でENUMもつだけでいいかも。

https://qiita.com/park-jh/items/32b21d7b8d24b0ab3dba

# 制約

## PK制約(NOTNULL + UNIQUE)
・1つのPKを指定
```sql
CREATE TABLE USER (
  id     BIGINT   PRIMARY KEY,
);
```
・複数のPK指定
```sql
CREATE TABLE USER (
  id        BIGINT,
  group_id  BIGINT,
  PRIMARY KEY(id, group_id) 
);
```
・制約に名前をつける
```sql
CREATE TABLE USER (
  id        BIGINT,
  group_id  BIGINT,
  CONSTRAINT pk_user PRIMARY KEY(id, group_id) 
);
```

`CONSTRAINT ~`で明示的に制約名を指定しなければ自動で名前がつけられるが、**実際に開発する上ではきちんと`CONSTRAINT ~`で名前をつけるべき**。

## UNIQUE制約
・NULL格納可能。<br>
・**UNIQUEをつけたカラムには自動でインデックスが作成される**。<br>
・PKと同じつけ方

## NOT NULL制約
・PK, UNIQUEと同じつけ方

## FK制約
```sql
CREATE TABLE "entries" (
  "id" INT PRIMARY KEY,
  "account_id" INT NOT NULL,
  "amount" INT NOT NULL,
  "created_at" DATETIME NOT NULL DEFAULT (CURRENT_DATE),
  FOREIGN KEY fk_entries_id (account_id) REFERENCES accounts(id)
);
```


## INDEX制約


# CSVファイルインポート
```
load data local infile "---.csv" into table <table_name> fields terminated by ',' optionally enclosed by '"';
```
## 日本語を含むCSVファイルの場合
mysqlクライアントにログイン後、下記コマンドを実行する。
```
set character_set_database=sjis;
```

# テーブル定義参照コマンド
```
mysql> SHOW FULL COLUMNS FROM doctor_info;
+-------------------------+--------------+-------------+------+-----+---------+-------+---------------------------------+---------+
| Field                   | Type         | Collation   | Null | Key | Default | Extra | Privileges                      | Comment |
+-------------------------+--------------+-------------+------+-----+---------+-------+---------------------------------+---------+
| prefecture_code         | char(2)      | utf8mb4_bin | NO   | PRI | NULL    |       | select,insert,update,references |         |
| doctor_genre            | char(1)      | utf8mb4_bin | NO   |     | NULL    |       | select,insert,update,references |         |
| doctor_id               | char(5)      | utf8mb4_bin | NO   | PRI | NULL    |       | select,insert,update,references |         |
+-------------------------+--------------+-------------+------+-----+---------+-------+---------------------------------+---------+
```

# 日本語でソートする [参考](https://www.karakaram.com/changing-the-character-set-to-utf8mb4-after-creating-mysql-table/)

## 日本語でソートしたいカラムのCOLLATIONを変更する。
```
ALTER TABLE <テーブル名> MODIFY <ソート対象のカラム名> VARCHAR(33) CHARACTER SET cp932 COLLATE cp932_japanese_ci;
```

```
// 文字コードを「CP932」
CHARACTER SET cp932
// 文字コードに合わせてCOLLATEも「CP932」のやつにする。
COLLATE cp932_japanese_ci;
```
## COLLATEとは
> Collation is a set of rules that tell database engine how to compare and sort the character data

Collation =「ソートするためのルール」。<br>
MySQL側でこのルール(Table Collation)が用意されているので、その中から選択すればおk。
```
mysql> SELECT COLLATION_NAME,CHARACTER_SET_NAME
    ->     FROM INFORMATION_SCHEMA.COLLATION_CHARACTER_SET_APPLICABILITY;
+----------------------------+--------------------+
| COLLATION_NAME             | CHARACTER_SET_NAME |
+----------------------------+--------------------+
| big5_chinese_ci            | big5               |
| big5_bin                   | big5               |
| dec8_swedish_ci            | dec8               |
| dec8_bin                   | dec8               |
| cp850_general_ci           | cp850              |
| cp850_bin                  | cp850              |
/---------------長いので割愛-----------------------/
```

各テーブルごとにCollationが設定されており、下記SQLで確認できる。
```
SELECT TABLE_NAME,TABLE_COLLATION
  FROM INFORMATION_SCHEMA.TABLES
 WHERE TABLE_SCHEMA='databse_name';
```


# DB全体の文字コード設定
```
mysql> show variables like "chara%";
+--------------------------+---------------------------------------------------------------+
| Variable_name            | Value                                                         |
+--------------------------+---------------------------------------------------------------+
| character_set_client     | cp932 【クライアント側で発行したsql文はこの文字コードになる】
| character_set_connection | cp932 【クライアントから受け取った文字をこの文字コードへ変換する】
| character_set_database   | utf8mb4 【現在参照しているDBの文字コード】
| character_set_filesystem | binary                                                        |
| character_set_results    | cp932  【クライアントへ送信する検索結果はこの文字コードになる】
| character_set_server     | utf8 【DB作成時のデフォルトの文字コード】
| character_set_system     | utf8 【システムの使用する文字セットで常にutf8が使用されている】
| character_sets_dir       | /rdsdbbin/oscar-5.7.mysql_aurora.2.10.2.0.4.0/share/charsets/ |
+--------------------------+---------------------------------------------------------------+
```