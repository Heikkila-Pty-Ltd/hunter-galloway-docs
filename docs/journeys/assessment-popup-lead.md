# Journey: Assessment Popup Quiz Lead

**Form ID:** F-04
**Last updated:** 2026-04-01

## 1. Purpose

Quick-qualify popup quiz that appears when visitors click "Get a Free Assessment" CTAs across the site. Captures property price expectations, deposit availability, and contact details. These leads come from visitors already browsing specific content (calculators, service pages), making them contextually qualified.

## 2. Entry Point

- **URL:** Multiple pages (popup triggered by CTA)
- **Form name:** Assessment Quiz Popup
- **Plugin:** Popup Maker (IDs: #popmake-11791, #popmake-14062, etc.)
- **Audience:** Website visitors who click assessment CTAs

**Pages with confirmed CTAs:**
| Page | URL | CTA Confirmed? |
|------|-----|---------------|
| Homepage | `/` | Yes |
| First Home Buyer | `/first-home-buyer-loans/` | Yes |
| Refinance | `/refinance-home-loan/` | Yes |
| Mortgage vs Rent Calc | `/mortgage-vs-rent-calculator/` | Yes |
| Deposit Calculator | `/deposit-calculator/` | Yes |
| Equity Calculator | `/equity-calculator/` | Yes |

## 3. Systems Touched

```
WordPress (Popup Maker) → ??? → ???
```

## 4. Trigger

User clicks "Get a Free Assessment" button on any page → popup opens with quiz → user completes quiz → lead captured.

## 5. Quiz Flow & Field Mapping

### Step 1: Property Price

| Field | Type | Options |
|-------|------|---------|
| Property price | Preset buttons + manual input | $500k, $600k, $700k, $800k, more than $800k, or custom amount |

**Auto-calculation:** "Required minimum loan deposit" shown as 8% of selected price.

### Step 2: Deposit Availability

| Field | Type | Options |
|-------|------|---------|
| Do you have access to this amount? | Radio buttons | YES, NO |

### Step 3: Special Circumstances

| Field | Type | Options |
|-------|------|---------|
| Circumstances | Checkboxes | Bridging or Low Documentation Loan, Bad Credit Rating Default or Bankruptcy, None of these |

### Step 4: Contact Details

| Source Field | Type | Required | → Destination |
|-------------|------|----------|---------------|
| Name | text | Yes | TBD |
| Email | email | Yes | TBD |
| Phone | text | Yes | TBD |

## 6. Expected Behaviour (Happy Path)

```
Visitor clicks "Get a Free Assessment" on any page
  → Popup opens (Popup Maker)
  → Step 1: Selects $600k → sees "minimum deposit: $48,000"
  → Step 2: Selects YES (has deposit)
  → Step 3: Selects "None of these"
  → Step 4: Enters name, email, phone → Finish & Send
  → TBD: Lead created in CRM with quiz data
  → TBD: Broker notified
  → User sees thank-you / "Ok, Sounds Great!" message
```

## 7. Branching / Rules

| Condition | Path |
|-----------|------|
| Deposit = NO | TBD: Different result? Rejection? Still captured? |
| Bad Credit selected | TBD: Different routing to specialist? |
| Bridging Loan selected | TBD: Different product path? |
| Price > $800k | TBD: High-value lead routing? |

## 8. Error Handling

| Failure Point | What Happens | Confirmed? |
|---------------|-------------|-----------|
| Popup JS fails to load | CTA click does nothing or navigates to /get-free-assessment/ | TBD |
| User closes popup mid-quiz | Data lost (no partial capture assumed) | TBD |
| Cookie blocking popup | Popup may not appear on return visit | TBD |

## 9. Dependencies

| Dependency | Type | Risk if Down |
|-----------|------|-------------|
| Popup Maker plugin | WordPress plugin | CTAs don't open popup |
| Popup Maker JS | JavaScript | Popup doesn't render |
| Cookie tracking | Browser | Popup may show/hide unexpectedly |

## 10. Test Cases

| Test ID | Scenario | Automated? | Status |
|---------|----------|-----------|--------|
| T-023–T-028 | CTA present on 6 pages | Yes | Pass |
| T-029 | CTA opens popup or navigates | Yes | Pass |

## 11. Known Issues / Caveats

- **Multiple popup IDs:** The site uses several Popup Maker popup IDs. If a specific popup is deleted or its ID changes, the CTA may silently fail.
- **Cookie-based tracking:** Popup Maker uses cookies to track whether a popup has been shown. This can cause inconsistent behavior in testing.
- **Fallback behavior varies:** Some CTAs open popups, others navigate to `/get-free-assessment/`. The behavior is page-specific.
