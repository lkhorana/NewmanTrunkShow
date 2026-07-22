# Newman Exclusive — Trunk Show Scheduler (Standalone Site)

This is a separate, small website — not part of your main `NewmanExclusive` repo, not linked from your main site's nav. It's only used when your dad is traveling for a trunk show, and only shared as a direct link.

It still uses your **same Supabase project and PromptPay setup** — it just pulls the config live from your main site (`newmanexclusive.com/js/config.js`) instead of duplicating it, so you only ever maintain one copy of your Supabase keys and PromptPay ID.

## What's in this folder

- **`trunkshow-admin.html`** — your dad (or you) logs in, creates a trunk show, gets a shareable booking link, blocks off already-confirmed slots, watches bookings come in.
- **`trunkshow-booking.html`** — the link he sends to customers.
- **`trunkshow-schema.sql`** — database tables for this feature (run once in Supabase).

No `js/` folder needed here — both pages load `config.js`, `promptpay.js`, and `ics.js` directly from `https://www.newmanexclusive.com/js/...`, which is already live from the payment system setup.

## Setup steps

**1. Run the schema (one-time).** In Supabase -> SQL Editor -> New query, paste and run all of `trunkshow-schema.sql`. This adds three new tables alongside your existing ones — nothing else is touched.

**2. Create a new GitHub repo.**
- Go to github.com -> click the **+** in the top right -> **New repository**
- Name it something like `NewmanTrunkShow`
- Set it to **Public** (GitHub Pages needs this on the free plan)
- Don't add a README/gitignore — leave it empty
- Click **Create repository**

**3. Enable GitHub Pages.**
- In the new repo, go to **Settings -> Pages**
- Under "Build and deployment" -> Source, select **Deploy from a branch**
- Branch: `main`, folder: `/ (root)` -> **Save**
- GitHub will show you the live URL, something like `https://lkhorana.github.io/NewmanTrunkShow/`

**4. Upload the two files.**
- On the repo's main page, click **Add file -> Upload files**
- Drag in `trunkshow-admin.html` and `trunkshow-booking.html`
- Commit directly to `main`

**5. Test it.**
- Visit `https://lkhorana.github.io/NewmanTrunkShow/trunkshow-admin.html`
- Log in with the same staff account you use for your payment admin
- Create a test trunk show, copy the booking link it gives you
- Open that link in another tab, walk through a test booking, confirm the `.ics` file downloads

## How your dad uses it day-to-day

1. Before a trip, go to `trunkshow-admin.html`, create the trunk show for that city/dates.
2. Copy the booking link, send it to customers directly (WhatsApp, email — wherever he normally reaches them).
3. Block off any slots already arranged the old way.
4. Check back on the admin page to see bookings as they roll in.

## Notes

- Since this pulls `config.js` from your live main site, if you ever update your Supabase keys or PromptPay ID there, this scheduler picks up the change automatically — no need to update anything here.
- Like the payment system, there's no push notification yet for new bookings — check `trunkshow-admin.html` directly. The same email-notification add-on could cover both systems at once, whenever you want it built.
