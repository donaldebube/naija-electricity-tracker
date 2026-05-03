# NaijaLight ⚡

> A crowdsourced electricity status tracker for Nigeria — built for real people, in real time.

NaijaLight lets you check whether an area in Nigeria currently has power before you travel there, plan your day, or make business decisions. No official PHCN/DisCo data — just honest, community-reported updates from residents, visitors and business owners across all 36 states and the FCT.

---

## Why this exists

Anyone who has lived in Nigeria knows the frustration: you travel 2 hours to a meeting, arrive, and the generator is running because there's been no light for 3 days. Or you're planning a trip to visit family in Ido-Ekiti and want to know whether to pack a power bank or not.

NaijaLight solves this with a simple idea — if enough people report what's happening in their area, everyone benefits.

---

## Features

- **207+ areas covered** across all 36 states + FCT, from major cities to smaller LGAs
- **Real-time community reports** — residents and visitors submit status updates (power on / off / unstable)
- **DisCo information** — each area shows its electricity distribution company, feeder line and contact number
- **Area summaries** — plain-English description of each area's typical power situation
- **Report confidence score** — shows how much community reports agree with each other
- **Upvoting** — helpful reports rise to the top
- **Spam protection** — math CAPTCHA + rate limiting (3 reports/area/hour) + duplicate detection
- **Zero dependencies** — single HTML file, no build tools, no frameworks
- **Free to host** — works on Netlify, GitHub Pages, or any static host

---

## How it works

### Area data (static)
All 207+ areas are hardcoded as a JSON array inside the HTML file. Each entry includes:
- Area name, LGA, state
- DisCo (e.g. EKEDC, IBEDC, AEDC)
- Feeder line name
- Average daily supply hours
- A written summary of the area's power situation

This means the app loads instantly with no API calls — search and area info are fully offline-capable.

### Community reports (crowdsourced)
Reports are the live layer on top of the static data. When a user submits a report:

```
User submits report
       │
       ▼
Is Supabase connected?
       │
  ┌────┴────┐
  YES       NO
  │         │
  ▼         ▼
POST to    Save to
Supabase   localStorage
database   (device only)
  │
  ▼
All users worldwide
can now read it
```

**Local mode** (default): Reports save to the browser's `localStorage`. They persist across page refreshes but are only visible on that device.

**Shared mode** (with Supabase): Reports go to a free cloud Postgres database. Anyone visiting the app from any device sees everyone's reports.

### Spam protection
Three layers protect the database from abuse:

| Layer | What it does |
|---|---|
| Math CAPTCHA | Random arithmetic question (add / subtract / multiply) required before each submission |
| Rate limiting | Max 3 reports per area per hour per device, tracked in localStorage |
| Duplicate detection | Blocks identical messages submitted within 10 minutes for the same area |

---

## Tech stack

| Layer | Technology | Cost |
|---|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript | 
| Fonts | Google Fonts (Syne + DM Sans) | 
| Database | Supabase (Postgres) |
| Hosting | Netlify / GitHub Pages | 


## Project structure

```
naija-light.html
│
├── <style>          CSS — layout, components, dark-mode variables
├── <body>
│   ├── <nav>        Top navigation bar with brand + Connect DB button
│   ├── Modal        Supabase configuration modal
│   ├── .hero        Search bar + quick area chips
│   └── .main
│       ├── .result-area
│       │   ├── .status-card     Live status, stats, confidence bar
│       │   ├── .card            Area summary text
│       │   ├── .reports-card    Community reports list
│       │   └── .form-card       Submit report form (with CAPTCHA)
│       └── .sidebar
│           ├── DisCo info
│           └── Simulated history
└── <script>
    ├── Supabase config & fetch helpers
    ├── DISCOS object   DisCo metadata
    ├── AREAS array     All 207+ area definitions
    ├── localStorage helpers (reports + rate limiting)
    ├── Render functions (reports, history, DisCo info)
    ├── Search logic (exact match → partial → suggestions)
    ├── CAPTCHA generator
    ├── Rate limiter + duplicate checker
    └── Submit handler
```

---

## Roadmap

These are features worth building as the project grows:

- [ ] Add remaining LGAs and towns to reach 500+ areas
- [ ] User accounts — let people build reputation as trusted reporters
- [ ] Push notifications — get alerted when power comes back in your saved area
- [ ] 7-day supply history chart per area
- [ ] Mobile app (PWA — installable from browser, no app store)
- [ ] DisCo outage notice scraper (EKEDC, AEDC etc. post on Twitter/X)
- [ ] API endpoint — let other developers query NaijaLight data
- [ ] Admin dashboard — review and flag suspicious reports

---

## Contributing

Contributions are welcome — especially:

- **New areas**: Add towns, LGAs and suburbs that are missing
- **Area info corrections**: If the DisCo, feeder or description for an area is wrong
- **Bug fixes**: Open an issue or submit a PR

Please keep the single-file architecture. The goal is that anyone can download one `.html` file and have a working app.

---

## Disclaimer

NaijaLight data is entirely crowdsourced and community-reported. It does not represent official information from any DisCo, NERC, or government agency. Supply hours and status indicators are estimates based on historical patterns and user reports — they may not reflect current conditions. Always verify with local contacts for critical decisions.

---

## License

MIT — free to use, modify and distribute. If you build something with it, a mention would be appreciated.

---

*Built with the belief that information sharing is one of the most practical things technology can do for everyday Nigerians.*