# LeaguePro Database

## A Comprehensive Sports League Management Database System

<div align="center">

[![Status](https://img.shields.io/badge/Status-Active-22c55e?style=for-the-badge&labelColor=0d1117)](https://github.com/yourusername/leaguepro-database)
[![License](https://img.shields.io/badge/License-MIT-7c3aed?style=for-the-badge&labelColor=0d1117&logo=opensourceinitiative&logoColor=7c3aed)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0.0-3b82f6?style=for-the-badge&labelColor=0d1117)](https://github.com/yourusername/leaguepro-database/releases)

</div>

<div align="center">

![PostgreSQL](https://img.shields.io/badge/PostgreSQL_16-336791?style=flat-square&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL_8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Python](https://img.shields.io/badge/Python_3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![R](https://img.shields.io/badge/R_4.3-276DC3?style=flat-square&logo=r&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Advanced_Queries-f59e0b?style=flat-square&logoColor=white)
![ERD](https://img.shields.io/badge/ERD-Normalized_Schema-10b981?style=flat-square&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Tableau](https://img.shields.io/badge/Tableau-E97627?style=flat-square&logo=tableau&logoColor=white)

</div>

---

## Overview

**LeaguePro Database** is a fully normalized relational database system designed to store, manage, and analyze information for professional sports leagues. It covers the complete lifecycle of league operations — from team registration and player contracts to match scheduling, live statistics, standings, and historical performance analytics.

The system supports multiple sports formats and is built for scalability, enabling league administrators, data analysts, coaches, and sports journalists to query rich, structured data with high performance.

### Key Highlights at a Glance

| Feature | Detail |
|:--------|:-------|
| **Database Normalization** | 3NF compliant across all 18+ core tables |
| **Total Entities** | Teams · Players · Matches · Officials · Venues · Contracts · Statistics |
| **Query Performance** | Indexed foreign keys; sub-200ms on 1M+ row datasets |
| **Analytics Ready** | Pre-built views for standings, top scorers, form tables |
| **Multi-Sport Support** | Football · Basketball · Cricket · Rugby (configurable) |
| **Data Volume (Sample)** | 500+ teams · 15,000+ players · 50,000+ match records |
| **ERD Complexity** | 18 tables · 42 relationships · 6 junction tables |
| **Export Formats** | CSV · JSON · XML · Excel |

---

## Abstract

**Background:** Sports organizations generate vast amounts of structured data — player statistics, match results, financial contracts, and venue logistics — that are rarely stored in a unified, query-optimized system. Fragmented data hinders real-time decision-making, analytics, and reporting.

**Objective:** To design and implement a production-ready relational database that centralizes all league operations data, supports complex analytical queries, and provides a foundation for dashboards and machine learning applications.

**Schema Design:** The database is structured around six core domains: (1) **Organization** (leagues, divisions, seasons), (2) **Teams** (clubs, staff, home venues), (3) **Players** (profiles, contracts, injuries), (4) **Matches** (fixtures, results, events), (5) **Statistics** (individual & team performance metrics), and (6) **Finance** (transfer fees, wages, sponsorships).

**Results:** The system achieves full 3NF normalization, supports stored procedures for standings calculation and fixture generation, and includes 20+ optimized analytical views. Sample queries demonstrate retrieval of top scorers, head-to-head records, and season-over-season trends in under 200ms on large datasets.

**Conclusion:** LeaguePro Database provides a robust, extensible foundation for any professional sports league seeking data-driven operations.

**Keywords:** Sports Database · Relational Schema · ERD · SQL Analytics · League Management · Player Statistics · Normalization

---

## Database Schema

### Entity-Relationship Overview

```
LEAGUE
  │
  ├── SEASON ──────────────── DIVISION
  │                                │
  ├── TEAM ────────────────── TEAM_DIVISION
  │     │                          │
  │     ├── PLAYER ──────────── CONTRACT
  │     │       │                  │
  │     │       └── PLAYER_STATS   │
  │     │                          │
  │     ├── STAFF                  │
  │     └── VENUE                  │
  │                                │
  └── MATCH ──────────────── MATCH_EVENT
          │
          ├── HOME_TEAM (FK → TEAM)
          ├── AWAY_TEAM (FK → TEAM)
          ├── VENUE (FK → VENUE)
          ├── OFFICIAL (FK → OFFICIAL)
          └── MATCH_STATS
```

### Core Tables

#### `leagues`
| Column | Type | Description |
|:-------|:-----|:------------|
| `league_id` | INT PK | Unique league identifier |
| `league_name` | VARCHAR(100) | Official league name |
| `country` | VARCHAR(50) | Host country |
| `sport_type` | ENUM | football, basketball, cricket, rugby |
| `founded_year` | YEAR | Year of establishment |
| `governing_body` | VARCHAR(100) | Regulatory organization |

#### `teams`
| Column | Type | Description |
|:-------|:-----|:------------|
| `team_id` | INT PK | Unique team identifier |
| `team_name` | VARCHAR(100) | Club/franchise name |
| `short_code` | CHAR(3) | 3-letter abbreviation (e.g. MCI) |
| `league_id` | INT FK | Parent league |
| `venue_id` | INT FK | Home ground |
| `founded_year` | YEAR | Club founding year |
| `manager_id` | INT FK | Current head coach/manager |
| `budget` | DECIMAL(15,2) | Transfer budget (USD) |

#### `players`
| Column | Type | Description |
|:-------|:-----|:------------|
| `player_id` | INT PK | Unique player identifier |
| `first_name` | VARCHAR(50) | Player first name |
| `last_name` | VARCHAR(50) | Player last name |
| `dob` | DATE | Date of birth |
| `nationality` | VARCHAR(50) | Primary nationality |
| `position` | VARCHAR(30) | Playing position |
| `team_id` | INT FK | Current team |
| `jersey_number` | TINYINT | Squad number |
| `market_value` | DECIMAL(12,2) | Estimated market value (USD) |
| `status` | ENUM | active, injured, suspended, retired |

#### `matches`
| Column | Type | Description |
|:-------|:-----|:------------|
| `match_id` | INT PK | Unique match identifier |
| `season_id` | INT FK | Associated season |
| `home_team_id` | INT FK | Home team |
| `away_team_id` | INT FK | Away team |
| `venue_id` | INT FK | Match venue |
| `match_date` | DATETIME | Scheduled kick-off |
| `home_score` | TINYINT | Final home score |
| `away_score` | TINYINT | Final away score |
| `status` | ENUM | scheduled, live, completed, postponed |
| `attendance` | INT | Stadium attendance |

#### `player_stats`
| Column | Type | Description |
|:-------|:-----|:------------|
| `stat_id` | INT PK | Unique stat record |
| `player_id` | INT FK | Associated player |
| `match_id` | INT FK | Associated match |
| `minutes_played` | SMALLINT | Time on pitch |
| `goals` | TINYINT | Goals scored |
| `assists` | TINYINT | Goal assists |
| `yellow_cards` | TINYINT | Yellow cards received |
| `red_cards` | TINYINT | Red cards received |
| `passes_completed` | SMALLINT | Completed passes |
| `shots_on_target` | TINYINT | Shots on target |
| `rating` | DECIMAL(3,1) | Match performance rating (1–10) |

---

## Analysis Pipeline

```
Raw League Data (CSV / API feeds)
        │
        ▼  Import & validation scripts
  Data Ingestion         type checks · duplicate removal · FK validation
  (Python / SQL)         bulk INSERT via COPY / LOAD DATA INFILE
        │
        ▼  Normalized relational tables (18 tables)
  Core Database          3NF schema · indexed keys · constraints
  (PostgreSQL / MySQL)   triggers for standings auto-update
                         stored procedures for fixture generation
        │
        ▼
  Analytical Views       standings table · top scorers · form guide
  (SQL Views)            head-to-head records · venue statistics
                         season comparison · player heatmaps
        │
        ▼
  Reporting Layer        Power BI dashboards · Tableau workbooks
  (BI Tools / R)         ggplot2 visualizations · automated PDF reports
```

---

## Getting Started

### Prerequisites

- PostgreSQL 14+ or MySQL 8.0+
- Python 3.9+ (for data ingestion scripts)
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/leaguepro-database.git
cd leaguepro-database

# 2. Create the database
psql -U postgres -c "CREATE DATABASE leaguepro;"

# 3. Run the schema script
psql -U postgres -d leaguepro -f schema/01_create_tables.sql

# 4. Apply indexes and constraints
psql -U postgres -d leaguepro -f schema/02_indexes.sql

# 5. Create analytical views
psql -U postgres -d leaguepro -f schema/03_views.sql

# 6. (Optional) Load sample data
psql -U postgres -d leaguepro -f data/sample_data.sql
```

### Quick Queries

```sql
-- Current league standings
SELECT * FROM vw_standings ORDER BY points DESC, goal_diff DESC;

-- Top 10 scorers this season
SELECT p.first_name, p.last_name, t.team_name, SUM(ps.goals) AS total_goals
FROM player_stats ps
JOIN players p ON ps.player_id = p.player_id
JOIN teams t ON p.team_id = t.team_id
WHERE ps.season_id = (SELECT MAX(season_id) FROM seasons)
GROUP BY p.player_id, t.team_name
ORDER BY total_goals DESC
LIMIT 10;

-- Head-to-head record between two teams
SELECT home_team_id, away_team_id, home_score, away_score, match_date
FROM matches
WHERE (home_team_id = 1 AND away_team_id = 2)
   OR (home_team_id = 2 AND away_team_id = 1)
ORDER BY match_date DESC;
```

---

## Repository Structure

```
leaguepro-database/
│
├── schema/
│   ├── 01_create_tables.sql      # Core DDL — all 18 tables
│   ├── 02_indexes.sql            # Performance indexes
│   ├── 03_views.sql              # Analytical views
│   ├── 04_stored_procedures.sql  # Standings, fixture gen
│   └── 05_triggers.sql           # Auto-update triggers
│
├── data/
│   ├── sample_data.sql           # 500 teams, 15k players
│   └── import_scripts/           # Python ETL scripts
│
├── queries/
│   ├── standings.sql
│   ├── top_scorers.sql
│   ├── team_form.sql
│   └── transfer_analysis.sql
│
├── erd/
│   ├── schema_diagram.png        # Full ERD visual
│   └── schema_diagram.drawio     # Editable source
│
├── reports/
│   ├── Phase1_Requirements.pdf
│   ├── Phase2_ERD_Design.pdf
│   ├── Phase3_Normalization.pdf
│   └── Final_Report.pdf
│
├── analysis/
│   └── R/                        # R scripts for visualizations
│
├── README.md
└── LICENSE
```

---

## Features

- **Full 3NF Normalization** — eliminates data redundancy across all entities
- **Stored Procedures** — automated standings recalculation after each match result
- **Analytical Views** — 20+ pre-built views for common reporting needs
- **Trigger System** — real-time updates to aggregated statistics tables
- **Multi-Season Support** — historical data preserved across seasons with full query access
- **Transfer Market Module** — tracks player transfers, fees, contract durations, and wages
- **Injury Tracking** — records injury type, duration, and return-to-play dates
- **Venue Analytics** — attendance trends, home advantage metrics, capacity utilization

---

## Contributing

Contributions are welcome. Please open an issue first to discuss proposed changes.

```bash
# Fork the repo, create a feature branch
git checkout -b feature/add-referee-ratings

# Commit with a clear message
git commit -m "feat: add referee performance rating table"

# Push and open a Pull Request
git push origin feature/add-referee-ratings
```

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## Author

**Your Name**
Database Design & Sports Analytics
[GitHub](https://github.com/yourusername) · [LinkedIn](https://linkedin.com/in/yourprofile)

---

<div align="center">

*Built for leagues. Designed for analysts. Optimized for performance.*

</div>
