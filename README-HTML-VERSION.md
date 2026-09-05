# Student Planner — HTML/JS Version

This is a converted version of the original PHP + MySQL "Student Planner" app.
It runs entirely as static HTML, CSS and JavaScript — **no PHP, no MySQL, no
server required.** Just open `login.html` in a browser (or host this folder
on any static web host).

## What changed vs. the original

The original app used PHP to run SQL queries against a MySQL database on a
server. HTML by itself can't run a server-side language or a database, so to
make a version that actually works as plain HTML, all of that logic was
rewritten in JavaScript, and the database was replaced with the browser's
built-in `localStorage`:

- Every `.php` page became a `.html` page with the same look and the same
  features, but the logic that used to live in PHP now runs in a `<script>`
  block using JavaScript.
- `db.php` / MySQL → `assets/js/app.js`, which keeps all data (accounts,
  tasks, subjects, schedule, notes, events, groups, messages) as JSON in
  `localStorage`, scoped to whichever browser/device opens the page.
- Session login (`$_SESSION`) → a logged-in user id stored in `localStorage`.
- File uploads (avatars, chat images) → images are read in the browser and
  stored as base64 data directly in `localStorage`, instead of being saved
  as files on a server.
- Password hashing is not meaningful for a file that runs entirely in the
  visitor's own browser with no server to protect, so passwords are kept as
  plain values in local storage — fine for a demo/personal planner, but treat
  this the way you'd treat any local-only tool, not as production auth.

## Data & multi-device note

Because everything is saved with `localStorage`, data lives **only in the
browser/device where you use it** — there's no shared server. Two people
opening these files on two different computers will each get their own,
separate data. If you need real multi-user syncing, you'd want to keep the
original PHP + MySQL version (included alongside this one) and host it on a
PHP + MySQL server.

## Demo logins (seeded automatically the first time you open it)

- **admin / admin123** (role: admin)
- **juan / juan123** (role: student)

You can also register a new account from `register.html`. Admins can approve
pending accounts and change roles from **Manage Accounts**.

## Files

- `login.html`, `register.html` — sign in / sign up
- `index.html` — dashboard
- `tasks.html`, `subjects.html`, `schedule.html`, `notes.html`,
  `calendar.html`, `analytics.html`, `messages.html`, `profile.html` — main app
- `accounts.html` — admin-only account management
- `assets/css/style.css` — the original shared theme, unchanged
- `assets/js/app.js` — the client-side "backend" (data + auth + layout)
- `original-php-version/` — the complete, untouched original PHP + MySQL
  project (all files, uploads, and `student_planner.sql`), kept here so
  nothing from the original was removed.

## Resetting the demo data

Open the browser console on any page and run:

```js
SP.resetAll();
```

This clears all local data and reseeds the original sample accounts/tasks.
