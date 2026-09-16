# Basketball Intelligence Platform: Design Proposal

A proposed coach-facing application for uploading game statistics, storing player history, and exploring trends. The goal is to reduce manual stat tracking and give coaches a practical way to review recent performance.

**Current repository status: design documentation only.** Application source, a package manifest, migrations, and a runnable demo have not been committed here. The items below describe the intended MVP and are not implemented features in this repository.

## Proposed workflow

1. Upload a CSV box score or enter statistics manually.
2. Validate and normalize player/game/stat records.
3. Store them in a relational database.
4. Filter player history by metric, time window, opponent, and location.
5. Display simple trends and rule-based summaries.

## Intended stack and data model

| Area | Proposed choice |
| --- | --- |
| Interface | Next.js, React, TypeScript, Tailwind CSS, Recharts |
| API | Next.js route handlers |
| Storage | PostgreSQL and Prisma |
| Core entities | Organization, Team, Player, Game, StatType, PlayerGameStat, DataUpload |
| Ingestion | CSV first; manual entry as a fallback |

The proposed stat model stores `Player + Game + StatType + Value`, allowing different sports and metrics without adding a database column for every statistic. The tradeoff is more validation and joins than a fixed basketball-specific table.

## Proposed CSV format

```csv
date,opponent,location,jerseyNumber,statName,value
2026-02-20,Central Prep,Home,1,Points,21
2026-02-20,Central Prep,Home,1,Rebounds,6
```

Player matching would use jersey number within a team, or first/last name with explicit conflict handling.

## Implementation checklist

- [ ] Commit the application source and dependency manifest.
- [ ] Add the Prisma schema and migrations.
- [ ] Implement CSV validation, duplicate detection, and useful upload errors.
- [ ] Provide fictional demo data and a reproducible local setup.
- [ ] Connect player history, queries, and rule-based summaries.
- [ ] Add access control before using real student-athlete data in a hosted application.

PDF parsing, OCR, fatigue scoring, injury prediction, and AI-generated training recommendations are outside the initial proposal.

## Related working projects

- [Sports Data Integration and Forecasting Pipeline](https://github.com/davislaroque/Sports-Data-Integration-and-Forecasting-Pipeline): API ingestion, odds normalization, and a dashboard.
- [NBA Player Performance Forecasting](https://github.com/davislaroque/Sports_Prediction_Model): reproducible modeling and chronological evaluation.
- [NBA API Player Lookup](https://github.com/davislaroque/NBA-API-PLAYER-LOOKUP): reusable schedule and box-score queries.
