# Teja & Deeksha — Wedding Invitation (office / colleagues)

A single-page, fully static invitation. Separate from the family invitation at
`tejadeekshawedding`, with its own repo and its own URL — **the two share nothing but the RSVP
form**.

**Live site (once Pages is enabled):** https://balasubramaniteja.github.io/tejadeekshawedding_US/

## What this version is

A plain Western wedding invitation for colleagues: one ceremony, one venue, one RSVP.

| | This version | The family version |
|---|---|---|
| Events shown | Wedding day only (21 Oct) | Haldi, Pellikoduku & Pellikuthuru, Wedding |
| Music | none | background track, opens behind a play button |
| Story animation | none | California → Kansas map video |
| Styling | ivory, charcoal, sage — English/Western | maroon, gold, cream — South Indian |
| Ornament | laurel wreath, engraved frame, corner flourishes | toran, muggu, thalambralu, deity artwork |
| Opens | straight to the invitation | behind a tap-to-open gate |
| RSVP | **the same Google Form** | the same Google Form |

There is no audio or video anywhere in this file, and nothing loads from a third party — the fonts
ship with the site, so it renders identically on a corporate network that blocks Google Fonts.

## How it is put together

It is styled as engraved stationery rather than a web page: three cream cards on a linen ground,
each with a fine double rule, gold corner flourishes and generous margins.

- **Great Vibes** carries the names, the section headings and the monogram — a script face is the
  single strongest signal of a Western wedding invitation. **Marcellus** does the letterspaced small
  caps, **Cormorant Garamond** the body.
- **The laurel wreath** at the top is generated, not hand-drawn: `wreathHalf()` walks a circle from
  168° down to 26° (measured clockwise from twelve o'clock), placing a leaf at each step, rotated
  `t - 90 + 22` so it sits along the tangent and splays outward. Only the right half is built — the
  markup mirrors it with `translate(160 0) scale(-1 1)`, which is why the two sides match exactly.
  Change the leaf count, taper or splay in that one function.
- **The paper grain** is an inline SVG `feTurbulence` data URI, so there is no texture image to load.
- **The date is spelled out** — *The Twenty-First of October, Two Thousand Twenty-Six, at ten
  o'clock in the morning* — which is the traditional wording. The numeric form appears on the
  details card, where people actually read times.

Nothing loads from a third party: no icon fonts, no CSS framework, no images at all.

## Details shown

```
Wednesday, 21 October 2026
From 9:00 AM onwards
  10:00 – 11:30 AM   Wedding ceremony
  Followed by        Lunch
6330 Lackman Rd, Shawnee, KS 66217
```

The heading is "Wedding ceremony" rather than "Muhurtham" — this audience will not know the term.

**RSVP deadline is 10 October 2026**, not the 1 October used on the family invitation, because this
one goes out later. Change it in two places if you want a different date: the visible line in the
RSVP card, and nowhere else (it is not used by any script).

## Files

- `index.html` — the whole site: HTML, CSS, JS and every SVG, all inlined. No build step.
- `assets/fonts/*.woff2` — self-hosted Great Vibes, Marcellus and Cormorant Garamond
  (7 files, ~170 KB).
- `.nojekyll` — tells GitHub Pages to serve the files as-is.

Total page weight is about 150 KB.

## RSVP — shared with the family invitation

Both sites POST into the **same Google Form**, so every reply lands in one sheet:

```
form:      1FAIpQLSddhxROwwf6gTaP7TP2h-wGorTD5r7ts9ZfE9lPKRvSoks2pg
name       entry.374166117
attending  entry.426617619
guests     entry.669232451
events     entry.1665966449
message    entry.248071712
```

Two rules that matter more here than usual, because a mistake is invisible:

1. **Every posted value must match a form option character for character.** Google silently discards
   anything it does not recognise, and the page cannot see that it happened — the guest still gets a
   thank-you message. The values in use are `Yes, I can make it.`, `Unfortunately, I can't make it.`
   and `Wedding`. Note the full stops, and that the apostrophe in the posted value is a plain ASCII
   `'` (the visible label uses a curly `'` because it reads better).
2. **Keep every question except *Your name* not required.** A required question whose answer is
   rejected takes the whole submission down with it.

This invitation covers one event, so there is no events checkbox — the value `Wedding` is sent
automatically, and only when the guest accepts. A decline sends no events at all, which is why
question 4 must not be required.

**Replies from both invitations look identical in the sheet.** There is deliberately no marker
saying which site a reply came from; you tell them apart by name. If you later want a proper
column, add a sixth question to the form and send me its `entry.N` id.

### When a reply never reaches the sheet

Add `?rsvpdebug=1` to the URL:

```
https://balasubramaniteja.github.io/tejadeekshawedding_US/?rsvpdebug=1
```

The reply is then posted into a **visible new tab** instead of the hidden iframe, so you can read
Google's actual response. A "Your response has been recorded" page means the row is in the sheet; an
error page names the question it rejected. Guests never see this unless they type it themselves.

## Reminders

**Add to calendar** produces an `.ics` carrying `VALARM` blocks, so the guest's own phone fires the
reminders — no push permission, no service worker, no backend:

| | |
|---|---|
| Wedding, 21 Oct | 1 week before · 1 day before · 2 hours before |

Edit them in the `alarms` array on `W.event`. The wording each shows comes from the `ALARM_TEXT` map
above the calendar handler, keyed by the same duration string, so **a new duration needs an entry in
both places** or the reminder reads "undefined".

Apple Calendar and Outlook honour these reliably. Google Calendar sometimes replaces imported alarms
with the account's own default, so a Google Calendar guest still gets *a* reminder, possibly not at
these exact offsets.

## Editing

All the text is in the HTML — search for the words you want to change. The only values used by
scripts live in the `const W = { … }` block near the bottom:

- `event` — the single calendar entry the "Add to calendar" button produces
- `googleForm` — the shared form's action and its five field ids
- `eventValue` / `acceptValue` — the two strings that must match form options exactly

**"Shawnee Venue" is a placeholder.** Replace it with the hall's real name when you have it; the
street address below it is correct either way, and the directions link routes from the address.

## Deploying to GitHub Pages

```bash
git clone https://github.com/Balasubramaniteja/tejadeekshawedding_US.git
cd tejadeekshawedding_US
# copy index.html, README.md, .nojekyll and assets/ in here
git add -A
git commit -m "Office wedding invitation"
git push origin main
```

Then **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` /
`/ (root)` → Save.** Live in about a minute.
