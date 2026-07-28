# IronForge Components — Customer Support Chatbot

Built for NCI H9CEAI CA2 ("Build It Live, Prove It's Real"). Assigned business (student ID ending in 3):
**IronForge Components** — industrial manufacturing supply. Paired public API: **UK Carbon Intensity**
(api.carbonintensity.org.uk).

## How it works

Single static page (`index.html`), no backend, no API key anywhere in the code:

- **Brain**: [Puter.js](https://developer.puter.com/) — a free, client-side call to a real hosted LLM.
  No signup key is embedded in the code; the visitor's browser talks to Puter directly. The first time
  a visitor sends a message, Puter may show a one-time free sign-in popup (no credit card) — that's
  Puter's own usage model, not something this app controls.
- **Live tool 1 — Google Sheet**: fetched fresh on *every single question* via
  `https://docs.google.com/spreadsheets/d/<SHEET_ID>/gviz/tq?tqx=out:csv` with a cache-busting
  timestamp and `cache: 'no-store'`. Nothing from the sheet is copied into the code — only the sheet ID
  is in the code, the values always come from a live HTTP request made at the moment of the question.
- **Live tool 2 — UK Carbon Intensity API**: fetched fresh the same way from
  `https://api.carbonintensity.org.uk/intensity`, no key required.
- **Live data log panel**: the collapsible "Live data log" under the chat box timestamps every real
  fetch made for every question — use it (or the browser Network tab) as your evidence that the
  connection is live and not cached/hardcoded.

## Run it locally before deploying

Browsers block `fetch()` from a bare `file://` page for some of this, so serve it locally:

```bash
cd ironforge-chatbot
python3 -m http.server 8000
```

Open `http://localhost:8000` and try the four quick-question buttons (off-topic probe, price/stock,
carbon intensity, and the combined sheet+API question).

## Deploy to GitHub Pages

1. Create a **new public GitHub repository** (e.g. `ironforge-chatbot`) on your own GitHub account.
2. Push this folder to it:
   ```bash
   cd ironforge-chatbot
   git init
   git add index.html README.md
   git commit -m "IronForge Components live chatbot for H9CEAI CA2"
   git branch -M main
   git remote add origin https://github.com/<your-username>/ironforge-chatbot.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Source: Deploy from branch → Branch: main / (root)** → Save.
4. Your live URL will be `https://<your-username>.github.io/ironforge-chatbot/` — that's the link you
   submit (github.io format, not the repo link, per the brief).
5. Keep the repo public and the Pages site live for at least 4–8 weeks after submission — the lecturer
   re-tests your live URL after the deadline by changing the Google Sheet values.

If you'd rather I push it for you, install and authenticate the GitHub CLI (`gh auth login`) or share
repo write access, then tell me and I'll run the push commands — I don't have your GitHub credentials
so I can't do this without one of those.

## What you still have to do yourself (per the brief)

The brief is explicit: the AI can build the plumbing, but **the proof, the judgement, and the
reflection must come from your own contact with your own deployed system**. I've built the app so all
of this is possible — you need to actually click through it, screenshot it, and write the analysis in
your own words.

**Task 1 — brain proof**: Open your live URL, ask the off-topic probe ("Can I order food?") and 2–3
more off-script questions (jokes, general knowledge, something absurd). Screenshot the replies.

**Task 2 — live data proof**: Ask about a price/stock/lead time for a specific part. Screenshot the
reply next to the actual Google Sheet value, and screenshot the "Live data log" panel (or your
browser's Network tab) showing a fetch happened at that moment.

**Task 3 — interrogate the data**: Open the Google Sheet yourself and find the deliberately implausible
value and/or the zero-stock item (I did not remove or flag them in the code — the bot sees the raw
sheet). Ask the bot about that exact item. Screenshot the answer. Then write, in your own words:
did it report the value faithfully or quietly "correct" it? Which of Grice's four maxims (quality,
quantity, relation, manner) is at stake, and why? Then give your own judgement on when a
customer-facing bot should report vs. caveat vs. escalate to a human.

**Task 4 — second live tool**: Use the "⚡ Carbon intensity" and "🔗 Combined" quick buttons (or ask
your own version), screenshot the answer, and note that it's genuinely business-relevant (scheduling
energy-intensive CNC/machining runs against grid carbon intensity).

**Task 5 — reflection (200–300 words, your own voice)**: what the off-topic probe told you about the
brain being real, how you know the data connection is live (point to the log panel / Network tab /
the post-deadline swap test), and which specific value you distrusted and why. Add one user-facing
disclosure sentence (EU AI Act Article 50 — I've already put a permanent one in the app footer/banner
as a starting point, but the brief wants you to state it in your submission doc too) and name one
data-protection risk of wiring a live chatbot to a shared Google Sheet (e.g. the sheet may contain more
than intended, access isn't scoped per-user, no audit trail of who asked what, etc. — pick the one you
actually believe and justify it).

## AI tools used (disclose this in your submission)

- Claude (Anthropic, via Claude Code) — used to read the brief, design the architecture, and write all
  of `index.html`.
- Puter.js — third-party free hosted LLM used as the chatbot's live brain at runtime.

State this disclosure in your submission document as required by the AI Usage Policy.
