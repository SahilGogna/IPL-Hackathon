# 🏏 IPL Dataset — Data Guide

This document covers everything you need to know about the dataset provided for the **IPL Hackathon**. It includes how to download and load the data, what each file contains, and a full data dictionary.

---

## 📦 What's in the Dataset?

The dataset contains **IPL (Indian Premier League) ball-by-ball data from 2008 to 2025**, extracted from the [Cricsheet](https://cricsheet.org/) website.

It is divided into **two parts**:

| File | Level | Description |
|------|-------|-------------|
| `deliveries` | Ball-by-ball | Every single delivery bowled across all IPL matches |
| `matches` | Match-level | High-level match metadata (result, toss, venue, etc.) |

> The dataset is also available as **`archive.zip`** in the repository root.

---

## ⬇️ Downloading the Dataset

### Option 1 — From the Repository

The dataset is already included as `archive.zip` in this repository. Simply unzip it:

```bash
unzip archive.zip
```

### Option 2 — Via KaggleHub (Python)

```python
import kagglehub

# Download latest version
path = kagglehub.dataset_download("dgsports/ipl-ball-by-ball-2008-to-2022")

print("Path to dataset files:", path)
```

Install `kagglehub` if you haven't already:

```bash
pip install kagglehub
```

---

## 🐍 Loading the Data in Python

```python
import pandas as pd

# Load ball-by-ball deliveries
deliveries = pd.read_csv("deliveries.csv")

# Load match-level data
matches = pd.read_csv("matches.csv")

print(f"Deliveries: {len(deliveries):,} rows")
print(f"Matches:    {len(matches):,} rows")
```

### Joining the Two Files

Both files share a common `match_id` key:

```python
# Join deliveries with match metadata
full_df = deliveries.merge(matches, on="match_id", how="left")
```

---

## 📖 Data Dictionary

### Deliveries Table (Ball-by-Ball)

| Column | Type | Description | Sample Values |
|--------|------|-------------|---------------|
| `match_id` | int | Unique identifier for each match | 335982, 335983 |
| `date` | str | Match date | 2008-04-18 |
| `match_type` | str | Format of the match | T20 |
| `event_name` | str | Tournament name | Indian Premier League |
| `innings` | int | Innings number (1, 2, or 3 for super over) | 1, 2, 3 |
| `batting_team` | str | Team currently batting | Kolkata Knight Riders |
| `bowling_team` | str | Team currently bowling | Royal Challengers Bangalore |
| `over` | int | Over number, zero-indexed (0 = over 1) | 0, 1, 2 |
| `ball` | int | Ball number within the over | 1, 2, 3, 4, 5, 6 |
| `ball_no` | float | Combined over.ball notation | 0.1, 4.3, 19.6 |
| `batter` | str | Batter facing the delivery | SC Ganguly |
| `bat_pos` | int | Batter's position in the batting order | 1, 2, 3 |
| `non_striker` | str | Batter at the non-striker end | BB McCullum |
| `non_striker_pos` | int | Non-striker's batting order position | 1, 2, 3 |
| `bowler` | str | Bowler delivering the ball | P Kumar |
| `valid_ball` | int | Whether the delivery counts as a legal ball (1 = yes, 0 = no) | 0, 1 |
| `runs_batter` | int | Runs credited to the batter | 0, 1, 2, 4, 6 |
| `balls_faced` | int | Whether the batter faced the ball (1 = yes) | 0, 1 |
| `runs_extras` | int | Extras conceded on this delivery | 0, 1, 5 |
| `runs_total` | int | Total runs from this delivery (batter + extras) | 0, 1, 4 |
| `runs_bowler` | int | Runs charged to the bowler (excludes byes and leg-byes) | 0, 1, 4 |
| `runs_not_boundary` | bool | True if the runs scored were not a boundary | True, False |
| `extra_type` | str | Type of extra conceded | wides, legbyes, noballs, byes |
| `wicket_kind` | str | Dismissal type (null if no wicket) | caught, bowled, lbw, run out, stumped |
| `player_out` | str | Name of dismissed batter (null if no wicket) | SC Ganguly |
| `fielders` | str | Fielder(s) involved in the dismissal | JH Kallis |
| `striker_out` | bool | Whether the batter on strike was dismissed | True, False |
| `new_batter` | str | Batter who came in after a wicket | RT Ponting |
| `next_batter` | str | Next batter to come in | DJ Hussey |
| `batting_partners` | str | Current partnership pair | ('BB McCullum', 'SC Ganguly') |
| `team_runs` | int | Cumulative team runs at this point in the innings | 1, 45, 120 |
| `team_balls` | int | Cumulative legal balls faced in the innings | 1, 12, 60 |
| `team_wicket` | int | Cumulative wickets fallen in the innings | 0, 1, 5 |
| `batter_runs` | int | Cumulative runs scored by the batter in this innings | 0, 4, 47 |
| `batter_balls` | int | Cumulative balls faced by the batter in this innings | 1, 5, 30 |
| `bowler_wicket` | int | Wickets taken by the bowler in this spell | 0, 1, 2 |
| `runs_target` | float | Target set for the chasing team | 223.0, 141.0 |
| `review_batter` | str | Batter involved in a DRS review (null if no review) | MS Dhoni |
| `team_reviewed` | str | Team that took the DRS review | Mumbai Indians |
| `review_decision` | str | Outcome of the DRS review | upheld, struck down |
| `umpire` | str | On-field umpire | CB Gaffaney |
| `umpires_call` | bool | Whether the DRS decision was umpire's call | True, False |

### Match-Level Columns (also available in Deliveries)

| Column | Type | Description | Sample Values |
|--------|------|-------------|---------------|
| `player_of_match` | str | Player of the match award | BB McCullum |
| `match_won_by` | str | Winning team | Kolkata Knight Riders |
| `win_outcome` | str | Margin of victory | 140 runs, 9 wickets |
| `toss_winner` | str | Team that won the toss | Royal Challengers Bangalore |
| `toss_decision` | str | Decision made after winning the toss | bat, field |
| `venue` | str | Ground name | M Chinnaswamy Stadium |
| `city` | str | Host city | Bangalore |
| `day` | int | Day of the match | 18 |
| `month` | int | Month of the match | 4 |
| `year` | int | Year of the match | 2008 |
| `season` | str | IPL season identifier | 2007/08, 2025 |
| `superover_winner` | str | Team that won the super over (null if no super over) | Rajasthan Royals |
| `result_type` | str | Special result type (null for normal results) | tie, no result |
| `method` | str | Method used to determine a revised result | D/L |
| `stage` | str | Stage of the tournament | Unknown, Semi Final, Final |
| `event_match_no` | str | Match number within the event | 1, 2, 74 |
| `balls_per_over` | int | Balls per over (always 6) | 6 |
| `overs` | int | Total overs per innings (always 20) | 20 |

---

## 💡 Key Notes & Gotchas

- **`over` is zero-indexed** — Over 0 = the 1st over of the innings. Add 1 when displaying to users.
- **`valid_ball`** distinguishes legal deliveries from wides and no-balls. Use it when computing strike rates and economy rates.
- **Innings 3** represents a **super over**. Filter it out for standard analysis.
- **`runs_bowler` ≠ `runs_total`** — Byes and leg-byes are charged to the innings but not to the bowler.
- **Null values** appear in `wicket_kind`, `player_out`, `fielders`, `new_batter`, `review_batter`, `superover_winner`, `result_type`, and `method` — handle them accordingly.
- **`batting_partners`** is stored as a string representation of a Python tuple — parse with `ast.literal_eval()` if needed.

---

## 📊 Dataset Coverage

| Attribute | Details |
|-----------|---------|
| **Seasons** | 2008 – 2025 (IPL seasons 1 through 18) |
| **Source** | [Cricsheet.org](https://cricsheet.org/) |
| **Kaggle** | [dgsports/ipl-ball-by-ball-2008-to-2022](https://www.kaggle.com/datasets/dgsports/ipl-ball-by-ball-2008-to-2022) |
| **Download** | `archive.zip` (included in repo) |

---

*Happy hacking! 🏏*
