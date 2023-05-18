# Internet_TV
step1 

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| name | varchar(100) | No |  |  |  |

テーブル2：programs

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| title | varchar(100) | No |  |  |  |
| description | text | Yes |  |  |  |

テーブル3：genres

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| name | varchar(100) | No |  |  |  |

テーブル4：program_genres

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| program_id | bigint(20) | No | PRIMARY |  |  |
| genre_id | bigint(20) | No | PRIMARY |  |  |

テーブル5：seasons

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| program_id | bigint(20) | No | INDEX |  |  |
| number | int(11) | No |  |  |  |

テーブル6：episodes

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| season_id | bigint(20) | Yes | INDEX |  |  |
| number | int(11) | Yes |  |  |  |
| title | varchar(100) | No |  |  |  |
| description | text | Yes |  |  |  |
| duration | int(11) | No |  |  |  |
| release_date | date | No |  |  |  |
| view_count | bigint(20) | No |  | 0 |  |

テーブル7：broadcasts

| カラム名 | データ型 | NULL | キー | 初期値 | AUTO INCREMENT |
| --- | --- | --- | --- | --- | --- |
| id | bigint(20) | No | PRIMARY |  | YES |
| channel_id | bigint(20) | No | INDEX |  |  |
| episode_id | bigint(20) | No | INDEX |  |  |
| broadcast_time | datetime | No |  |  |  |

以下の外部キー制約を
