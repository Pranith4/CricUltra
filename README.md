# 🏏 CricTrack - Cricket Scoring App
## Architecture: Next.js → Google Apps Script → Google Sheets

No server needed. No Google Cloud Console. No service accounts.
**Just Apps Script deployed as a Web App.**

---

## 🏗️ Architecture

```
┌─────────────────┐    NEXT_PUBLIC_API_URL    ┌──────────────────────────┐
│  Vercel (Next)  │ ─────────────────────────▶│  Apps Script Web App     │
│  (Frontend)     │ ◀─────────────────────────│  (Code.gs = Backend API) │
└─────────────────┘       JSON responses       └──────────────────────────┘
                                                           │
                                                           ▼
                                               ┌──────────────────────────┐
                                               │   Google Sheets          │
                                               │   (Database / Storage)   │
                                               └──────────────────────────┘
```

---

## 🚀 Setup in 4 Steps

### Step 1 – Google Sheet
1. Create a new [Google Spreadsheet](https://sheets.google.com)
2. Copy the Sheet URL / ID (you don't need it in the app, Apps Script reads its own parent sheet)

### Step 2 – Apps Script Backend
1. In your Sheet, go to **Extensions → Apps Script**
2. Delete any existing code
3. Paste the entire contents of **`apps-script/Code.gs`** from this repo
4. Click **Save** (💾)
5. Click **Deploy → New Deployment**
   - Type: **Web App**
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Click **Deploy** and **Authorize** when prompted
7. **Copy the Web App URL** (looks like: `https://script.google.com/macros/s/AKfy.../exec`)

### Step 3 – Deploy to Vercel
1. Push this whole repo to **GitHub**
2. Import it at [vercel.com](https://vercel.com)
3. Add one **Environment Variable**:
   ```
   NEXT_PUBLIC_API_URL = https://script.google.com/macros/s/YOUR_ID/exec
   ```
4. Deploy!

### Step 4 – Initialize Sheets
1. Open your deployed app
2. Go to **Profile tab → Apps Script Setup**
3. Tap **Initialize All Sheets**
4. Done! All 11 sheet tabs are created with headers ✅

---

## 📁 Files

```
cricket-app/
├── apps-script/
│   └── Code.gs          ← PASTE THIS into Apps Script editor
├── lib/
│   └── api.js           ← Thin client that calls your Apps Script URL
├── pages/
│   ├── index.js         ← Home (live matches, stats)
│   ├── matches/         ← Match list, create, scorecard
│   ├── score/           ← Live ball-by-ball scoring pad
│   ├── players/         ← Player profiles & stats
│   ├── teams/           ← Team management
│   ├── tournaments/     ← Tournaments & points table
│   ├── profile.js       ← Setup & config
│   └── api/proxy.js     ← Optional SSR proxy (not required)
├── components/
│   └── BottomNav.js     ← Mobile nav bar
└── styles/
    └── globals.css      ← All styles & design tokens
```

---

## 📊 Google Sheets (11 tabs auto-created)

| Sheet | Contents |
|-------|----------|
| Players | Player profiles, career stats |
| Teams | Team info, W/L records |
| Tournaments | Tournament details |
| Matches | All matches |
| Innings | Per-innings summary |
| Batting | Batting scorecards |
| Bowling | Bowling figures |
| LiveScoring | Real-time scoring state |
| TeamPlayers | Team ↔ Player links |
| TournamentTeams | Points table |
| BallLog | Every ball ever bowled (for undo) |

---

## 🔄 Apps Script API Endpoints

Apps Script handles all CRUD via `?resource=X&method=Y&id=Z`:

| Resource | Methods | Description |
|----------|---------|-------------|
| `matches` | GET, POST, PATCH | Match CRUD |
| `live` | GET | Live scoring state |
| `ball` | POST | Record a ball |
| `ball/undo` | POST | Undo last ball |
| `innings/end` | POST | End innings |
| `match/end` | POST | End match |
| `scorecard` | GET | Full scorecard |
| `players` | GET, POST, PATCH | Player CRUD |
| `teams` | GET, POST, PATCH | Team CRUD |
| `tournaments` | GET, POST, PATCH | Tournament CRUD |
| `tournament/standings` | GET | Points table |
| `leaderboard` | GET | Batting/bowling leaders |
| `stats/summary` | GET | Dashboard stats |
| `admin/init` | POST | Initialize all sheets |

---

## 📱 PWA Install

**iPhone:** Safari → Share → Add to Home Screen  
**Android:** Chrome → Menu → Add to Home Screen / Install App

---

## 🛠️ Local Dev

```bash
npm install

# .env.local
NEXT_PUBLIC_API_URL=https://script.google.com/macros/s/YOUR_ID/exec

npm run dev
# → http://localhost:3000
```

> ⚠️ Apps Script Web Apps don't support CORS by default.
> During local dev, use the Profile page to test your URL, or deploy to Vercel.

---

## ✨ Features
- Live ball-by-ball scoring with undo
- Auto batting/bowling scorecards  
- Player career stats & profiles
- Team management & W/L records
- Tournaments with Points Table & NRR
- PWA (installable on iOS & Android)
- Zero backend infra cost
