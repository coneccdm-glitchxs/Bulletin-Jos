# M.Com Karnataka Job Bulletin — Auto-Updating Setup

This turns your bulletin into a page that refreshes itself daily, for free,
using GitHub Actions + GitHub Pages. No server to rent, no code to run on
your own computer.

## How it works
```
GitHub Actions (daily cron, 7:30 AM IST)
        │
        ▼
   scraper.py  ──►  data/jobs.json   (committed back to the repo)
                          │
                          ▼
              index.html  (fetches data/jobs.json on page load)
                          │
                          ▼
                 GitHub Pages (your live URL)
```

## One-time setup (about 10 minutes)

### 1. Create a GitHub account
Go to github.com → Sign up (free). Skip if you already have one.

### 2. Create a new repository
- Click the **+** in the top right → **New repository**
- Name it something like `mcom-bulletin`
- Set it to **Public** (required for free GitHub Pages)
- Do NOT initialize with a README (we already have the files)

### 3. Upload these files
On the new repo's page, click **"uploading an existing file"** and drag in
the whole contents of this folder, keeping the structure:
```
mcom-bulletin/
├── index.html
├── scraper.py
├── requirements.txt
├── data/
│   └── pinned.json
└── .github/
    └── workflows/
        └── update-jobs.yml
```
(If the web uploader flattens folders, instead install GitHub Desktop —
github.com/apps/desktop — clone the empty repo, copy these files into the
cloned folder on your computer, and push. This preserves folder structure
reliably.)

### 4. Turn on GitHub Pages
- In your repo, go to **Settings → Pages**
- Under "Build and deployment", set **Source: Deploy from a branch**
- Branch: `main`, folder: `/ (root)` → **Save**
- GitHub will give you a URL like `https://yourusername.github.io/mcom-bulletin/`
  — that's your permanent link. Open it any time.

### 5. Run the scraper for the first time
- Go to the **Actions** tab in your repo
- Click **"Update M.Com Job Bulletin"** in the left sidebar
- Click **Run workflow** → **Run workflow** (green button)
- Wait ~30 seconds, refresh — you should see a green checkmark
- This creates `data/jobs.json` for the first time and commits it

### 6. Visit your live page
Open your GitHub Pages URL. It should now show "Latest Checked Listings"
pulled from the first scraper run. From tomorrow onward, it updates itself
every day at 7:30 AM IST — just open the same link whenever you want.

## Customizing what gets checked
Open `scraper.py` and look at the `SOURCES` list near the top — add more
government/portal URLs there, following the pattern of the existing
entries. If a new source has a different page structure, you'll need a
small custom parser function (the two existing ones,
`parse_karnatakacareers` and `parse_generic_notice_list`, are templates
to copy from).

## Adding things you want tracked no matter what
Edit `data/pinned.json` directly (through GitHub's web editor — click the
file, then the pencil icon) to add or update entries like IBPS exam
cycles that are easier to hand-maintain than scrape. These always show
in the "Pinned — Watch These" section regardless of what the scraper finds.

## Important honesty notes
- **Scraping is inherently fragile.** If a government site redesigns its
  page, that source's parser may return nothing for a while — the script
  is written to keep yesterday's data for that source rather than erase
  it, but you'll want to glance at the Actions tab occasionally to check
  for repeated failures.
- **Respect each site's terms of use.** This is set up to check a small,
  fixed list of pages once a day — not to crawl aggressively. Before
  adding a new source, quickly check its robots.txt / terms of service.
- **Always verify on the official site before applying or paying any
  fee.** This bulletin is a discovery tool, not the authoritative source.
