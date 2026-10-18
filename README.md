# Boundary Cricket

A responsive HTML/CSS/JavaScript cricket scoring prototype designed for Android WebView.

## Files
- `index.html` — app structure
- `styles.css` — mobile-first design
- `index.js` — manual scoring, scorecards, international match feed adapter

## Run
Open `index.html` in a browser, or place these three files in an Android WebView project's assets folder and load `file:///android_asset/index.html`.

## Manual scoring
- Runs 0–6, wides, no-balls, byes, leg-byes, wicket, undo
- Striker/non-striker and bowler fields
- Automatic runs, wickets, overs, run rate, batter figures, bowler figures, extras, recent balls and scorecard tables
- Start a new match, swap batters, change innings, export scorecard JSON

## Real international scores
No live-score API key or verified feed is included. The home page uses clearly marked placeholder/demo cards until you connect a real provider.

In `index.js`, configure:
```js
const API_CONFIG = {
  enabled: true,
  endpoint: "https://YOUR-SERVER.example/api/international-matches",
  pollMs: 15000
};
```

Your endpoint should return JSON like:
```json
{
  "matches": [
    {
      "id": "match-123",
      "international": true,
      "status": "live",
      "format": "T20,ODI,TEST",
      "series": "International series",
      "venue": "Ground, City",
      "team1": {"name": "Team A", "flag": "🏏", "score": "120-3", "overs": "14.2 ov"},
      "team2": {"name": "Team B", "flag": "🏏", "score": "—", "overs": ""},
      "result": "Live"
    }
  ]
}
```
Allowed statuses: `live`, `result`, `upcoming`. Return only international fixtures (`international: true`) if the home page should show international matches only.

## Important API notes
- A frontend cannot safely hide a private API key. Use a small backend/proxy and keep the provider key on the server.
- Refreshing the page every second does not mean the score provider updates every second. This sample checks the display clock every second and polls a configured feed at a minimum of 15 seconds to avoid unnecessary requests/rate limits. Set the interval according to your provider's rules.
- Google Search is not a public general-purpose live cricket score API. Use a licensed cricket data provider and follow its terms. Do not scrape Google Search results.
- This is a front-end prototype, not a signed APK. To publish on Android, create an Android Studio WebView wrapper or package it with an appropriate trusted web-to-app workflow.
