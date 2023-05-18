# Internet_TV
<details><summary>STEP1</summary> 

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | | PRIMARY |  | YES |
| name | varchar(100) |  |  |  |  |

テーブル2：programs

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) |  | PRIMARY |  | YES |
| title | varchar(100) |  |  |  |  |
| description | text | Yes |  |  |  |

テーブル3：genres

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) |  | PRIMARY |  | YES |
| name | varchar(100) |  |  |  |  |

テーブル4：program_genres

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| program_id | bigint(20) | | PRIMARY |  |  |
| genre_id | bigint(20) | | PRIMARY |  |  |

テーブル5：seasons

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | | PRIMARY |  | YES |
| program_id | bigint(20) | | INDEX |  |  |
| number | int(11) | |  |  |  |

テーブル6：episodes

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | | PRIMARY |  | YES |
| season_id | bigint(20) | Yes | INDEX |  |  |
| number | int(11) | Yes |  |  |  |
| title | varchar(100) | |  |  |  |
| description | text | Yes |  |  |  |
| duration | int(11) | |  |  |  |
| release_date | date | |  |  |  |
| view_count | bigint(20) | |  | 0 |  |

テーブル7：broadcasts

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | | PRIMARY |  | YES |
| channel_id | bigint(20) | | INDEX |  |  |
| episode_id | bigint(20) | | INDEX |  |  |
| broadcast_time | datetime | |  |  |  |

# 外部キー制約、ユニークキー制約に関して

- テーブル：program_genres
  - 外部キー制約：program_id に対して、programs テーブルの id カラムから設定
  - 外部キー制約：genre_id に対して、genres テーブルの id カラムから設定
- テーブル：seasons
  - 外部キー制約：program_id に対して、programs テーブルの id カラムから設定
- テーブル：episodes
  - 外部キー制約：season_id に対して、seasons テーブルの id カラムから設定
- テーブル：broadcasts
  - 外部キー制約：channel_id に対して、channels テーブルの id カラムから設定
  - 外部キー制約：episode_id に対して、episodes テーブルの id カラムから設定
</details>

<details><summary>STEP2</summary>
  1.データベースの構築
  　MySQL始動後下記コードにて新規データベースを作成、今回はinternet_TVというデータベースを作成
  ```
  CREATE DATABASE internet_TV;
  ```
  
  2.ステップ1で設計したテーブルを構築
  ・下記コードにて使用するデータベースの選択
  ```
  USE internet_TV;
  ```
  
  ・テーブル構築のSQL文
  ```
  CREATE TABLE channels (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50) NOT NULL
);

CREATE TABLE programs (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(100) NOT NULL,
  detail TEXT,
  program_length INT NOT NULL
);

CREATE TABLE genres (
  id INT AUTO_INCREMENT PRIMARY KEY,
  genre_name VARCHAR(50) NOT NULL
);

CREATE TABLE program_genres (
  id INT AUTO_INCREMENT PRIMARY KEY,
  program_id INT NOT NULL,
  genre_id INT NOT NULL,
  FOREIGN KEY (program_id) REFERENCES programs(id),
  FOREIGN KEY (genre_id) REFERENCES genres(id)
);

CREATE TABLE seasons (
  id INT AUTO_INCREMENT PRIMARY KEY,
  program_id INT NOT NULL,
  season_number INT NOT NULL,
  FOREIGN KEY (program_id) REFERENCES programs(id)
);

CREATE TABLE episodes (
  id INT AUTO_INCREMENT PRIMARY KEY,
  season_id INT NOT NULL,
  episode_number INT,
  title VARCHAR(100) NOT NULL,
  detail TEXT,
  duration INT NOT NULL,
  release_date DATE NOT NULL,
  view_count INT NOT NULL DEFAULT 0,
  FOREIGN KEY (season_id) REFERENCES seasons(id)
);

CREATE TABLE broadcasts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  channel_id INT NOT NULL,
  episode_id INT NOT NULL,
  broadcast_time DATETIME NOT NULL,
  view_count INT NOT NULL DEFAULT 0,
  FOREIGN KEY (channel_id) REFERENCES channels(id),
  FOREIGN KEY (episode_id) REFERENCES episodes(id)
);
```
  
  
  

