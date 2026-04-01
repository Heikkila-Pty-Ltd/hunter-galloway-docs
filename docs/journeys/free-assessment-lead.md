# Journey: Free Assessment Lead

**Form ID:** F-02
**Last updated:** 2026-04-01

## 1. Purpose

Primary assessment funnel for prospective borrowers. This is the highest-conversion form — it's the destination of "Get a Free Assessment" CTAs across the entire site. Leads from this form are the most qualified because they've explicitly requested an assessment.

## 2. Entry Point

- **URL:** `/get-free-assessment/`
- **Form name:** Free Assessment
- **Form ID in DOM:** `result-form`
- **Audience:** Public website visitors who clicked "Get a Free Assessment"
- **Plugin:** Contact Form 7 (CF7), form ID: 68942
- **Also reachable via:** CTA buttons on homepage, calculator pages, landing pages, popup modals

## 3. Systems Touched

```
WordPress (CF7) → ??? → ??? → ???
```

> **TBD:** Confirm the full chain.

| System | Role | Confirmed? |
|--------|------|-----------|
| WordPress / CF7 | Form capture, validation | Yes |
| Zapier | TBD | No |
| HubSpot | TBD | No |
| Email | TBD | No |

## 4. Trigger

User clicks "Get a Free Assessment" CTA (found on multiple pages) → lands on `/get-free-assessment/` → fills form → submits.

**CTA locations confirmed (automated tests):**
- Homepage (`/`)
- First Home Buyer (`/first-home-buyer-loans/`)
- Refinance (`/refinance-home-loan/`)
- Mortgage vs Rent Calculator (`/mortgage-vs-rent-calculator/`)
- Deposit Calculator (`/deposit-calculator/`)
- Equity Calculator (`/equity-calculator/`)

## 5. Field Mapping

| Source Field | Label | Type | Required | Validation | → Destination |
|-------------|-------|------|----------|------------|---------------|
| `text-name` | Name | text | Yes | Non-empty. Error: "Please enter your name." | TBD |
| `text-email` | Email | email | Yes | Regex: `/^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/`. Error: "Please enter a valid email address." | TBD |
| `text-phone` | Phone | text | Yes | Regex: `/^(04\d{2}\s\d{3}\s\d{3}|\d{2}\s\d{4}\s\d{4})$/`. Max 12 chars. Error: "Please enter a valid phone number." | TBD |
| `transaction_type` | What Are You Looking To Do? | custom select | Yes | Must select. Error: "Please choose an option." | TBD |

**Transaction type options:**
- Purchase A Property
- Refinance A Loan
- Commercial Or Business Loan
- Build

## 6. Expected Behaviour (Happy Path)

```
Visitor clicks "Get a Free Assessment" CTA
  → Navigates to /get-free-assessment/ (or popup opens)
  → Enters name, email, phone
  → Phone auto-formats to "0412 345 678"
  → Selects transaction type from custom dropdown
  → Clicks Submit
  → Submit button disables during submission
  → CF7 processes submission
  → Result section shown:
    - .result-typ_one for non-build loans (Purchase/Refinance/Commercial)
    - .result-typ_two for Build loans
    - .result_get for thank-you message
  → TBD: Lead created in CRM
  → TBD: Broker notified
  → TBD: Follow-up workflow enrolled
```

## 7. Branching / Rules

| Condition | Path |
|-----------|------|
| Transaction type = Build | Shows `.result-typ_two` (build-specific result) |
| Transaction type ≠ Build | Shows `.result-typ_one` (standard result) |
| Email exists in CRM | TBD: Update or create? |
| Transaction type = Commercial | TBD: Different broker assignment? |

## 8. Error Handling

| Failure Point | What Happens | Confirmed? |
|---------------|-------------|-----------|
| Name empty | "Please enter your name." | Yes |
| Email invalid | "Please enter a valid email address." | Yes |
| Phone invalid | "Please enter a valid phone number." | Yes |
| No transaction type | "Please choose an option." | Yes |
| All fields empty + submit | Multiple validation errors shown | Yes (Playwright) |
| Server-side failure | TBD | TBD |
| CRM integration failure | TBD: Lead lost? | **CRITICAL TBD** |

## 9. Notifications

| Event | Who | Channel | Confirmed? |
|-------|-----|---------|-----------|
| New assessment request | TBD | TBD | No |
| High-value lead (e.g., Commercial) | TBD | TBD | No |

## 10. Dependencies

| Dependency | Type | Risk if Down |
|-----------|------|-------------|
| CF7 plugin | WordPress plugin | Form doesn't render |
| Custom select JS | Custom UI component | Dropdown may not open |
| Phone formatting JS | Custom JS | Format not applied |
| TBD: Zapier | Automation | Leads not reaching CRM |
| TBD: HubSpot | CRM | Contacts not created |

## 11. Test Cases

| Test ID | Scenario | Automated? | Status |
|---------|----------|-----------|--------|
| T-011 | Form loads | Yes | Pass |
| T-012 | Name field works | Yes | Pass |
| T-013 | Email field present | Yes | Pass |
| T-014 | Phone auto-format | Yes | Pass |
| T-015 | Transaction type dropdown | Yes | Pass |
| T-016 | Submit button present | Yes | Pass |
| T-017 | Empty submission blocked | Yes | Pass |
| T-018 | Valid end-to-end | **No** | **TBD** |
| T-023–T-029 | CTA presence on 6 pages | Yes | Pass |

## 12. Known Issues / Caveats

- **Highest priority form on the site.** If this breaks, it's the primary lead funnel. All "Get a Free Assessment" CTAs across the site point here.
- **Custom select component:** The transaction type dropdown uses a custom `.btn_select` UI replacement, not a native `<select>`. If the custom JS breaks, the dropdown won't open.
- **Two result display paths:** Build vs non-build results use different DOM sections. Regression in either path could show wrong results.
- **No end-to-end test:** Manual verification needed to confirm leads actually reach the CRM.
