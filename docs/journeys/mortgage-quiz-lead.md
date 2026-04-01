# Journey: Mortgage Calculator Quiz Lead

**Form ID:** F-03
**Last updated:** 2026-04-01

## 1. Purpose

Multi-step quiz that assesses a visitor's borrowing capacity while capturing their details as a lead. This is the most engaging lead gen tool — it provides value (borrowing estimate) in exchange for contact information. Visitors who complete the full quiz are highly qualified leads.

## 2. Entry Point

- **URL:** `/mortgage-calculator/`
- **Form name:** Mortgage Calculator Quiz
- **Audience:** Public website visitors exploring borrowing capacity
- **Plugin:** Custom JavaScript quiz application (not CF7/Gravity Forms)
- **Also linked from:** Header nav "Calculators", `/tools/` page

## 3. Systems Touched

```
WordPress (custom JS quiz) → ??? → ??? → ???
```

> **TBD:** How does the quiz submit data? AJAX? Webhook? Direct HubSpot API?

| System | Role | Confirmed? |
|--------|------|-----------|
| WordPress / Custom JS | Quiz UI, calculations, form capture | Yes |
| TBD | TBD | No |

## 4. Trigger

User navigates to `/mortgage-calculator/` and progresses through the multi-step quiz.

## 5. Quiz Flow & Field Mapping

### Step 1: Purpose Selection

| Field | Type | Options |
|-------|------|---------|
| Loan purpose | Radio buttons | Purchase a property, Build, Commercial or business loan, Refinance a loan, I'm just exploring |

### Step 2: Applicant Details

| Field | Type | Options |
|-------|------|---------|
| Applicant type | Radio buttons | Couple, Single person |
| First home owner | Radio buttons | Yes, No |
| State | Dropdown | NSW, VIC, QLD, ACT, WA, SA, TAS, NT |
| Dependents | Radio buttons | 0, 1, 2, 3+ |

### Step 3: Financial Information

| Field | Type | Validation |
|-------|------|-----------|
| Total household income (before tax) | Currency input | Dollar amount with $ prefix |
| Deposit amount | Currency input | Dollar amount with $ prefix |

### Step 4: Contact Details + Goal

| Source Field | Type | Required | Validation | → Destination |
|-------------|------|----------|------------|---------------|
| Name | text | Yes | Non-empty | TBD |
| Email | email | Yes | Valid email | TBD |
| Phone | text | Yes | Valid AU phone | TBD |
| What are you looking to do? | dropdown | Yes | Must select | TBD |

**Goal options:** borrowing power, pre-approval, contract signed, refinance, other

### Calculated Outputs (shown to user)

| Output | Calculation |
|--------|-------------|
| Loan type | Based on Step 1 selection |
| Loan amount | Based on income, deposit, dependents |
| LVR | Loan amount / estimated property value |

## 6. Expected Behaviour (Happy Path)

```
Visitor opens /mortgage-calculator/
  → JS quiz app loads (may take 2-3 seconds)
  → Step 1: Selects "Purchase a property" → clicks Continue
  → Step 2: Selects Couple, No (not first home), QLD, 1 dependent → Continue
  → Step 3: Enters $150,000 income, $80,000 deposit → Continue
  → Step 4: Enters name, email, phone, selects "borrowing power" → Finish & Send
  → Summary displayed (loan type, amount, LVR)
  → TBD: Data submitted to CRM
  → TBD: Broker notified
```

## 7. Branching / Rules

| Condition | Path |
|-----------|------|
| Purpose = "I'm just exploring" | TBD: Different handling? |
| Purpose = "Commercial or business loan" | TBD: Different routing? |
| Deposit < 8% of estimated property | May show deposit warning |
| Goal = "contract signed" | TBD: Highest urgency lead? |

## 8. Error Handling

| Failure Point | What Happens | Confirmed? |
|---------------|-------------|-----------|
| JS fails to load | Blank page with logo + back arrow only | Yes (observed) |
| User abandons mid-quiz | TBD: Partial data captured? | **TBD** |
| Back button clicked | Returns to previous step with selections preserved | Yes |
| Contact fields invalid | TBD: Inline validation? | TBD |

## 9. Notifications

TBD — confirm who gets notified and how when a quiz is completed.

## 10. Dependencies

| Dependency | Type | Risk if Down |
|-----------|------|-------------|
| Custom JS quiz bundle | JavaScript | Page renders blank — **critical failure, invisible to user** |
| CDN (cdn-bdkfg.nitrocdn.com) | Asset delivery | JS/CSS may not load |
| TBD: Backend API | Data submission | Quiz completes but lead not captured |

## 11. Test Cases

| Test ID | Scenario | Automated? | Status |
|---------|----------|-----------|--------|
| T-019 | Quiz page loads | Yes | Pass |
| T-020 | Purpose options present | Yes | Pass |
| T-021 | Contact fields in DOM | Yes | Pass |
| T-022 | Full quiz end-to-end | **No** | **TBD** |

## 12. Known Issues / Caveats

- **JS-dependent rendering:** The quiz is entirely JavaScript. If the JS bundle fails to load (CDN issue, script error), the page renders blank — just a logo and back arrow. There's no server-side fallback. This is a silent failure that would be invisible without automated testing.
- **Blank page is a known risk:** During testing, the quiz sometimes rendered blank on first load. This could be a real regression risk.
- **No partial data capture (assumed):** If a user completes steps 1-3 but abandons at step 4 (contact details), the financial data is likely lost. Confirm whether any tracking captures partial completion.
