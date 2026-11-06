# USTBites

Canteen food, brought to your floor by a hallmate. A working prototype for HKUST ISOM 2010 (Group 7).

A student orders at the canteen as usual, posts the pickup number in USTBites, and a runner from the same hall who is heading back anyway brings it to their door. The handoff is confirmed with a 4-digit code.

## What works

- Sign-up limited to `@connect.ust.hk` emails (email verification is not built yet)
- Profile with hall and role: order, deliver or both
- Request form: canteen, pickup number, floor and room, Priority (+HK$5)
- Pricing: HK$12, HK$15 in the lunch (11:30–14:00) and dinner (17:30–20:30) rush, Hong Kong time
- Runners join their hall's pool by turning on "Available"; requests only go to that hall
- The first runner to accept locks the request; the customer sees who accepted it
- Statuses: Requested → Accepted → Picked up → Delivered, saved in the database (survive reloads)
- Delivery closes only with the customer's 4-digit code
- Two-way ratings; runner earnings = fee − HK$2; ratings of 2 or less are flagged for the team
- "No runner after 10 minutes" warning with cancel
- A/B test: each request is randomly assigned A (all runners at once) or B (one runner first for 3 minutes)
- Team data: event log, orders table, A/B results, CSV downloads, test-data reset

Not built: payments (customers pay runners directly), USThing login, live tracking, canteen integration.

## Set up your own copy (about 15 minutes)

The page is hosted free on GitHub Pages. The shared data lives in a free Firebase Firestore database, so every phone sees the same orders live.

### 1. Create the Firebase database

1. Go to <https://console.firebase.google.com> and sign in with a Google account.
2. Click **Create a project** (or **Add project**). Name it `ustbites`. You can turn Google Analytics off. Click **Create project**.
3. In the left menu open **Build → Firestore Database** and click **Create database**.
4. Pick the location **asia-east2 (Hong Kong)** and start in **production mode**. Click **Create**.
5. Open the **Rules** tab, delete what's there, paste the contents of [`firestore.rules`](firestore.rules) from this repository, and click **Publish**.

### 2. Get the web config

1. Click the gear icon next to **Project Overview → Project settings**.
2. Under **Your apps**, click the web icon `</>`. Nickname: `ustbites-web`. Leave Firebase Hosting unticked. Click **Register app**.
3. You'll see a `firebaseConfig = { apiKey: ..., authDomain: ..., ... }` block. Copy the values.
4. In this repository, open `config.js`, click the pencil (Edit), replace each `PASTE_...` value with yours, and click **Commit changes**.

These values only identify your project; they are not passwords. What people can do is controlled by the Firestore rules.

### 3. Turn on GitHub Pages

1. In this repository go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**, branch **main**, folder **/ (root)**. Click **Save**.
3. Wait 1–2 minutes, then refresh the page. The link appears at the top: `https://<your-username>.github.io/USTBites/`.

### 4. Use it on phones

- Open the link on each phone. On iPhone: Share → **Add to Home Screen**. On Android: menu → **Add to Home screen**. It then opens like an app.
- Each phone keeps its own sign-in, so one phone = one person. For usability tests, a tester signs up on a team phone with their own HKUST email, and you tap **Profile → Switch account** afterwards.
- Before your real tests: **Profile → Team data → Download event log (CSV)**, then **Reset test data**.

## Privacy and limits

- The rules let anyone with the link read and write USTBites data. That's fine for a class test with people you know. Use first names, test room numbers where possible, and delete the Firebase project after the course (Project settings → Delete project).
- Anyone who knows a classmate's email can sign in as them, and the handoff code is stored in the shared database. These are prototype shortcuts, not real security.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole app (HTML, CSS and JavaScript) |
| `config.js` | Your Firebase project config |
| `firestore.rules` | Database rules to paste into Firebase |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |
