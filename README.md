# RMA Schedule — Deployment Guide

A shared event-scheduling + SOC builder for your team. Works on any device
through the browser. Data is stored in a Supabase database so everyone sees the
same schedule, with live updates across devices.

There is **no build step** — it's plain static files. You'll do three things:

1. Create the **database** (Supabase) — ~5 min
2. Put the code on **GitHub** — ~3 min
3. Connect GitHub to **Netlify** to host it — ~3 min

---

## 1. Database — Supabase

1. Go to **https://supabase.com** → sign up (free) → **New project**.
   - Give it a name and a strong **database password** (you can ignore this later).
   - Pick the region closest to your team. Wait ~2 min for it to finish.

2. In the left sidebar open **SQL Editor → New query**. Open the file
   **`supabase-schema.sql`** from this project, copy everything, paste it in,
   and click **Run**. You should see "Success".

3. Create your single shared team login:
   **Authentication → Users → Add user → Create new user**.
   - **Email:** anything you like, e.g. `team@yourcompany.com`
   - **Password:** this becomes your **team password** — share it with staff.
   - Tick **Auto Confirm User** if shown.

4. Get your keys: **Project Settings → API**. You'll need:
   - **Project URL**
   - the **anon / public** key (the long one — safe to share publicly)

5. Open **`config.js`** in this project and fill in the three values:
   ```js
   window.RMA_CONFIG = {
     SUPABASE_URL:      "https://xxxx.supabase.co",
     SUPABASE_ANON_KEY: "eyJhbGciOi...",          // anon/public key
     TEAM_EMAIL:        "team@yourcompany.com"      // the user you made in step 3
   };
   ```

> The first time anyone signs in, the app automatically uploads its current
> schedule into the database — your existing sample/real data is migrated for you.

---

## 2. Code — GitHub

1. Create a free account at **https://github.com** if you don't have one.
2. Click **New repository** → name it e.g. `rma-schedule` → **Private** → Create.
3. Upload these files (drag-and-drop into GitHub's "uploading an existing file"
   screen, or use the Desktop app):
   - `index.html`
   - `support.js`
   - `config.js`   ← with your real values filled in
   - `netlify.toml`
   - `supabase-schema.sql` (optional, just for reference)
   - `README.md` (optional)

---

## 3. Hosting — Netlify

1. Sign up at **https://netlify.com** (you can log in with GitHub).
2. **Add new site → Import an existing project → GitHub** → pick your repo.
3. Leave build settings empty (the `netlify.toml` already handles it) → **Deploy**.
4. After ~30 seconds you get a live URL like `https://rma-schedule.netlify.app`.
   Share it with your team.
5. (Optional) **Domain settings → Add custom domain** to use your own address.

---

## Day-to-day

- Open the site → enter **your name** + the **team password** → you're in.
- Every change saves to the cloud automatically (watch the dot in the bottom-left:
  green = saved). Changes appear on teammates' screens live.
- Each SOC shows **who created and last edited it**.

## Changing the team password later

Supabase → **Authentication → Users** → click the team user → reset password.
No code change or redeploy needed.

## Updating the app

Edit the files on GitHub (or re-upload) → Netlify redeploys automatically in
under a minute.

## Troubleshooting

- **Stuck on "Connecting…"** → your `config.js` values are wrong, or you didn't
  run `supabase-schema.sql`. Re-check steps 1.2 and 1.5.
- **"Wrong password"** → that's the password of the team user from step 1.3,
  not your Supabase account password.
- **Nothing saves / "Offline – retrying"** → confirm the SQL ran successfully and
  that Realtime is enabled (the schema script does this).
