# Spotify Streaming Analytics – Power BI

This repository contains a Power BI report that analyzes global Spotify streaming data to answer a single question:

> **Who’s turning listeners into streams?**  
> *The unsung metric of artist efficiency.*

Using a synthetic 2024 Spotify-style dataset, the report explores:
- **Artist-level efficiency** – how well artists convert monthly listeners into streams.
- **Genre-level engagement** – skip rates, replay behaviour, completion rates, and revenue implications.
- **Platform dynamics** – how **Free vs Premium** users behave across genres.
- **Engagement quadrants & popularity paradoxes** – visual storytelling around streams vs skips vs replays.

---

## 🔍 Key Concepts

### Listener Stream Efficiency (LSE)
**LSE** measures how efficiently an artist turns monthly listeners into total streams:

> **LSE = Total Streams (M) ÷ Monthly Listeners (M)**

- Higher LSE → deeper engagement, repeat listens, stronger fan conversion.
- Lower LSE → lots of passive listeners with relatively fewer streams.

Other core metrics:
- **Skip Rate (%)** – how often tracks are skipped.
- **Replay Rate** – how often tracks are replayed.
- **Completion Rate** – how often tracks are listened to near completion.
- **Momentum Score** – how much of an artist’s streaming volume comes from the last 30 days.

---

## 📊 Report Pages

The Power BI report currently includes:

1. **Artist Efficiency**
   - *Artist Efficiency Leaderboard* (Top artists by LSE / replay-driven engagement)
   - *The Spotify Graveyard* – long-tail distribution of replay rates.

2. **Genre Engagement**
   - *Engagement Quadrant* – Avg Replay Rate vs Avg Skip Rate by Genre.
   - *Genre Engagement Hierarchy – Premium vs Free* – compares skip rates by platform type.
   - Genre summary table with Avg Skip %, Avg Replay Rate, Album count.

3. **Popularity vs Engagement**
   - *The Popularity Paradox* – Total Streams (M) vs Avg Skip Rate (%).
   - *The Engagement Matrix* – Replay-driven engagement vs Skips.

4. **Platform Dynamics**
   - Free vs Premium scatter for genres (Replay, Skip, Streams).

Each page is documented in detail in [`/docs/04_report_design.md`](docs/04_report_design.md).

---

## 🧱 Data

Source file (example):

- `data/Spotify_2024_Global_Streaming_Data.csv`

Core columns:

- `Country`
- `Artist`
- `Album`
- `Genre`
- `Release Year`
- `Monthly Listeners (Millions)`
- `Total Streams (Millions)`
- `Total Hours Streamed (Millions)`
- `Avg Stream Duration (Min)`
- `Platform Type` (Free / Premium)
- `Streams Last 30 Days (Millions)`
- `Skip Rate (%)`
- `Replay Rate`
- `Completion Rate`

Aggregated summary table (genre-level) is created in Power BI using DAX (see [`docs/03_dax_measures.md`](docs/03_dax_measures.md)).

This dataset is **synthetic** and intended for learning, portfolio, and demo purposes.

---

## 🛠️ Getting Started

### Prerequisites

- **Power BI Desktop** (current version)
- Basic familiarity with:
  - Creating measures in DAX
  - Importing CSV files
  - Using visuals such as bar charts, scatter plots, and tables

Add README

