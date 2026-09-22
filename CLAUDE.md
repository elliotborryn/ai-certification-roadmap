# AI Certification Roadmap

A static GitHub Pages site. There's no build step, no framework and no npm dependencies for the site.

## Structure

```
index.html                  The whole site: layout, CSS and rendering. Loads the data with fetch().
assets/                     Images: favicon.svg, apple-touch-icon.png (180px render of the SVG),
                            og-image.png (1200×630 link preview, absolute URL in index.html), banner-image.png (README).
data/certifications.json    All content: meta.lastChecked, domains (columns), levels (rows), certifications.
PLACEMENT.md                Rules for choosing a certification's domain, level and field values.
scripts/validate.mjs        Data validator (plain Node, no dependencies).
scripts/check-links.mjs     Checks every certification URL still loads (plain Node, no dependencies).
.github/workflows/validate.yml        Runs the validator on every push and pull request.
.github/workflows/monthly-checks.yml  Monthly: opens issues for stale data (6+ months) and broken links.
.github/ISSUE_TEMPLATE/suggest-certification.yml  Issue form linked from the page footer.
README.md                   For visitors: what the roadmap is, how placement works, how to suggest a certification.
```

- The page renders certifications in array order within each domain and level. Header totals and each domain's count leave out `"status": "prereq"` entries.
- The CSS grid is sized for exactly **8 domains and 4 levels** (`.board` columns and `.yaxis` rows in `index.html`). Adding or removing a domain or level needs CSS changes and an icon in `ICONS`. Don't do it as part of a routine certification update.
- The site lives at https://elliotborryn.github.io/ai-certification-roadmap/ (repo `elliotborryn/ai-certification-roadmap`). The `og:*` and `canonical` tags and the footer's "Suggest one" link hardcode this address.
- Fees are stored in USD and converted in the browser by the currency picker (rates from api.frankfurter.dev). The converter rewrites every `$<number>` in a `price` string, so keep prices in the format described in PLACEMENT.md.
- On screens 760px wide or less, the page always shows the List view (CSS in the `max-width: 760px` block).
- `fetch()` doesn't work over `file://`. To preview, run `python3 -m http.server` in the repo root and open http://localhost:8000.

## Adding or updating a certification

Follow these steps in order:

1. **Research first.** Find the certification's official page on the vendor's own site. Get the official name, exam code, fee, prerequisites and recommended experience, status (beta or launch date), and whether it has been retired. Only use industry reports if the vendor page doesn't have the information, and then set `"source": "reports"`.
2. **Apply [PLACEMENT.md](PLACEMENT.md)** to choose the `domain`, `level`, `status` and `source`, and to fill in the fields. If a certification is retired, don't add it. Put it in the footer's "Not included" note instead, and remove its entry if one exists.
3. **Edit only `data/certifications.json`.** The one exception is the footer's "Not included" note in `index.html` when you handle a retired certification. Keep the file order: grouped by domain, low to high level.
4. **Run the validator:** `node scripts/validate.mjs`. It must pass.
5. **Commit message:** list each certification you added or changed, the domain and level you chose, and **why**, citing the PLACEMENT.md rule you applied (for example, "gen/L3: 1–2 years of production agent experience recommended; building agents → gen"). Also say what you checked on the vendor page and what came from reports.

## Suggestions and re-checks

- **Suggestion issue:** treat it as a lead, not a source. Verify everything on the vendor page, then follow the steps above. Mention the issue number in the commit message (for example "Closes #12") only if the user asks you to commit.
- **Broken-links issue:** find the certification's new official page on the vendor's site (vendors often move exam pages to a separate domain). Confirm it is the same certification before updating `url`. If the certification was retired, handle it as a retirement. Run `node scripts/check-links.mjs` to confirm. A "blocked" result (401/403/429) usually means bot protection, not a dead link.
- **Full re-check** (for example after the monthly reminder issue): go through every certification's vendor page, fix what changed, handle retirements, then set `meta.lastChecked` to the current month (`"YYYY-MM"`). In the commit message, list what changed and which entries you couldn't verify.

## Hard rules

- **Never invent URLs, fees or prerequisites.** If you can't verify a fee, use `"See vendor"`. If there's no dedicated page, use the vendor's certification landing page. If you don't know the prerequisites, say what is known and mark `"source": "reports"`.
- Only set `"source": "vendor"` when you actually checked the details on the vendor's official page in this session.
- Don't change other certifications' content unless the user asks you to or the vendor page shows it is out of date. If you do change it, say so in the commit message.
- Don't commit or push unless the user asks you to.
