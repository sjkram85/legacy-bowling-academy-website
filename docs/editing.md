# Editing the academy site

This repository is the public website only. Admin, billing, and customer records live in the Legacy Computing Services employee portal. Do not put private data, payment keys, or Apps Script secrets in this repo.

Links in `index.html` stay relative so the GitHub Pages project path keeps working. Do not add a CNAME file until the custom domain is ready.

## Where to paste the two URLs

Open `index.html` and find the academy desk config at the top of the page script:

```js
const ACADEMY_ENDPOINT = '';
const PAYMENT_LINK = '';
const FACEBOOK_URL = '';
const YOUTUBE_URL = '';
```

Paste each URL between the quotes. Leave a value blank until it exists. A blank value is honest: the page will not pretend a booking or a payment went through, and Facebook / YouTube cards show a "coming soon" pill until their full `https://` URLs are added.

Both values must be full `https://` URLs. Anything else is treated as blank.

### Booking and inquiry endpoint

`ACADEMY_ENDPOINT` is the deployed Google Apps Script web app URL (the `/exec` link), once that app exists in the portal project.

The form sends a `application/x-www-form-urlencoded` POST with `fetch` and `mode: 'no-cors'`, which a public Apps Script web app can accept. The browser cannot read the script's reply. A success message means the browser handed the request off, not that a receipt came back. Confirm a test submission in the portal.

While `ACADEMY_ENDPOINT` is blank, Book a lesson and Send an inquiry only prepare an email to info@legacybowlingacademy.com. The visitor has to send it. The on-page placeholder hides itself after a real https URL is saved.

Suggested Apps Script `doPost` reads `e.parameter` for the fields below. Deploy the web app so it runs as the script owner and is available to anyone, including anonymous visitors. The script itself does not belong in this repo.

### Payment link

`PAYMENT_LINK` is a Legacy Computing Services, LLC payment URL. Legacy Bowling Academy is a DBA of that company. It does not have its own bank account or merchant account. Paste the LCS payment link here. Do not paste a link that would make the academy look like it takes payment on its own.

While it is blank, the Pay section says online payment is coming soon and does not link out. When it is an https URL, that block is replaced by a button that opens the Legacy Computing Services payment page in a new tab. This site never collects card numbers.

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

## Placeholders to replace

Each of these is marked on the page with "Placeholder — add the real details before relying on this page". Delete the note when the real fact is in the page.

1. `placeholder-clinics` — next clinic or camp name, date, time, ages, group size, and price.
2. `placeholder-youth` — youth program age range, season dates, meeting day and time, group size, and price. Do not reuse the $60 or $75 private-lesson rate as that price.
3. `placeholder-program-prices` — clinic, camp, and youth-program prices in the Pay section. Delete this note when those prices are published.
4. `endpoint-placeholder` — hides on its own once `ACADEMY_ENDPOINT` is a real https URL. You can also delete the paragraph.
5. `pay-link-placeholder` — hides on its own once `PAYMENT_LINK` is a real https URL, along with the "coming soon" block.

The two constants at the top of the script are the other blanks: the endpoint URL and the payment link.

## Adding coaches as the roster expands

Scott J. Kramer is currently listed as the sole coach, paired with an "Expanding Our Coaching Staff" card and inquiry button in `#staff`. When additional USBC-certified coaches join, add new `.coach-card` articles inside `.staff-grid` and add their names to the `#preferred-coach` select menu.
