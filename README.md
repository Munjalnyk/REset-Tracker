DEMO Website

https://reset-tracker-test.vercel.app/

Password = "reset110"

# Reset

A single-file, offline-first fitness and nutrition tracker for people losing weight with a volumetric, high-protein Indian-friendly meal plan. Built around a 7-day meal rotation, grocery budget tracking with price analytics, and weight/measurement logging — all in one `index.html` with no build step, no server, and no account.

![license](https://img.shields.io/badge/license-MIT-green)
![stack](https://img.shields.io/badge/stack-vanilla%20JS-f7df1e)
![hosting](https://img.shields.io/badge/hosting-static-blue)

---

## Why this exists

Most fitness apps are bloated, require sign-ups, sell your data, or lock the features you actually need behind a subscription. This is the opposite — one HTML file, your data stays on your device, you host it wherever you want.

It was built for a specific person (35 kg to lose, studying, driving Uber part-time, vegetarian-plus-chicken-plus-eggs diet, based in Sydney) but the structure generalizes to anyone who wants a no-nonsense tracker they fully control.

## Features

**Today tab** — the daily execution view
- Check off each meal as you eat it (progress bar fills)
- Tap water cells — each one is 300 ml, 10 total = 3 L daily target
- Movement toggles for morning walk, evening walk, strength session
- Live stats: current weight, kg to goal, % progress, streak

**Week tab** — the plan
- 7-day meal rotation with timing, portions, and ingredient detail
- Every meal shows estimated cost ($) based on your logged shopping prices
- Daily water-timing chart and volumetric eating rules

**Recipes tab** — the building blocks
- 10 core recipes (besan chilla, chicken tikka, rajma, chana masala, paneer bhurji, moong dal soup, protein pancakes, cucumber raita, sprouts chaat, tandoori chicken)
- Expandable cards with ingredients, method, macros, and substitution tips

**Shop tab** — the grocery list
- Categorized shopping list (Proteins / Legumes & Grains / Vegetables / Fruits / Pantry)
- Check items off as you buy them
- Per-item expandable input: actual quantity bought + price paid
- Trip date picker (backdate a shop if needed)
- Live budget comparison ($110 AUD default, configurable)
- "Save this trip" commits to history and clears for the next shop

**$ Spending tab** — the analytics
- Total spent all-time, number of trips, average per week
- Cost-per-kg-lost metric (gets interesting over time)
- This week vs last week comparison with trend badge
- Weekly spend bar chart
- Category breakdown doughnut chart
- Top items by spend (your 8 biggest line items)
- Full trip history with delete buttons
- **Estimated meal cost** — per-day and per-week, calculated from the plan using your actual logged prices when available, falling back to supermarket defaults otherwise

**Body tab** — the progress
- Weight logging with live line chart vs goal
- Waist and chest measurement tracking
- Full history timeline
- Save today's log as a readable `.txt` file
- Export all data as JSON (backup)
- Import JSON (restore from backup)
- Manual lock-app button

**Auth gate**
- Password screen on load (SHA-256 hashed, client-side)
- "Remember this device for 365 days" — sign in once on your iPhone home-screen icon, never again
- Pre-login "Restore from backup JSON" — recover your data even if storage is wiped, no unlock needed

---

## Screenshots

*(Add screenshots to a `screenshots/` folder in your repo and reference them here)*

---

## Tech stack

- **Vanilla HTML, CSS, JavaScript** — no framework, no build step
- **Chart.js** via CDN for charts
- **Google Fonts** (Fraunces + Geist + Geist Mono) via CDN
- **localStorage** for persistence
- **Web Crypto API** for password hashing (SHA-256)

Total size: one HTML file, ~110 KB. No external dependencies installed, no `package.json`, no `npm install`.

---

## Quick start

### Option A — Run it locally (no hosting)

1. Clone or download this repo
2. Open `index.html` in any browser
3. Default passphrase: `reset110`

Data persists in `localStorage` tied to that exact file path on disk.

### Option B — Deploy to Vercel (recommended)

1. Fork this repo on GitHub
2. Go to [vercel.com](https://vercel.com) → New Project → import your fork
3. Framework preset: **Other** (it's a static file, no build config needed)
4. Deploy — you'll get a URL like `reset-yourname.vercel.app`

Every push to `main` redeploys automatically.

### Option C — Deploy to GitHub Pages (also free)

1. Fork this repo
2. Repo settings → Pages → Source: Deploy from branch → `main` → `/` (root) → Save
3. Wait ~1 minute, your URL is `https://yourname.github.io/reset/`

### Option D — Netlify Drop (zero-config)

1. Download `index.html`
2. Drag it onto [app.netlify.com/drop](https://app.netlify.com/drop)
3. Done — you get a URL instantly

---

## Install as an iPhone app (no App Store)

Once deployed to any URL:

1. Open the URL in **Safari** on iPhone (not Chrome — iOS Add-to-Home-Screen only works properly from Safari)
2. Tap the Share button at the bottom
3. Scroll down → **Add to Home Screen**
4. Name it "Reset" → Add

You now have an icon on your home screen. Tapping it launches the app full-screen (no Safari UI), and thanks to the "Remember this device" checkbox, you won't see the password screen again for 365 days.

Same flow works on Android via Chrome ("Add to Home screen").

---

## Customization

### Change the passphrase

The default passphrase is `reset110`. To change it:

1. Open any browser's DevTools console (F12 → Console tab)
2. Run this, replacing `your-new-password`:

```js
crypto.subtle.digest('SHA-256', new TextEncoder().encode('your-new-password'))
  .then(h => console.log(Array.from(new Uint8Array(h)).map(b => b.toString(16).padStart(2,'0')).join('')))
```

3. Copy the 64-character hash from the console
4. Open `index.html` in a text editor
5. Find `const AUTH_HASH = '...';` near the top of the `<script>` tag
6. Replace the value with your new hash
7. Commit and push — Vercel auto-redeploys

**Important**: this is a client-side cosmetic lock. The HTML source is visible to anyone who visits the URL, so:
- A short or common passphrase can be brute-forced from the hash
- Pick something long and non-obvious (e.g. `blue-kangaroo-studies-hard-2027`) for real protection
- This is not a substitute for real auth — it's a soft lock that keeps casual snoopers out of your personal data

### Change the meal plan

All meal data lives in the `MEALS` object near the top of the `<script>` tag:

```js
const MEALS = {
  Mon: [
    { time: '08:00 · Breakfast', name: 'Your breakfast', detail: 'what it contains' },
    // ... 5 meals per day
  ],
  Tue: [ /* ... */ ],
  // Wed, Thu, Fri, Sat, Sun
};
```

Each day should have 5 meals (breakfast, snack 1, lunch, snack 2, dinner) for the timing chart to line up, but the code doesn't enforce this — you can have as many or few as you want per day.

### Change the grocery list

Edit the `GROCERIES` array:

```js
const GROCERIES = [
  { cat: 'Proteins', items: [
    { n: 'Chicken breast', q: '1.5 kg' },
    // ...
  ]},
  // more categories
];
```

Categories and items render in the order they appear. Each item needs a name (`n`) and planned quantity (`q`, displayed only).

### Enable meal cost estimation for new meals

If you add a new meal to `MEALS`, and you want it to show a cost badge, add an entry to `MEAL_INGREDIENTS`:

```js
const MEAL_INGREDIENTS = {
  'Your meal name': [
    { id: 'Proteins:Chicken breast', g: 150 },   // 150 grams of chicken
    { id: 'Vegetables:Onions', g: 50 },          // 50 grams of onion
    { id: 'Fruits:Apples', u: 1 },               // 1 unit (piece)
  ],
};
```

The `id` must match `Category:ItemName` exactly as defined in `GROCERIES`. Use `g` for items measured in grams/ml, `u` for items counted in pieces.

Update `GROCERY_UNITS` if you add new ingredient items — it tells the price-learning algorithm how many grams/units come in a typical purchase of each item.

### Change default fallback prices

`FALLBACK_PRICES` has a per-gram or per-unit price for every ingredient. These are used when you haven't logged a real shopping trip yet. Edit values to match your country's supermarket prices:

```js
const FALLBACK_PRICES = {
  'Proteins:Chicken breast': { perGram: 0.013 },  // AUD $13/kg = $0.013/g
  'Proteins:Eggs':           { perUnit: 0.55 },   // AUD $0.55 each
  // ...
};
```

Once you start logging real trips with prices, the app learns from your actual data and uses those prices instead.

### Change the weekly budget

Search for `const budget = 110;` (appears twice — once in `renderGroceries`, once in `updateTripItem`). Change to your target AUD/USD/INR etc.

### Change the color theme

All colors are CSS custom properties in the `:root` block at the top of the `<style>` tag:

```css
:root {
  --bg: #0f0d0b;          /* background */
  --accent: #e07a2c;      /* orange accent */
  --ink: #f2ebdd;         /* text */
  /* ... */
}
```

Replace the hex values to retheme. The color scheme is a warm dark-earth palette — swap `--accent` to change the dominant brand color.

### Change the starting weight and goal

Search for `110` and `75` in the code — these appear in a few places (hero section, progress calculation, stats). Replace with your values.

---

## Data model

All data stored in `localStorage` under keys prefixed with `reset:`:

| Key | Contents |
|-----|----------|
| `reset:weights` | Array of `{ date, weight }` |
| `reset:measurements` | Array of `{ date, waist, chest }` |
| `reset:tripHistory` | Array of saved shopping trips with items, prices, totals |
| `reset:currentTrip` | In-progress trip (items and prices being entered) |
| `reset:tripDate` | Date of the current trip |
| `reset:groceries` | Array of checked grocery item IDs in the current trip |
| `reset:meals:YYYY-MM-DD` | Array of meal indices completed for that date |
| `reset:water:YYYY-MM-DD` | Number of 300 ml water units (0–10) for that date |
| `reset:toggles:YYYY-MM-DD` | Object of movement toggles `{ walk_am, walk_pm, strength }` for that date |
| `reset:auth:remembered` | Device token + expiry timestamp for skipping login |

The **Export all (JSON)** button in the Body tab gives you a complete dump of every key above (except the auth token) as a single file. Back this up weekly.

---

## Backup and restore

### Before you lose anything

In the app: **Body tab → Export all (JSON)**. Saves a file like `reset-backup-2026-04-19.json` to your Downloads / Files app. Email it to yourself, upload to iCloud Drive, whatever. Do this weekly.

### When you lose something

On the login screen, there's a **Restore from backup JSON** button *before* you even enter the passphrase. Tap it, pick your backup file, data is merged back in. You don't need to unlock the app to recover data — this is intentional for disaster recovery.

Alternatively, unlock normally and use **Body tab → Import JSON**.

### Things that can cause data loss

- Clearing Safari's "Website Data" in Settings
- iOS's periodic purge of website data for sites you haven't visited in 7+ weeks
- Uninstalling the home-screen icon and re-adding from a different URL
- Switching phones or browsers
- Deploying to a new domain (localStorage is per-origin)

None of these touch your JSON backup — that's why it exists.

---

## Development

There's nothing to install or build. Edit `index.html` in any text editor. Reload the browser.

### File structure

```
reset/
├── index.html      # The entire app (HTML + CSS + JS in one file)
├── README.md       # This file
└── screenshots/    # (optional) screenshots for README
```

### Debugging

Open browser DevTools → Console to see any errors. All localStorage keys are prefixed `reset:` so you can inspect the full state in Application → Local Storage.

To test the auth gate without waiting 365 days, delete the `reset:auth:remembered` key in DevTools and reload.

---

## Roadmap / ideas

Things that aren't built but would be reasonable additions:

- **Step counter integration** via the [`Pedometer`](https://developer.apple.com/documentation/coremotion/cmpedometer) API (iOS Safari support limited)
- **Nutrition macro tracking** — protein/carb/fat totals per day, not just completion
- **Photo progress gallery** — monthly body photos stored as blobs
- **Notification reminders** — service worker + `Notification.requestPermission()` for water/meal prompts (tricky on iOS Safari)
- **Recipe substitutions** — swap paneer for tofu, chicken for legumes, etc.
- **Multiple user profiles** — family sharing mode with namespaced storage keys
- **CSV export** — for people who want to analyze in Excel/Sheets
- **Weight trend smoothing** — 7-day moving average line on the weight chart
- **BMI and body-fat calculators** — input height once, see BMI over time

PRs welcome if you build any of these for your own fork.

---

## FAQ

**Is my data sent anywhere?**
No. Everything is in your browser's localStorage. The only network requests are to Google Fonts (for typography), cdnjs (for Chart.js), and whatever host serves the HTML itself. Look at the Network tab in DevTools — zero requests to any backend, zero analytics, zero tracking.

**Does the password actually protect my data?**
It prevents someone with your URL from seeing your logs in their own browser. It does **not** encrypt your localStorage — if someone has physical access to your unlocked device and opens DevTools, they can read your data directly. The password is a soft UI lock, not cryptographic security. For most personal fitness tracking, that's fine.

**Why not use a real backend with real auth?**
Then it's not one file anymore. This project's entire philosophy is: no server, no account, no dependency that can disappear. Every additional piece of infrastructure is a future way for your tracker to break.

**Can I use this for something that's not weight loss?**
Yes — swap out the meal plan, rename the goal, change the color palette. The structure (daily checkoffs + weekly planning + price tracking + progress logging) works for bulking, maintenance, any diet philosophy, habit tracking, etc.

**Does it work offline?**
Yes, once the page has loaded once. No network is needed after initial load — all state is local. Could be made fully offline-first with a service worker; PRs welcome.

**Will it work on Android / Windows / Linux?**
Yes — anything with a modern browser. The "Add to Home Screen" experience varies (Chrome on Android is similar; desktop browsers usually just bookmark), but the app itself works everywhere.

**Why the orange-on-brown palette?**
Looked different enough from every other fitness app. Also works in low light while using the app at night without burning retinas. Change it in CSS vars.

---

## Credits and license

Built through a conversation with Claude (Anthropic). The original use case and meal plan were tailored to one person's specific situation, then generalized into this template.

**License**: MIT. Fork it, modify it, sell it, whatever — just don't blame me if you eat 50 g of besan a day for a year and decide you hate chilla.

## A note on the meal plan itself

The included 7-day plan is built for:
- ~1800 kcal/day
- 150–170 g protein/day
- Proteins from chicken, eggs, cottage cheese, Greek yogurt, paneer, whey, and legumes
- No fish, no red meat, no tofu (customize as needed)
- Indian / South Asian cuisine lean

It's designed for someone at BMI 35+ trying to reach a healthy weight over 10–14 months. **This is not medical advice.** Talk to a doctor before starting any significant diet, especially if you have health conditions. Bloodwork before a cut (fasting glucose, HbA1c, lipids, thyroid, vitamin D, B12) is a good idea — it gives you a baseline and rules out things that'd sabotage progress.

Calorie and protein needs vary by body composition, activity, age, sex, and medical context. 1800 kcal is a reasonable deficit for someone at 100+ kg with low-to-moderate activity; it's too low for a lean athlete and too high for a sedentary 55 kg person. Adjust based on your own maintenance.

If anything in the app stops working, you're not stuck — your data is in `localStorage` (inspectable in DevTools) and your JSON backup is human-readable. You can always walk away with your data intact.
