# Publishing raviwar to niketmishra.com/raviwar

The whole site is `Website/raviwar/`: one `index.html` and an `assets/` folder,
4.3 MB total. No build step, no dependencies, no server code. Any static host
will serve it as-is.

---

## Step 0, and nothing works until this is done

**niketmishra.com currently forwards to Discord.**
`https://niketmishra.com` 301-redirects to `https://discord.com/invite/FKsWwPjb`,
which is a GoDaddy **domain forwarding** rule, not a website.

While that rule exists, every request to the domain bounces to Discord before it
reaches any host. Remove it first:

> GoDaddy → My Products → Domains → niketmishra.com → **DNS** → find the
> **Forwarding** section → delete the rule.

Note also that the forwarded invite (`FKsWwPjb`) is **not** the game's Discord
(`ckWAKjyZt`). Worth checking which one you actually want people landing in.

---

## Then pick a host

### Option A, Cloudflare Pages. Recommended.

Free, fast worldwide, free TLS, and you have used it before on other projects.

1. Put `Website/` in a git repo and push it (GitHub works).
2. Cloudflare dashboard → **Workers & Pages** → Create → **Pages** → connect the repo.
3. Build command: **leave empty**. Build output directory: **`/`** (the repo root,
   so that `raviwar/index.html` resolves at `/raviwar`).
4. Deploy, then **Custom domains** → add `niketmishra.com`.
5. Cloudflare will tell you to point the domain at its nameservers. Do that in
   GoDaddy under **Nameservers → Change → I'll use my own**.

Nameserver changes take anywhere from minutes to a few hours.

### Option B, GoDaddy hosting, if you already pay for it

Only works if you have a **Web Hosting** plan, not just the domain.

cPanel → **File Manager** → `public_html` → create a folder `raviwar` → upload
the contents of `Website/raviwar/` into it. Upload the zip and use Extract;
uploading 18 files one at a time is miserable.

### Option C, GitHub Pages

Free and fine. Push `Website/` to a repo, Settings → Pages → deploy from branch
root, then add `niketmishra.com` as the custom domain and set the GoDaddy DNS
records GitHub gives you. Slightly more DNS fiddling than Cloudflare.

**Do not use GoDaddy Website Builder.** It cannot host hand-written HTML.

---

## Before you publish

- [x] **Buy Me a Coffee** is set to `buymeacoffee.com/raviwar` in three places.
- [ ] **Steam link.** Points at `store.steampowered.com/app/1327256/`, which
      404s until the store page is published. That is fine; it starts working by
      itself the moment the page goes live.
- [ ] Decide what lives at the **root** of niketmishra.com. Right now only
      `/raviwar` exists, so the bare domain would 404. Either redirect the root
      to `/raviwar`, or write a one-page index later.

## After it is live

Check the page on a phone. The four clips autoplay muted and loop, which is the
one thing most likely to behave differently on real iOS Safari than in testing.
They carry `playsinline` and `muted`, which is what iOS requires, and a poster
frame shows if a browser refuses to autoplay at all.
