# Internet_TV
step1 

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
