# Gaming Profiles 2025: Database Project

## Xbox Gaming Data

* **Name:** Jak
* **Course:** Database for Analytics
* **Project:** Gaming Profiles 2025
* **Database:** `Video Games`
* **Tools Used:** PostgreSQL, pgAdmin, DrawDB, GitHub
* **Programming Language:** SQL

---

## 1. Initial Data Source

### Overview

* **Original Data Source:** Gaming Profiles 2025 (Steam, PlayStation, Xbox)
* **Dataset URL:** [Gaming Profiles 2025 on Kaggle](https://www.kaggle.com/datasets/artyomkruglov/gaming-profiles-2025-steam-playstation-xbox/data?select=xbox)
* **File Format:** CSV
* **Subject:** Xbox players and gaming information

### Description

I selected this dataset because, as a gamer myself, I wanted to work with information related to a topic I was personally interested in. The dataset included information about Steam, PlayStation, and Xbox players. I chose the Xbox data because, although I currently have a PlayStation, I had more enjoyable gaming experiences on Xbox.

### Screenshot

![Initial Data Source](screenshots/initial_data_source.png)

---

## 2. Data Format and Dataset Size

The original data was provided in CSV format and loaded into PostgreSQL. The data was organized into six tables to support database queries and analysis.

### Dataset Overview

| Attribute                       | Description         |
| ------------------------------- | ------------------- |
| Original file format            | CSV                 |
| Database format                 | Relational database |
| Database management system      | PostgreSQL          |
| Number of tables                | 6                   |
| Total rows across all tables    | 27,580,772          |
| Total columns across all tables | 26                  |

**Note:** The total row count represents the sum of the records across all six tables. It does not necessarily represent the number of unique records because the tables contain different types of related information.

### 2.1 Row Counts

The following SQL queries count the records in each table.

```sql
SELECT COUNT(*) AS row_count FROM players;

SELECT COUNT(*) AS row_count FROM games;

SELECT COUNT(*) AS row_count FROM achievements;

SELECT COUNT(*) AS row_count FROM prices;

SELECT COUNT(*) AS row_count FROM purchased_games;

SELECT COUNT(*) AS row_count FROM history;
```

### 2.2 Column Counts

The following query displays the number of columns in each table.

```sql
SELECT
    table_name,
    COUNT(*) AS column_count
FROM information_schema.columns
WHERE table_schema = 'public'
  AND table_name IN (
      'players',
      'games',
      'achievements',
      'prices',
      'purchased_games',
      'history'
  )
GROUP BY table_name
ORDER BY table_name;
```

### Dataset Counts

| Table             | Number of Rows | Number of Columns |
| ----------------- | -------------: | ----------------: |
| `players`         |        274,450 |                 5 |
| `games`           |         10,489 |                 7 |
| `achievements`    |        351,111 |                 3 |
| `prices`          |         22,638 |                 2 |
| `purchased_games` |     11,656,184 |                 7 |
| `history`         |     15,275,900 |                 2 |
| **Total**         | **27,580,772** |            **26** |

### Screenshot

![Dataset Counts](screenshots/2_data_counts.png)

---

## 3. Data Dictionary and Schema Overview

The database contains six tables, each representing a different category of gaming information.

* **`achievements`** — contains achievement information associated with games.
* **`games`** — contains general game information, including titles and developer information.
* **`history`** — contains records of player gaming activity from 2008 through 2025.
* **`players`** — contains information about Xbox players.
* **`prices`** — contains game pricing information.
* **`purchased_games`** — contains records of games purchased by players.

The following query displays each table's column names, data types, and column order.

```sql
SELECT
    table_name,
    ordinal_position,
    column_name,
    data_type
FROM information_schema.columns
WHERE table_schema = 'public'
  AND table_name IN (
      'players',
      'games',
      'achievements',
      'prices',
      'purchased_games',
      'history'
  )
ORDER BY table_name, ordinal_position;
```

### Screenshot

![Data Structure](screenshots/DataStructure.png)

---

## 4. Data Transformation and Challenges

### Challenge 1: Selecting a Dataset

One of the first challenges was finding a dataset that met the project requirements while covering a topic I was personally interested in. I explored gaming datasets and selected Gaming Profiles 2025 because it included information about Steam, PlayStation, and Xbox. I chose the Xbox data because of my personal interest in the platform and my previous gaming experiences with it.

**Solution:**

I narrowed the project to the Xbox portion of the dataset. This allowed me to focus the database design and analysis on one gaming platform rather than working with all three platforms.

### Challenge 2: Loading the Data

The dataset was provided in CSV format, while the project required the data to be stored and queried in PostgreSQL. The data also needed to be organized into separate tables so that the information could be examined and analyzed.

**Solution:**

I used PostgreSQL and pgAdmin to load and manage the data. After importing the data, I ran SQL queries to count the records and inspect the columns in each table. This helped me verify the amount of data stored in the database.

### Challenge 3: Understanding the Data Structure and Data Types

Working with multiple tables required understanding what each table represented, which columns could connect related records, and how the tables could be used together in SQL queries.

**Solution:**

I used the `information_schema.columns` view to inspect column names, data types, and column order. I also used row-count queries to examine the size of each table and SQL joins to analyze information across related tables. The DrawDB diagram helped me visualize the database structure and relationships.

---

## 5. Table Structure and Data Types

The DrawDB diagram provides a visual representation of the database tables and their relationships. It helps illustrate how the data is organized and how the tables connect to support the SQL analysis.

### Database Relationship Diagram

![Video Games Database Structure and Relationships](screenshots/VideoGames_db.png)

---

## 6. Displaying All Tables

The following SQL statements retrieve all columns and records from each table. Because some tables contain millions of records, pgAdmin may display only a portion of the results at a time.

### 6.1 Players

```sql
SELECT * FROM players;
```

![Players Query Results](screenshots/6_players.png)

### 6.2 Games

```sql
SELECT * FROM games;
```

![Games Query Results](screenshots/6_games.png)

### 6.3 Achievements

```sql
SELECT * FROM achievements;
```

![Achievements Query Results](screenshots/6_achievements.png)

### 6.4 Prices

```sql
SELECT * FROM prices;
```

![Prices Query Results](screenshots/6_prices.png)

### 6.5 Purchased Games

```sql
SELECT * FROM purchased_games;
```

![Purchased Games Query Results](screenshots/6_purchased_games.png)

### 6.6 History

```sql
SELECT * FROM history;
```

![History Query Results](screenshots/6_history.png)

---

## 7. SQL Queries and Analysis

### 7.1 Which Developers Produced the Most Games?

```sql
SELECT
    developers,
    COUNT(gameid) AS total_games_produced
FROM games
WHERE developers IS NOT NULL
GROUP BY developers
ORDER BY total_games_produced DESC
LIMIT 20;
```

#### Results and Interpretation

In my query results, SNK and Hamster Corporation ranked highest, with 127 games. Ubisoft followed with 68 games, and EpiXR Games ranked third with 57 games.

The results show that SNK and Hamster Corporation had a notably large number of games in this dataset compared with the next-ranked developer. However, these findings represent the records in the selected dataset and should not be interpreted as a complete count of every game produced by each developer.

#### Screenshot

![Top Developers](screenshots/top_developers.png)

---

### 7.2 Which Players Have the Most Achievement Points?

```sql
SELECT
    p.playerid,
    p.nickname,
    SUM(a.points) AS total_achievement_points
FROM players AS p
JOIN history AS h
    ON p.playerid = h.playerid
JOIN achievements AS a
    ON h.achievementid = a.achievementid
GROUP BY p.playerid, p.nickname
ORDER BY total_achievement_points DESC
LIMIT 20;
```

#### Results and Interpretation

In my query results, **Five1oh** ranked first with 4,190,043 achievement points. **Sangriaz** ranked second with 4,110,485 points. These were the only two players in the results with more than four million points.

This analysis identifies the players with the highest summed achievement points based on the records in the joined tables. The interpretation assumes that the history records represent achievement events in a way that makes summing the associated points appropriate.

#### Screenshot

![Top Players by Total Achievement Points](screenshots/query2_achievement_count_by_game.png)

---

### 7.3 How Many Games Have the Top Achievement Players Purchased?

```sql
SELECT
    p.nickname,
    COUNT(pg.gameid) AS total_games_purchased
FROM players AS p
JOIN purchased_games AS pg
    ON p.playerid = pg.playerid
WHERE p.nickname IN ('Five1oh', 'Sangriaz')
GROUP BY p.nickname
ORDER BY total_games_purchased DESC;
```

#### Results and Interpretation

In my query results, Five1oh had 4,430 purchased-game records, while Sangriaz had 4,968. Although Five1oh ranked first in the achievement-points analysis, Sangriaz had the higher number of purchased-game records among these two players.

This comparison suggests that the number of games purchased and the total achievement points earned do not necessarily follow the same ranking.

#### Screenshot

![Games Purchased by Five1oh and Sangriaz](screenshots/most_games.png)

### 7.3.1 Comparing Purchased Games and Listed Prices

The following query compares players' purchased-game records with the prices listed in the `prices` table.

```sql
SELECT
    p.playerid,
    p.nickname,
    COUNT(pg.gameid) AS total_games_purchased,
    SUM(pr.usd) AS total_listed_price_usd
FROM players AS p
JOIN purchased_games AS pg
    ON p.playerid = pg.playerid
JOIN prices AS pr
    ON pg.gameid = pr.gameid
GROUP BY p.playerid, p.nickname
ORDER BY total_games_purchased DESC
LIMIT 20;
```

#### Results and Interpretation

In my query results, Sangriaz was the only player from the top-achievement comparison who appeared in the top 20 for purchased-game records. This suggests that players who purchase the most games are not necessarily the players with the highest achievement-point totals.

**Important:** The `total_listed_price_usd` column represents the sum of the joined listed prices, not necessarily the amount a player actually spent. If a game has multiple price records, the join may count multiple prices for the same purchased-game record. Actual spending would require reliable transaction-price information or other purchase-cost data.

#### Screenshot

![Total Games Purchased](screenshots/total_games_purchased.png)

---

### 7.4 What Are the Game Prices in US Dollars?

```sql
SELECT
    g.gameid,
    g.title AS game_title,
    p.usd
FROM games AS g
INNER JOIN prices AS p
    ON g.gameid = p.gameid
WHERE p.usd IS NOT NULL
ORDER BY p.usd DESC;
```

#### Results and Interpretation

The query displays the listed game prices in US dollars, ordered from highest to lowest. In my results, many games were listed at $69.99, while some titles or editions were priced higher. The highest price I observed was $109.99 for S.T.A.L.K.E.R. 2.

This finding reflects the prices recorded in the dataset. The query results should be checked to confirm the highest-priced title and determine whether the dataset distinguishes standard editions from premium or special editions.

#### Screenshot

![Game Prices in USD](screenshots/Game_price.png)

---

## 8. Conclusion

This project provided an opportunity to work with a large gaming dataset and organize it in a relational database using PostgreSQL. I used SQL to inspect table structures, count records, join related data, and analyze developers, player achievement points, purchased games, and listed game prices.

The analysis demonstrated the importance of understanding table relationships and interpreting query results carefully. For example, the number of games purchased does not necessarily correspond to achievement points, and listed prices should not automatically be treated as actual spending.

Overall, this project allowed me to apply database concepts, practice SQL queries, document a relational schema using DrawDB, and explore gaming data through analytical questions.

---
