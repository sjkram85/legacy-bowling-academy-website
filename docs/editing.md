# Editing the academy site

This repository is the public website only. Admin, billing, and customer records live in the Legacy Computing Services employee portal. Do not put private data, payment keys, or Apps Script secrets in this repo.

Links in `index.html` stay relative so the GitHub Pages project path keeps working. Do not add a CNAME file until the custom domain is ready.

## Where to paste the URLs

Open `index.html` and find the academy desk config at the top of the page script:

```js
const ACADEMY_ENDPOINT = 'https://legacy-control-center-api.legacycomputing.workers.dev/api/public/academy';
const PAYMENT_LINK = 'https://venmo.com/u/LegacyBrands';
const FACEBOOK_URL = 'https://www.facebook.com/profile.php?id=61596557673263';
const YOUTUBE_URL = 'https://www.youtube.com/@LegacyBowlingAcademy';
```

Paste each URL between the quotes. Leave a value blank until it exists. A blank value is honest: the page will not pretend a booking or a payment went through, and Facebook / YouTube cards show a "coming soon" pill until their full `https://` URLs are added.

Both values must be full `https://` URLs. Anything else is treated as blank.

### Booking and inquiry endpoint

`ACADEMY_ENDPOINT` currently points to the Cloudflare Worker endpoint (`https://legacy-control-center-api.legacycomputing.workers.dev/api/public/academy`) backed by Cloudflare D1 (`Academy → Requests` in the Control Center).

The form sends a JSON `POST` request with real CORS and shows a verified confirmation reference ID (`{ ok: true, id }`) once saved. Legacy Google Apps Script `/exec` URLs are also still supported automatically via `no-cors` form POST if `ACADEMY_ENDPOINT` is ever switched to `script.google.com`.

While `ACADEMY_ENDPOINT` is blank, Book a lesson and Send an inquiry prepare a pre-filled email to info@legacybowlingacademy.com. The visitor has to send it.

### Payment link

`PAYMENT_LINK` currently points to the Venmo Business account `@LegacyBrands` (`https://venmo.com/u/LegacyBrands`), which operates under Legacy Computing Services, LLC. Legacy Bowling Academy is a DBA of that company and does not have its own bank account or merchant account. Venmo Business is currently the sole accepted payment method until additional payment options are set up.

While `PAYMENT_LINK` is blank, the Pay section shows a "Book a time first" fallback and does not link out. When it is an https URL, `#pay-live` displays the Venmo Business button that opens `PAYMENT_LINK` in a new tab. This site never collects card numbers.

Do not add prices that are not already published in the Sessions section. Private lessons on the page today are $60 or $75 a session, plus lane lineage paid at Forest Hill Lanes. Clinic, camp, and youth-program prices are not published.

## What the form sends

Every request uses these fields:

| Field | Value |
| --- | --- |
| form | `booking` or `inquiry` |
| name | Visitor's name |
| email | Visitor's email |
| phone | Phone number (required for a booking) |
| ageGroup | `youth`, `adult`, or `senior` |
| program | `private lesson`, `clinic-camp`, or `youth program`. An inquiry that is not a program request may send this empty. |
| preferredTimes | Days and times that work |
| goals | What they want to work on. A private lesson also includes the session focus here. |
| message | Anything else they wrote |
| source | `legacy-bowling-academy` |
| website | Honeypot. It must stay empty. If it is filled, the page does not send the request. |
| preferredCoach | Extra field. Best Match, Scott J. Kramer, or Academy Coaching Staff. |

`website` is a hidden honeypot, not a real question. Leave that field in the form.

## Remaining items to update when ready

All visible public placeholder banners have been removed from `index.html`. Update these items in `index.html` when ready:

1. `ACADEMY_ENDPOINT` — paste the Google Apps Script `/exec` URL at the top of `<script>` to enable direct form submission instead of the email-prep flow.
2. `#clinics` and `#youth` — replace the interest-list copy with real dates, times, age ranges, group sizes, and pricing once scheduled (do not reuse the $60 or $75 private-lesson rate for group programs).
3. `#testimonials` — currently commented out in `index.html`; uncomment and populate once 2–3 real bowler stories are ready to publish.
4. `#pay` / `PAYMENT_LINK` — update when additional payment options beyond Venmo Business (`@LegacyBrands`) are ready.

## Adding coaches as the roster expands

Scott J. Kramer is currently listed as the sole coach, paired with an "Expanding Our Coaching Staff" card and inquiry button in `#staff`. When additional USBC-certified coaches join, add new `.coach-card` articles inside `.staff-grid` and add their names to the `#preferred-coach` select menu.
