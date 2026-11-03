# ג'לטו אהוד — setup (all done while logged in as Ehud's Google account)

## Quick way: run setup.gs (replaces sections 1 and 3)
1. script.google.com → New project → delete the sample code → paste `setup.gs` (in the folder above `site/`).
2. Pick `setup` in the function menu → Run → allow access ("unverified app" → Advanced → Go to project; it's his own script).
3. View → Logs (Execution log): copy the `FORM` / `MENU_CSV` block over the same lines in `index.html`.
4. He gets an email for every order. The menu is its own sheet ("תפריט ג'לטו"), shared view-only, so the orders sheet stays private.

The manual steps below do the same thing by hand.

## 1. Google Form → Sheet (the backend)
1. forms.google.com → new blank form "הזמנות ג'לטו".
2. Add 6 **Short answer** questions, in this order: שם, טלפון, פירוט, מספר אריזות, סה״כ, הערה.
   Leave them all non-required (the page validates).
3. Responses tab → "Link to Sheets" → create a new spreadsheet. That sheet is the order list.
4. Settings → Responses: make sure "Collect email addresses" is OFF and "Restrict to users in…/require sign-in" is OFF.
5. Get the ids: ⋮ menu → "Get pre-filled link" → type `x` in every field → Get link → copy.
   The link looks like `.../forms/d/e/FORM_ID/viewform?entry.111=x&entry.222=x...`
6. In `index.html`, fill in `FORM`:
   - `action`: same URL with `viewform?...` replaced by `formResponse`
   - `entries`: the `entry.NNN` names, in the question order above.
7. Turn on notifications: in the Sheet, Tools → Notification settings → "When a form is submitted" → email.

## 2. Hosting (free, under Ehud's email)
Easiest: **Netlify Drop** — sign up at netlify.com with "Sign up with email" (Ehud's address, not GitHub),
then drag the `site/` folder onto app.netlify.com/drop. Rename the site in Site settings
(e.g. `gelato-ehud.netlify.app`). To update later, drag the folder again on the Deploys tab.

## 3. Menu & prices from the Sheet
1. In the orders spreadsheet, add a new tab named `תפריט` with this header row:

   | קבוצה | טעם | מחיר |
   |---|---|---|
   | חלבי | פיסטוק | 100 |
   | טבעוני | מנגו | 100 |

   One row per flavor. Groups show in the order they first appear. Delete a row to hide a flavor.
   Empty מחיר = 100 ₪.
2. File → Share → **Publish to web** → choose the `תפריט` tab (not "Entire document") → **CSV** → Publish → copy the link.
3. Paste it into `MENU_CSV` in `index.html`.

Changes in the sheet show on the site within ~5 minutes (Google caches the published CSV).
If the sheet can't be reached, the page falls back to the built-in brochure menu.
