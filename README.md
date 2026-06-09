# Basketball Intelligence Platform MVP

Version 0.1 validates one workflow: coaches upload game data, store historical player stats, query trends, and see basic rule-based insights.

## Stack

- Next.js App Router, React, TypeScript
- Next.js API routes
- PostgreSQL, Prisma
- Recharts
- Tailwind CSS

## Architecture

The schema is sport-neutral from day one:

- `Organization`
- `Team`
- `Player`
- `Game`
- `StatType`
- `PlayerGameStat`
- `DataUpload`

Basketball stats are seed data and UI defaults, not fixed columns. A player stat is stored as `Player + Game + StatType + Value`, so future football, soccer, volleyball, and baseball metrics can be added without schema redesign.

Upload ingestion follows this path:

`Data Source -> Parser -> Normalized Stats -> Database`

CSV parsing is implemented. PDF and OCR/photo uploads are represented in the parser abstraction but intentionally not implemented in MVP 0.1.

## Local Setup

1. Install dependencies:

```bash
npm install
```

2. Create `.env`:

```bash
cp .env.example .env
```

3. Update `DATABASE_URL` in `.env` for your PostgreSQL instance.

4. Run migrations:

```bash
npm run prisma:migrate -- --name init
```

The repository also includes the initial SQL migration under `prisma/migrations`.

5. Seed demo data:

```bash
npm run db:seed
```

The seed creates one basketball team, 12 players, 20 games, realistic stat history, upload records, and enough trends to exercise the Query Builder and insight engine.

6. Start development:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## CSV Upload Format

CSV uploads should use one stat per row:

```csv
date,opponent,location,jerseyNumber,statName,value
2026-02-20,Central Prep,Home,1,Points,21
2026-02-20,Central Prep,Home,1,Rebounds,6
```

Supported player matching:

- `jerseyNumber`
- or `playerFirstName` and `playerLastName`

See `sample-game-upload.csv` for a small upload file.

## MVP Pages

- `/` Dashboard: roster, recent uploads, recent insights, team/player creation, CSV upload
- `/players/[playerId]`: season averages, last 3/5 games, trend chart, insights, historical log
- `/query`: polished filter-based Query Builder for player/team, metric, and timeframe

## API Routes

- `POST /api/teams`
- `POST /api/players`
- `POST /api/uploads`
- `POST /api/query`
- `POST /api/insights`
- `GET /api/players/[playerId]`
- `GET /api/bootstrap`

## Deliberately Out of Scope

No authentication, AI recommendations, fatigue scores, injury prediction, OCR implementation, camera uploads, notifications, wearables, or workout plans are included in this MVP.
