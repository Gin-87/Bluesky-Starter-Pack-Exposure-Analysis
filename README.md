# Who Gets Boosted, Where, and Who Drives It? — Bluesky Starter Pack Exposure Analysis

![Python](https://img.shields.io/badge/Python-3.10-blue) ![Bluesky](https://img.shields.io/badge/Platform-Bluesky-teal)

This project examines how attention is distributed through Bluesky Starter Packs — a recommendation mechanism designed to help new users discover accounts to follow. Using a full-network snapshot dataset of 347,192 Starter Packs and 11.7 million pack–member edges, we quantify exposure concentration, compare inequality across pack themes and languages, and analyze the creator behavior that drives repeated recommendations.

---

## Research Questions

- **Q1 — Who gets boosted?** To what extent do Starter Packs distribute attention broadly versus amplifying a small set of highly visible accounts?
- **Q2 — Where?** How does exposure inequality differ across pack themes (politics, science, art, etc.) and languages?
- **Q3 — Who drives it?** Is repeated recommendation driven by a concentrated group of highly active pack creators?

## Key Findings

- Account exposure follows a heavy-tailed distribution (Gini = 0.76). The top 1% of accounts receive ~49% of all pack inclusions.
- Inequality is highest in `art_design` (Gini = 0.777) and lowest in `lgbtq` (Gini = 0.193). English (0.733), Swedish (0.702), and German (0.685) show the strongest language-level concentration.
- Creator supply is strongly long-tailed: a small subset of creators contributes tens of thousands of edges, mechanically amplifying repeated exposure for a narrow set of accounts.
- Within the observable boosted pool (8,877 accounts), the top 10% absorb 90.7% of pack inclusions. The most-boosted accounts behave as passive celebrities — high follower/following ratio (median 799) and low posting activity.

## Dataset

| File | Description | Source |
|------|-------------|--------|
| `starter_packs.parquet` | Starter Pack metadata (title, description, creator) | Balduf et al. (2024) |
| `list_items.parquet` | Pack–member membership edges | Balduf et al. (2024) |
| `starterpack_edges_processed.parquet` | Processed edge table with theme/language labels | Generated |
| `starterpack_edges_processed_cleaned.parquet` | Cleaned subset (low-volume categories removed) | Generated |

Raw datasets are downloaded at runtime from `bsky-data.leobalduf.com`.

## Project Structure
```
├── Project_Phase_3.ipynb       # Main analysis notebook
├── selected_packs.csv          # High-coverage pack IDs for API enrichment
├── real_pack_member_df.csv     # API-resolved pack → member handle table
├── real_member_profiles.csv    # Bluesky profile metadata for boosted accounts
└── README.md
```

## Setup & Usage
```bash
pip install pandas pyarrow matplotlib langdetect tqdm atproto requests
```

Open `Project_Phase_3.ipynb` in Jupyter or Google Colab and run cells in order. For API enrichment (Further Analysis 1–3), a Bluesky app password is required — set `USERNAME` and `APP_PASSWORD` in the credentials cell.

## Methods

- Rule-based theme labeling (keyword matching on pack title + description)
- Language detection via `langdetect`
- Inequality metrics: Gini coefficient, top-k share, Lorenz curve
- Streaming row-group aggregation for memory-efficient processing of 11M+ edges
- Bluesky AT Protocol API + web scraping for observable identity resolution


## References

Balduf et al. (2024). Looking AT the Blue Skies of Bluesky. *ACM IMC.* https://doi.org/10.1145/3646547.3688407
