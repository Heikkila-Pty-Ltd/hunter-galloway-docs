# Hunter Galloway — Forms, Automations & Testing Documentation

**Site:** https://www.huntergalloway.com.au
**Last updated:** 2026-04-01

---

## Documentation Pack

### Sheets (import to Google Sheets)

| # | File | What it is | Rows |
|---|------|-----------|------|
| 1 | [01-system-inventory.csv](sheets/01-system-inventory.csv) | **Master inventory** — every form, calculator, zap, workflow, notification | 25 |
| 2 | [02-field-mapping.csv](sheets/02-field-mapping.csv) | **Field mapping** — source field → validation → destination for every form | 35 |
| 3 | [03-rules-expected-behaviour.csv](sheets/03-rules-expected-behaviour.csv) | **Rules & expected behaviour** — decision table for all scenarios | 30 |
| 4 | [04-test-matrix.csv](sheets/04-test-matrix.csv) | **Test matrix** — every test case, automated or manual, pass/fail | 66 |
| 5 | [05-hubspot-workflows.csv](sheets/05-hubspot-workflows.csv) | **HubSpot workflows** — all 99 visible workflows, categorized with status and enrollment | 99 |

**To import:** Google Sheets → File → Import → Upload → select CSV → "Replace spreadsheet"

Recommended: Import all 4 CSVs as separate tabs in one Google Sheet.

### Journey Docs (one per lead path)

| Journey | Form ID | File |
|---------|---------|------|
| Contact Form Enquiry | F-01 | [contact-form-enquiry.md](journeys/contact-form-enquiry.md) |
| Free Assessment Lead | F-02 | [free-assessment-lead.md](journeys/free-assessment-lead.md) |
| Mortgage Quiz Lead | F-03 | [mortgage-quiz-lead.md](journeys/mortgage-quiz-lead.md) |
| Assessment Popup Lead | F-04 | [assessment-popup-lead.md](journeys/assessment-popup-lead.md) |
| Calculator → Broker Referral | F-05, F-06 | [calculator-broker-referral.md](journeys/calculator-broker-referral.md) |

---

## ID Convention

| Prefix | Meaning | Example |
|--------|---------|---------|
| `F-##` | Form | F-01 = Contact Us |
| `C-##` | Calculator | C-02 = Equity Calculator |
| `Z-##` | Zapier Zap | Z-01 = TBD |
| `HS-WF-##` | HubSpot Workflow | HS-WF-01 = TBD |
| `N-##` | Notification | N-01 = TBD |
| `T-###` | Test Case | T-001 = Contact form loads |

---

## What's Confirmed vs TBD

### Confirmed (from site exploration + automated tests)

- All WordPress form fields, validation rules, field names, IDs
- All calculator inputs, outputs, formulas
- All CTA locations and behavior
- Page availability (30+ pages)
- SEO meta tags
- Navigation elements
- Phone number formatting

### TBD — Needs Investigation

| Area | Questions |
|------|-----------|
| **Zapier** | What zaps exist? What triggers them? What's the webhook URL? |
| **HubSpot** | ~~What properties map to form fields?~~ RESOLVED. ~~What workflows exist?~~ RESOLVED (99 documented). |
| **Data flow** | ~~CF7 → Zapier → HubSpot? Or CF7 → HubSpot plugin direct?~~ RESOLVED: CF7 → HubSpot Collected Forms (tracking code). |
| **Notifications** | Who gets notified? Email? Slack? How fast? |
| **Spam protection** | reCAPTCHA? Honeypot? Akismet? |
| **Deduplication** | What happens when an existing contact submits again? |
| **Hidden fields** | Are UTM params, page URL, or calculator values captured? |
| **Gravity Forms destination** | Where do GF9 and GF10 submissions go? |
| **Partial data** | Is anything captured if a user abandons mid-quiz? |
| **Error fallback** | If Zapier/HubSpot is down, are leads stored in WordPress? |

---

## Automated Test Suite

**Location:** `test-hunter-regression/`
**Framework:** Playwright + TypeScript
**Results:** 97/97 desktop, 92/96 mobile

```bash
./run-tests.sh              # Full suite
./run-tests.sh smoke        # Quick smoke test
./run-tests.sh forms        # Form tests only
./run-tests.sh calculators  # Calculator tests only
```
