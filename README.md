# 🛍️ Wishlist Manager

A cross-device personal shopping wishlist web app. Save product links from any website, organise them by category, set priorities, and access the same list from your phone and laptop in real time.

🔗 **Live demo:** [yukkuyuktha.github.io/Wishlist](https://yukkuyuktha.github.io/Wishlist/)

---

## The Problem

I kept saving product links across different apps — notes, browser bookmarks, WhatsApp saved messages — and forgetting about them. There was no single place to organise them by category, set a priority, or check them off once bought. So I built one.

---

## What It Does

- 📂 **9 categories** — Clothes, Footwear, Accessories, Electronics, Books, Home, Beauty, Sports, Other
- 🔴 **Priority levels** — Someday / Want it / Need it! — items auto-sort by priority
- 💰 **Price tracking** — save the expected price alongside the link
- 📝 **Notes** — size, colour, "wait for sale" reminders
- ✅ **Mark as Bought** — moves purchased items to a separate section instead of deleting them
- 🔍 **Search** — find any saved item instantly across all categories
- 📊 **Stats** — total items, bought count, urgent items at a glance
- 🌐 **Cross-device sync** — save on phone, see it on laptop instantly (via Supabase)
- 📱 **Mobile-first** — designed and tested on both phone and desktop

---

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| Frontend | HTML, CSS, JavaScript | Single file, no build step, works in any browser |
| Database | Supabase (PostgreSQL) | Free cloud database with REST API — enables cross-device sync |
| API | Supabase REST API | HTTP calls from the browser to read and write data |
| Hosting | GitHub Pages | Free static hosting — deploys on every push |
| Local storage | localStorage | Saves Supabase credentials so you only enter them once per device |

---

## How It Works

```
Browser (HTML file)
      │
      │  HTTP POST   → Save new item
      │  HTTP GET    → Load all items
      │  HTTP PATCH  → Update item (e.g. mark as bought)
      │  HTTP DELETE → Remove item
      ▼
Supabase REST API
      │
      ▼
PostgreSQL Database (wishlist table)
```

1. On first open, you enter your Supabase Project URL and anon key once — saved to localStorage
2. The app loads all your items via a GET request to Supabase
3. Every action (add, update, delete) sends the matching HTTP request to Supabase
4. Open the same file on any device with the same credentials — same data, always in sync

**Why not just use localStorage for data?**
localStorage is sandboxed per browser. Data saved in Chrome on your laptop doesn't exist in Safari on your phone. Supabase is a cloud database — all devices read and write to the same place.

**What is optimistic UI?**
When you save an item, it appears on screen instantly without waiting for Supabase to respond. The API call happens in the background. If it fails, the item is removed and an error is shown. This makes the app feel fast even on slow connections.

---

## Setup (2 minutes)

### 1. Create a free Supabase project
Go to [supabase.com](https://supabase.com) → Sign up → New Project

### 2. Run this SQL in the Supabase SQL Editor

```sql
create table wishlist (
  id text primary key,
  title text,
  url text,
  cat text,
  price text,
  note text,
  prio text,
  bought boolean default false,
  ts bigint,
  created_at timestamptz default now()
);
alter table wishlist enable row level security;
create policy "allow all" on wishlist for all using (true) with check (true);
```

### 3. Get your credentials
Go to **Project Settings → API** and copy:
- Project URL → `https://xxxx.supabase.co`
- anon public key → `eyJhbGci...`

### 4. Open the app
Visit [yukkuyuktha.github.io/Wishlist](https://yukkuyuktha.github.io/Wishlist/), paste your credentials, and click **Connect**.

Done. Your wishlist is now live and syncs across all devices.

---

## Key Technical Decisions

**Single-file architecture**
The entire app — HTML structure, CSS styles, and JavaScript logic — lives in one `index.html` file. No build tools, no npm install, no dependencies to manage. Open the file and it works.

**Supabase over Firebase**
Both are free cloud backends. Supabase uses standard PostgreSQL and a plain REST API — easier to reason about and debug. Firebase uses a proprietary NoSQL structure with a custom SDK.

**Flat data model**
Each wishlist item is a single row in the database with all its properties (title, url, category, priority, etc.) stored as columns. No nested objects, no joins needed. Simple to query, simple to update.

**Row Level Security**
The Supabase policy `using (true)` allows all operations with the anon key. For a personal private app this is fine. For a multi-user app you would restrict access per authenticated user.

---

## What I Learned

- How to connect a frontend app to a cloud database using REST API calls
- The difference between client-side storage (localStorage) and server-side storage (Supabase)
- How optimistic UI updates work and why they improve perceived performance
- How to deploy a static web app using GitHub Pages
- How to debug cross-device sync issues by separating UI logic from data storage

---

## Built With

- [Supabase](https://supabase.com) — backend database
- [Google Fonts](https://fonts.google.com) — DM Sans + Instrument Serif
- [GitHub Pages](https://pages.github.com) — hosting

---

*Built by [Yuktha Mangalapally](https://github.com/yukkuyuktha)*
