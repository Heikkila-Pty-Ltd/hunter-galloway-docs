# Journey: Contact Form Enquiry

**Form ID:** F-01
**Last updated:** 2026-04-01

## 1. Purpose

General enquiries from website visitors. Primary lead capture form for visitors who want to ask questions or request a callback. This is the most visible form on the site (linked from header nav).

## 2. Entry Point

- **URL:** `/contact/`
- **Form name:** Contact Us (2-step)
- **Audience:** Public website visitors
- **Plugin:** Contact Form 7 (CF7), form ID: 11830
- **Endpoint:** `POST /wp-json/contact-form-7/v1/contact-forms/11830/feedback`

## 3. Systems Touched

```
WordPress (CF7) → ??? → ??? → ???
```

> **TBD:** Confirm the full chain. Does CF7 send directly to HubSpot via plugin, or via Zapier webhook, or both?

| System | Role | Confirmed? |
|--------|------|-----------|
| WordPress / CF7 | Form capture, validation, submission storage | Yes |
| Zapier | TBD: Webhook relay to CRM? | No |
| HubSpot | TBD: Contact creation, workflow enrollment? | No |
| Email | TBD: Notification to broker? | No |
| Slack | TBD: Team notification? | No |

## 4. Trigger

User navigates to `/contact/`, fills the 2-step form, and clicks Submit.

## 5. Field Mapping

| Source Field | Label | Type | Required | Validation | → Destination |
|-------------|-------|------|----------|------------|---------------|
| `your-message` | Message | textarea | Yes | Non-empty | TBD |
| `request-type` | Request Type | dropdown | Yes | Must select | TBD |
| `your-name` | Name | text | Yes | Non-empty | TBD |
| `your-email` | Email | email | Yes | Regex validated | TBD |
| `your-phone` | Phone | text | Yes | AU format, max 12 chars | TBD |

**Request type options:**
- First Home Purchase Inquiry
- Investment Property Financing
- Home Loan Refinance Inquiry
- Borrowing Capacity Assessment
- General Mortgage Advice
- Other

## 6. Expected Behaviour (Happy Path)

```
Visitor opens /contact/
  → Step 1: Types message in textarea
  → Clicks "Next"
  → Step 2: Selects request type, enters name, email, phone
  → Phone auto-formats to "0412 345 678"
  → Clicks Submit
  → Submit button disables + shows spinner
  → CF7 POSTs to /wp-json/.../11830/feedback
  → Response: { status: "success" }
  → User sees confirmation/thank-you message
  → TBD: Zap fires?
  → TBD: HubSpot contact created/updated?
  → TBD: Broker notified?
```

## 7. Branching / Rules

| Condition | Path |
|-----------|------|
| Request type = First Home Purchase | TBD: Different routing? |
| Request type = Partnerships / Other | TBD: Different assignment? |
| Email already exists in CRM | TBD: Update or create duplicate? |
| Spam detected | TBD: What spam protection is active? |

## 8. Error Handling

| Failure Point | What Happens | Confirmed? |
|---------------|-------------|-----------|
| Empty message field | "Please enter your message" error, blocked | Yes |
| Invalid email format | Inline validation error | Yes (regex) |
| Invalid phone format | Validation error | Yes (regex) |
| No request type selected | TBD | TBD |
| CF7 POST fails (server error) | User sees error? Status: `mail_failed` | TBD |
| CF7 POST returns `validation_failed` | Inline errors per field | TBD |
| Zapier webhook timeout | TBD: User still sees success? Lead lost? | **CRITICAL TBD** |
| HubSpot API rejects contact | TBD: Fallback? WP entry log? | **CRITICAL TBD** |

## 9. Notifications

| Event | Who | Channel | Confirmed? |
|-------|-----|---------|-----------|
| New contact form submission | TBD: Which broker? | TBD: Email? Slack? | No |
| Submission failure | TBD: Ops team? | TBD | No |

## 10. Dependencies

| Dependency | Type | Risk if Down |
|-----------|------|-------------|
| CF7 plugin | WordPress plugin | Form doesn't render or submit |
| CF7 REST API | WP endpoint | Submissions fail silently |
| TBD: Zapier zap | Automation | Leads not reaching CRM |
| TBD: HubSpot API | CRM | Contacts not created |
| JS auto-formatting script | Custom JS | Phone formatting breaks (cosmetic) |

## 11. Test Cases

| Test ID | Scenario | Automated? | Status |
|---------|----------|-----------|--------|
| T-001 | Form loads | Yes | Pass |
| T-002 | Textarea accepts input | Yes | Pass |
| T-003 | Step 1→2 transition | Yes | Pass |
| T-004 | Contact fields in DOM | Yes | Pass |
| T-005 | Submit button exists | Yes | Pass |
| T-006 | Request type options | Yes | Pass |
| T-007 | Phone auto-formatting | Yes | Pass |
| T-008 | Valid end-to-end submission | **No** | **TBD** |
| T-009 | Invalid email rejected | No | TBD |
| T-010 | Duplicate contact handling | **No** | **TBD** |

## 12. Known Issues / Caveats

- **No end-to-end submission test:** Automated tests stop short of clicking Submit to avoid creating fake leads. Manual testing required for full flow verification.
- **Phone formatting is JS-only:** If JS fails to load, phone field accepts unformatted input. Server-side validation status unknown.
- **Request type routing unknown:** It's unclear whether different request types trigger different workflows or assignments downstream.
- **Spam protection unknown:** Need to confirm what anti-spam measures are active (reCAPTCHA, honeypot, Akismet, etc.).
