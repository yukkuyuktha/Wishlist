# Wishlist

A personal wishlist manager to save and organize products from any shopping site — Amazon, Myntra, Flipkart, or anywhere else.

**Live app:** https://yukkuyuktha.github.io/Wishlist/

## The problem

When you save items on different shopping sites, they get buried over time. This app gives you one place to track everything you want to buy, organized by category.

## Features

- Save any product with a URL, title, category, and priority
- Categories: Clothes, Footwear, Accessories, Electronics, Books, Home, Beauty, Sports, Other
- Priority levels: Someday / Want it / Need it!
- Search across all your saved items
- Mark items as bought
- Stats dashboard (total items, bought, high priority)
- Works on mobile and desktop
- Syncs across all your devices via Supabase

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript (no frameworks) |
| Database | Supabase (PostgreSQL) |
| Hosting | GitHub Pages |

## How to use

1. Open the [live app](https://yukkuyuktha.github.io/Wishlist/)
2. Create a free account at [supabase.com](https://supabase.com)
3. Create a new project and run this SQL in the SQL Editor:

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

4. Go to Project Settings → API, copy your Project URL and anon public key
5. Paste them into the app and click Connect

Your data is stored in your own Supabase project — not shared with anyone.

## Architecture

The app is a single HTML file. JavaScript makes REST API calls to the user's Supabase project to read and write wishlist items. GitHub Pages hosts the static file for free.

## Future improvements

- [ ] Fetch product images automatically from product URLs
- [ ] Browser extension for one-click saving
- [ ] Price change tracking
- [ ] Supabase Auth for multi-user support
