# SQL-Progress-Journal-day-26

# The Most Efficient MLB Players by Salary — 2001 Season

## 🎯 Project Overview

This project analyzes Major League Baseball (MLB) player performance during the 2001 season to identify **the most efficient players per dollar spent**. By comparing **Home Runs (HR)**, **Hits (H)**, and **Runs Batted In (RBI)** relative to player salary, I was able to uncover hidden value—players who significantly outperformed their contracts.

## 📊 Metrics Defined

Efficiency was calculated for each of the following:

- **HR Efficiency** = (HR / Salary) * 100,000
- **Hits Efficiency** = (Hits / Salary) * 100,000
- **RBI Efficiency** = (RBI / Salary) * 100,000

Only the **top 50** performers by efficiency in each category were retained. Then, using SQL, I **intersected all three rankings** to identify the **most consistently efficient players** across all offensive metrics.

## 🏆 Key Insight

> 🔥 **Albert Pujols** was the single most efficient player in the MLB for 2001.

Despite earning only **$200,000**, Pujols posted:
- **37 Home Runs**
- **194 Hits**
- **130 RBIs**

His performance dominated all three metrics **relative to salary**, making him a clear outlier and massive ROI for the Cardinals.

## 🧠 Tools Used

- **SQL** (MySQL)
- **Tableau** (for data visualization)
- **CS50 Moneyball Dataset**

- Quarry used:

- .mode csv
.headers on
.output top28_efficiency.csv
WITH hr_top50 AS (
    SELECT pf.player_id, pf.hr, pf.year, s.salary,
           ROUND((1.0 * pf.hr) / s.salary * 100000, 4) AS hr_efficiency
    FROM performances pf
    JOIN salaries s ON pf.player_id = s.player_id AND pf.year = s.year
    WHERE pf.year = 2001
    ORDER BY hr_efficiency DESC
    LIMIT 50
),
hits_top50 AS (
    SELECT pf.player_id, pf.h, s.salary,
           ROUND((1.0 * pf.h) / s.salary * 100000, 4) AS h_efficiency
    FROM performances pf
    JOIN salaries s ON pf.player_id = s.player_id AND pf.year = s.year
    WHERE pf.year = 2001
    ORDER BY h_efficiency DESC
    LIMIT 50
),
rbi_top50 AS (
    SELECT pf.player_id, pf.rbi, s.salary,
           ROUND((1.0 * pf.rbi) / s.salary * 100000, 4) AS rbi_efficiency
    FROM performances pf
    JOIN salaries s ON pf.player_id = s.player_id AND pf.year = s.year
    WHERE pf.year = 2001
    ORDER BY rbi_efficiency DESC
    LIMIT 50
)

SELECT
    p.first_name,
    p.last_name,
    p.id,
    hr_top50.hr AS Home_Runs,
    hr_top50.hr_efficiency AS HR_Efficiency,
    hits_top50.h AS Hits,
    hits_top50.h_efficiency AS Hits_Efficiency,
    rbi_top50.rbi AS Runs_Batted_In,
    rbi_top50.rbi_efficiency AS RBI_Efficiency,
    hr_top50.salary
FROM hr_top50
JOIN hits_top50 ON hr_top50.player_id = hits_top50.player_id
JOIN rbi_top50 ON hr_top50.player_id = rbi_top50.player_id
JOIN players p ON p.id = hr_top50.player_id;
.output stdout


## 📁 Files

- `top28_efficiency.sql` — Final SQL query using CTEs to filter top players across all 3 metrics
- `top28_efficiency.csv` — Exported results used for Tableau visualizations
- `moneyball_efficiency_2001.twbx` — Tableau workbook
- `grouped_bar_chart.png` — Summary visual showing top players’ performance vs. efficiency

## 📌 Takeaways

- Combining performance metrics with salary data reveals deeper insights than totals alone.
- CTEs (Common Table Expressions) and `JOIN`s are powerful for layered filtering.
- ROI-based metrics are critical in real-world business and sports analytics.

## 🧑‍💻 Author

[Your Name]  
#100DaysOfSQL | #DataEngineering | #BIProjects | #MoneyballSQL

