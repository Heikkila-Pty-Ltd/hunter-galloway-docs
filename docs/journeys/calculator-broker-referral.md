# Journey: Calculator → Broker Referral

**Form IDs:** F-05, F-06
**Last updated:** 2026-04-01

## 1. Purpose

After using the LVR or YTD calculator, visitors can opt in to speak with a mortgage broker. This captures warm leads who have already engaged with a tool and demonstrated intent. These are high-quality leads because they've self-selected by requesting broker contact.

## 2. Entry Points

| Calculator | URL | Form ID | Gravity Form |
|-----------|-----|---------|-------------|
| LVR Calculator | `/lvr-calculator/` | F-05 | gform_wrapper_10 (GF ID: 10) |
| YTD Calculator | `/ytd-calculator/` | F-06 | gform_9 (GF ID: 9) |

## 3. Systems Touched

```
WordPress (Gravity Forms) → ??? → ???
```

## 4. Trigger

1. User fills calculator inputs and clicks Calculate
2. Calculator shows result
3. User selects "Yes" to "Talk to a Mortgage Broker" radio button
4. Gravity Form appears below the calculator
5. User fills name, phone, email and submits

## 5. Field Mapping

### LVR Calculator Broker Form (F-05, GF ID: 10)

| Source Field | Type | Required | → Destination |
|-------------|------|----------|---------------|
| Full Name | text | Yes | TBD |
| Contact Number | text (tel) | Yes | TBD |
| Email Address | email | Yes | TBD |

### YTD Calculator Broker Form (F-06, GF ID: 9)

| Source Field | Type | Required | → Destination |
|-------------|------|----------|---------------|
| Full Name | text | Yes | TBD |
| Contact Number | text (tel) | Yes | TBD |
| Email Address | email | Yes | TBD |

**Additional context captured (from calculator inputs):**

| Calculator | Data Available | Captured in form? |
|-----------|---------------|------------------|
| LVR | Loan amount, property value, LVR % | TBD: Are calc values passed as hidden fields? |
| YTD | Start date, end date, gross income, annualized result | TBD: Are calc values passed as hidden fields? |

## 6. Expected Behaviour (Happy Path)

```
Visitor uses LVR calculator
  → Enters $450k loan, $600k property
  → Clicks Calculate → sees 75% LVR
  → Selects "Yes" to broker contact
  → Gravity Form slides in below calculator
  → Fills name, phone, email → submits
  → TBD: Lead created with calculator context
  → TBD: Broker notified with LVR result
```

## 7. Branching / Rules

| Condition | Path |
|-----------|------|
| Broker radio = No | Gravity Form stays hidden, no lead capture |
| Broker radio = Yes | Gravity Form appears |
| High LVR (>80%) | TBD: Different handling for borderline borrowers? |

## 8. Error Handling

TBD — Gravity Forms has its own validation and AJAX submission. Confirm error handling.

## 9. Dependencies

| Dependency | Type | Risk if Down |
|-----------|------|-------------|
| Gravity Forms plugin | WordPress plugin | Broker form doesn't render |
| Calculator JS (AutoNumeric) | JavaScript | Calculator inputs don't format |
| Conditional display JS | Custom JS | Form may not appear when "Yes" selected |

## 10. Test Cases

| Test ID | Scenario | Automated? | Status |
|---------|----------|-----------|--------|
| T-035–T-038 | LVR calc loads, fields, calculates, validates | Yes | Pass |
| T-039–T-042 | YTD calc loads, fields, calculates, button works | Yes | Pass |

## 11. Known Issues / Caveats

- **Conditional rendering:** The Gravity Form only appears when "Yes" is selected. If the JS that handles the radio → form toggle breaks, the form never appears even though the user opted in. This is a silent lead loss.
- **Calculator context may not be captured:** It's unknown whether the calculator input values (loan amount, property value, LVR result) are passed to the Gravity Form as hidden fields. If not, the broker receives a contact request with no context about what the user was calculating.
- **Two separate Gravity Forms:** GF9 (YTD) and GF10 (LVR) are different forms. Changes to one don't affect the other. Both need monitoring.
