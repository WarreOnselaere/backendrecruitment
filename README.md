# Partner Directory - 100% Vercel

Frontend: static `index.html`. Backend: Vercel serverless functions in `/api`.
Database + files: Neon Postgres (added from the Vercel Marketplace, billed through Vercel). No Firebase.

## Deploy
1. Push this folder to a GitHub repo.
2. vercel.com > Add New > Project > import the repo. Framework preset: Other. No build command.
3. In the project: Storage > Create Database > Neon (Postgres). Connect it to the project; this adds `DATABASE_URL` automatically. Pick an EU region (Frankfurt) if offered.
4. Project > Settings > Environment Variables, add:
   - `SESSION_SECRET`  a long random string (30+ characters)
   - `ADMIN_EMAIL`     your email
   - `ADMIN_PASSWORD`  a strong password (only used to create the first admin)
5. Redeploy (Deployments > ... > Redeploy), open the site and sign in with ADMIN_EMAIL / ADMIN_PASSWORD. Tables are created automatically.
6. Remove `ADMIN_PASSWORD` from the environment variables afterwards.
7. Add colleagues via the "Accounts" button in the sidebar (admins only).

## How it works
- Passwords are hashed with scrypt; sessions are signed HttpOnly cookies (30 days). Login is rate limited.
- The app polls `/api/sync` every 5 seconds, so changes from colleagues show up within seconds.
- Documents are stored in the database (max 4 MB per file) and only served to signed-in users.
- Opening index.html straight from disk (or in the Electron app) still uses the old local mode.

## Moving your existing data
Use Backup in the old app, then Restore in the deployed one (try with a copy first).
