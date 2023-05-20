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

## 外部キー制約、ユニークキー制約に関して

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
  
・MySQL始動後下記コードにて新規データベースを作成、今回はinternet_TVというデータベースを作成
  
```
CREATE DATABASE internet_TV;
```
  
2.ステップ1で設計したテーブルを構築
  
・下記コードにて使用するデータベースの選択
  
```
USE internet_TV;
```
  
<details><summary>テーブル構築のSQL文</summary>
  
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

</details>
  
3.サンプルデータの挿入
  <details><summary>鬼滅の刃とゲーム・オブ・スローンズに侵されたサンプル例</summary>
    
```
    
-- channelsテーブルにデータを挿入
INSERT INTO channels (name) VALUES ('ドラマ1'), ('ドラマ2'), ('アニメ1'), ('アニメ2'), ('スポーツ'), ('ペット');

-- genresテーブルにデータを挿入
INSERT INTO genres (genre_name) VALUES ('アニメ'), ('映画'), ('ドラマ'), ('ニュース');

-- programsテーブルにデータを挿入
INSERT INTO programs (title, detail, program_length) VALUES 
('鬼滅の刃', '人間の血を飲む“鬼”と、それを狩る“鬼狩り”の戦いを描くアクションアニメ', 24),
('ゲーム・オブ・スローンズ', '七王国と呼ばれる地域を舞台に、数々の名家が玉座を巡って争うファンタジードラマ', 60);

-- program_genresテーブルにデータを挿入
INSERT INTO program_genres (program_id, genre_id) VALUES 
(1, 1),  -- 鬼滅の刃はアニメジャンルに属する
(2, 3);  -- ゲーム・オブ・スローンズはドラマジャンルに属する

-- seasonsテーブルにデータを挿入
INSERT INTO seasons (program_id, season_number) VALUES 
(1, 1),  -- 鬼滅の刃のシーズン1
(2, 1);  -- ゲーム・オブ・スローンズのシーズン1

-- episodesテーブルにデータを挿入
INSERT INTO episodes (season_id, episode_number, title, detail, duration, release_date, view_count) VALUES 
(1, 1, '鬼滅の刃 第1話', '竈門炭治郎の日常と家族との絆を描く', 24, '2021-04-01', 10000),
(1, 2, '鬼滅の刃 第2話', '鬼に襲われた炭治郎の運命が動き出す', 24, '2021-04-08', 9500),
(2, 1, 'ゲーム・オブ・スローンズ 第1話', 'ウィンターフェルの大公エド・スタークの日常とその運命が描かれる', 60, '2011-04-17', 22000);

-- broadcastsテーブルにデータを挿入
INSERT INTO broadcasts (channel_id, episode_id, broadcast_time, view_count) VALUES 
(1, 1, '2023-05-01 20
    
```
  </details>
 </details>

<details><summary>STEP3</summary>
  1.エピソード視聴数トップ3のエピソードタイトルと視聴数を取得するクエリ

```
SELECT e.title, SUM(b.view_count) as total_views
FROM episodes AS e
JOIN broadcasts AS b ON e.id = b.episode_id
GROUP BY e.id
ORDER BY total_views DESC
LIMIT 3;  
```
  2.エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得するクエリ
  
```
SELECT p.title AS program_title, s.season_number, e.episode_number, e.title AS episode_title, SUM(b.view_count) as total_views
FROM episodes AS e
JOIN seasons AS s ON e.season_id = s.id
JOIN programs AS p ON s.program_id = p.id
JOIN broadcasts AS b ON e.id = b.episode_id
GROUP BY e.id
ORDER BY total_views DESC
LIMIT 3;
```
  3.本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得するクエリ
  
```
SELECT
    c.name AS channel_name,
    b.start_time,
    DATE_ADD(b.start_time, INTERVAL e.duration MINUTE) AS end_time,
    s.season_number,
    e.episode_number,
    e.title AS episode_title,
    e.description AS episode_description
FROM
    broadcasts AS b
JOIN
    channels AS c ON b.channel_id = c.id
JOIN
    episodes AS e ON b.episode_id = e.id
JOIN
    seasons AS s ON e.season_id = s.id
WHERE
    DATE(b.start_time) = CURDATE()
ORDER BY
    b.start_time;
```
4.ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得するクエリ
  
```
SELECT
    b.start_time,
    DATE_ADD(b.start_time, INTERVAL e.duration MINUTE) AS end_time,
    s.season_number,
    e.episode_number,
    e.title AS episode_title,
    e.description AS episode_description
FROM
    broadcasts AS b
JOIN
    channels AS c ON b.channel_id = c.id
JOIN
    episodes AS e ON b.episode_id = e.id
JOIN
    seasons AS s ON e.season_id = s.id
WHERE
    c.name = 'ドラマ' AND
    DATE(b.start_time) BETWEEN CURDATE() AND DATE_ADD(CURDATE(), INTERVAL 7 DAY)
ORDER BY
    b.start_time;
```
  
</details>
  


  

  

  
  

