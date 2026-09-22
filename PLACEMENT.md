# Placement rules

How to decide where a certification goes on the roadmap and how to fill in its fields. Every entry in `data/certifications.json` should follow these rules so placements stay consistent over time.

## 1. Should it be added at all?

- **Only certifications that are currently offered.** A retired certification is not added. Instead, name it in the footer's "Not included" note in `index.html`, and remove its entry if one exists.
- **It must be AI-specific,** with one exception: a non-AI certification that is a formal prerequisite for an AI certification on the roadmap can be shown for context with `"status": "prereq"` (for example ISTQB CTFL). Prerequisites aren't counted in the header or domain totals.

## 2. Domain (the column)

Pick the domain for **what the certification proves**, not who issues it.

| key | Domain | Goes here |
|---|---|---|
| `lit` | AI Literacy & Adoption | AI literacy, business adoption, admin fundamentals (for example Copilot administration) |
| `ml` | ML Engineering & MLOps | Classic ML engineering, data science, MLOps |
| `gen` | GenAI & Agent Development | Building LLM applications and agents |
| `infra` | AI Infrastructure | GPU, cluster and networking operations |
| `test` | AI Quality & Testing | Testing AI systems, or testing with AI |
| `sec` | AI Security | Defending and securing AI systems |
| `red` | AI Red Teaming | Offensive testing and red teaming of AI |
| `grc` | AI Governance, Risk & Audit | Governance, risk, audit, and standards such as ISO/IEC 42001 |

### Tie-breakers

1. **Offensive work goes in `red`,** even if the vendor calls it a "security" certification. If the exam is mainly about attacking AI systems (prompt injection, model extraction, jailbreaks), it belongs in `red`.
2. **Building agents goes in `gen`,** even when it's done on a cloud platform or a business-apps platform.
3. **There is no "cloud" column.** A cloud vendor's certification goes in the domain it proves. For example, a cloud AI fundamentals exam goes in `lit` and a cloud ML engineer exam goes in `ml`.
4. If it still spans two domains, choose the one that covers **the majority of the exam objectives** in the vendor's published outline, and say why in the commit message.

## 3. Level (the row)

| Level | Name | Rule |
|---|---|---|
| 1 | Foundational | No experience or prerequisites required |
| 2 | Associate | Some hands-on experience recommended, **or** an "Associate" title |
| 3 | Professional | 1–3+ years of experience recommended, **or** a "Professional" title, **or** a practical exam aimed at practitioners |
| 4 | Expert | Requires another certification first, **or** targets senior or expert roles |

**If the vendor's title and the recommended experience disagree, go by the recommended experience.** For example, if an exam is titled "Associate" but recommends 3+ years of experience, it goes at L3. If a "Fundamentals" exam requires another certification first, it goes at L4.

Check from the top: if L4 applies, stop there. Otherwise check L3, then L2. Anything left is L1.

## 4. Fields

| Field | Rule |
|---|---|
| `code` | The vendor's exam code (for example `AIF-C01`). Use a short, recognisable name only if there is no code. **Must be unique.** |
| `name` | The full official name, as written on the vendor's page. |
| `vendor` | The issuing organisation, using the spelling already in the file (for example `Google Cloud`, `GIAC / SANS`). |
| `domain` | One of the keys above. |
| `level` | A number from 1 to 4, not a string. |
| `price` | The exam fee in **US dollars**, written with `$` and digits so the page's currency picker can convert it: `"$150"`, `"$459–$599"`, `"From $1,749 (course and exam)"`. Use `"Free"` when it is free. If the vendor only prices in another currency, give the approximate USD amount and say so, for example `"About $250 (priced in GBP)"`. Never write `€`, `£` or other symbols. **Use `"See vendor"` if you don't know the fee. Never guess a fee.** |
| `status` | `"new"` if launched in 2025 or later · `"beta"` if the exam is in beta · `"prereq"` only for a non-AI prerequisite shown for context · otherwise `"active"`. |
| `source` | `"vendor"` only if every detail was checked on the vendor's official page. Otherwise `"reports"`. |
| `url` | The vendor's official page for this certification, starting with `https://`. If there is no dedicated page, use the vendor's certification landing page. Never make up a URL. |
| `what` | One or two plain sentences on what the certification covers. Mention anything it replaced ("Replaced AI-102.") or a notable launch date. |
| `before` | The prerequisites and recommended experience, as the vendor states them. If there are none, write `"No prerequisites."` or `"No formal prerequisites."` Say "Reported to…" if the information came from reports rather than the vendor. |

### Order in the file

Entries are grouped by domain, in the same order as `domains`, and by level from low to high within each domain. Within a level, the file order is the order shown on the page, so put a new entry next to related ones.

### After a check pass

When you re-check the data, update `meta.lastChecked` (`"YYYY-MM"`). The page footer shows it.

## 5. Template

```json
{
  "code": "EXAM-CODE",
  "name": "Full Official Certification Name",
  "vendor": "Vendor",
  "domain": "gen",
  "level": 2,
  "price": "See vendor",
  "status": "active",
  "source": "vendor",
  "url": "https://vendor.example/certification-page",
  "what": "What the exam covers, in one or two sentences.",
  "before": "No formal prerequisites."
}
```

## 6. Checklist

- [ ] Currently offered (not retired) and AI-specific, or a formal prerequisite of one on the roadmap
- [ ] Domain chosen from what it proves, with the tie-breakers applied
- [ ] Level chosen from the rubric; where the title and experience disagree, experience decides
- [ ] Price, prerequisites and URL come from a real source; unknown price is `"See vendor"`
- [ ] `source` is honest about where the details came from
- [ ] `node scripts/validate.mjs` passes
