# PRD — IE-nergy Spring Campaign Landing Page

## Product

A single-page landing site for the **IE-nergy Spring Campaign** — a fictional energy drink targeting IE Business School students. The page has one job: capture qualified signups for the spring launch and deliver them into Mailchimp with verified consent.

## Form fields

All four fields are **required**.

| Field | Type | Details |
|---|---|---|
| `name` | text input | Free text, min 1 character after trimming. Maps to Mailchimp merge field `FNAME`. |
| `email` | email input | Must match a valid email format. Maps to Mailchimp primary email address. |
| `campus` | dropdown | Options: `Madrid`, `Segovia`, `online`. Maps to Mailchimp merge field `CAMPUS`. |
| `preferred_flavor` | dropdown | Options: `Arctic Mint`, `Citrus Storm`, `Berry Voltage`. Maps to Mailchimp merge field `FLAVOR`. |

Plus one consent control:

| Control | Type | Details |
|---|---|---|
| `gdpr_consent` | checkbox | Required. Unchecked by default. Label: *"I agree to receive marketing emails from IE-nergy."* Maps to the Mailchimp GDPR marketing permission configured on the audience. |

## Behaviour on submit

1. Client-side validation runs: all four fields populated, email format valid, GDPR checkbox ticked.
2. If validation passes, the form POSTs to the Mailchimp public embedded-form endpoint for the "IE-nergy Spring Campaign" audience (action URL produced by `mailchimp-setup.js` in Step 4).
3. Mailchimp adds the contact with `status = pending` and triggers its **double opt-in** email automatically.
4. The page swaps the form out for a **thank-you state**: a short headline (e.g. *"Check your inbox."*), a line explaining the confirmation email, and the hero can image still in place.
5. The contact only becomes `subscribed` in Mailchimp once they click the confirmation link.

## Acceptance criteria

The feature is not done until **all** of the following are true:

1. **GDPR consent is enforced.** The checkbox is required, unchecked by default, and the submit button must not fire a network request unless the checkbox is ticked. A visible inline error appears if the user tries to submit without ticking it.
2. **Mailchimp double opt-in is enabled** on the "IE-nergy Spring Campaign" audience. A new signup lands in Mailchimp as `pending`, receives a confirmation email, and only flips to `subscribed` on link click.
3. **All four form fields are required.** Submitting with any of `name`, `email`, `campus`, or `preferred_flavor` blank surfaces an inline error next to the blank field and blocks submission.
4. **The email field validates format** before submit (standard HTML `type="email"` or equivalent regex — no manual workaround).
5. **The thank-you state replaces the form** on successful submit — the form is not re-shown.
6. **No secret material is present in the deployed HTML.** The Mailchimp API key from `.env` must never reach the static site.

## Edge cases

| Case | Expected behaviour |
|---|---|
| **Invalid email format** (e.g. `ana@`, `ana.com`) | Browser-native validation message appears on the email field. Submission blocked. No network request. |
| **GDPR consent refused** (checkbox unticked) | Inline error beside the checkbox: *"You need to agree before we can sign you up."* Submission blocked. No network request. |
| **Mailchimp API error on submit** (network failure, 4xx/5xx from Mailchimp) | The form stays visible. A non-dismissive error message appears above the submit button: *"Something broke on our side. Try again in a minute — your details haven't been sent yet."* No silent failure, no partial thank-you state. |
| **User already subscribed** | Mailchimp returns a `Member Exists` response. Treat as success for the user — show the thank-you state. Do not surface Mailchimp's raw error. |
| **User closes tab before confirming double opt-in** | Nothing to do client-side. Contact stays `pending` in Mailchimp; their resend policy handles re-prompting. |

## Out of scope (for this lab)

- Server-side validation or rate limiting (the form hits Mailchimp directly).
- A/B testing of copy or layout.
- Analytics integration (GA4, etc.).
- Localization beyond the Spanish-English code-switching already allowed by `CLAUDE.md`.
