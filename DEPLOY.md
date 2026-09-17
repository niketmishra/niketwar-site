# Putting raviwar on niketmishra.com, for free

You already own the domain. **Registration and hosting are separate things**, so
you can keep the domain at GoDaddy, pay them nothing further, and serve the site
from Cloudflare Pages at no cost.

- Repo: https://github.com/niketmishra/niketwar-site
- Target: `niketmishra.com/raviwar`
- Site weight: 4.7 MB per full visit, mostly the four clips

Cloudflare Pages was chosen over GitHub Pages for one reason: **bandwidth**.
GitHub Pages has a 100 GB per month soft limit, which at 4.7 MB a visit is only
about 21,000 visits. Cloudflare Pages has no bandwidth cap, so a launch spike
cannot knock the site over or get it throttled.

---

## Where things stand right now

```
nameservers   ns73.domaincontrol.com, ns74.domaincontrol.com   (GoDaddy default)
A records     15.197.225.128, 3.33.251.168                      (GoDaddy forwarding)
result        https://niketmishra.com  ->  301  ->  discord.gg/FKsWwPjb
```

That forwarding rule is why nothing you host is reachable yet. It is removed as a
side effect of step 2 below, so there is no separate step for it.

Note that the forwarded invite (`FKsWwPjb`) is **not** the game's Discord
(`ckWAKjyZt`). Worth deciding which one you actually want people landing in.

---

## 1. Create the Pages project

1. Sign up at **dash.cloudflare.com** (free, no card).
2. **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Authorise GitHub and pick **niketwar-site**.
4. Build settings, and this is the part people get wrong:

   ```
   Framework preset          None
   Build command             (leave completely empty)
   Build output directory    /
   ```

   There is no build step. The repo root is the site: `index.html` redirects to
   `/raviwar/`, and `raviwar/index.html` is the page itself.

5. **Save and Deploy.** You get a live URL like `niketwar-site.pages.dev` within
   a minute. **Open it and check the site works before touching DNS.** If
   something is wrong, it is much easier to find now than after the domain moves.

## 2. Move the domain to Cloudflare

1. In Cloudflare: **Add a site** → `niketmishra.com` → **Free** plan.
2. Cloudflare scans your existing DNS and shows what it found. **Delete the two
   A records** pointing at `15.197.225.128` and `3.33.251.168`. Those are the
   GoDaddy forwarding service, and they are the Discord redirect.
   Keep anything else you recognise, especially **MX records if you use email on
   this domain**. Losing those breaks your mail.
3. Cloudflare gives you two nameservers, something like `xxx.ns.cloudflare.com`.
4. In **GoDaddy** → My Products → Domains → niketmishra.com → **Nameservers** →
   **Change** → **I'll use my own nameservers** → paste Cloudflare's two.
5. Wait. Usually minutes, occasionally a few hours. Cloudflare emails you when
   the domain is active.

## 3. Attach the domain to the site

Back in **Workers & Pages** → your project → **Custom domains** → **Set up a
custom domain** → `niketmishra.com`. Cloudflare creates the DNS record itself and
issues the TLS certificate. Add `www.niketmishra.com` too if you want it to work.

Then `https://niketmishra.com/raviwar` is live, and the bare domain redirects to
it via the `_redirects` file in the repo root.

---

## Updating the site later

```bash
cd /Users/niket/RaviVerse/Website
git add -A && git commit -m "what changed" && git push
```

Cloudflare rebuilds and republishes on every push, usually inside a minute. No
dashboard visit needed.

---

## Worth knowing

**The trailer is a click-to-load facade.** YouTube is not contacted at all until
someone presses play, so the page stays fast and sets no third-party cookies
before then.

**Check it on a phone once it is live.** The four clips autoplay muted and loop;
that is the one thing most likely to behave differently on real iOS Safari than
in testing. They carry `playsinline` and `muted`, which is what iOS requires, and
each has a poster frame if a browser refuses to autoplay.

**Cloudflare's free analytics** are privacy-preserving and need no cookie banner.
Turn them on under the project's Analytics tab if you want to see whether the
Steam button is actually being clicked.

**If you ever want the bare domain to be a real page** rather than a redirect to
the game, replace the root `index.html`. The `_redirects` rule would need to go
at the same time, since it currently takes priority.
