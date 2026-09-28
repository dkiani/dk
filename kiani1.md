# Adding kia.ni as a custom domain

Goal: point kia.ni at the site so the Instagram bio link can change from
kiani.vc to kia.ni. kiani.vc stays active and is not affected.

Domain purchased from 101domain. Site is hosted on Vercel (project `dk`).
No code changes are needed in the repo. Everything happens in two dashboards.

---

## Step 1: Add the domain in Vercel

1. Open the Vercel dashboard and select the `dk` project.
2. Go to Settings, then Domains.
3. Add `kia.ni`.
4. When asked about `www.kia.ni`, add it and choose to redirect it to `kia.ni`.
5. Vercel will show "Invalid Configuration" until DNS is set. That is expected.

## Step 2: Point DNS at Vercel from 101domain

Pick one option. Option A is simpler and recommended.

### Option A: Use Vercel's nameservers

In 101domain, open kia.ni, find Nameservers, and replace them with:

    ns1.vercel-dns.com
    ns2.vercel-dns.com

### Option B: Keep 101domain's DNS and add records

In 101domain's DNS manager for kia.ni, add:

    Type   Host   Value
    A      @      76.76.21.21
    CNAME  www    cname.vercel-dns.com

## Step 3: Wait, then verify

1. Go back to the Vercel Domains page and click Refresh.
2. Once it shows a green check, Vercel issues the SSL certificate automatically.
3. kia.ni is live. Update the Instagram bio link.

---

## Things to know

- .ni is Nicaragua's country domain. Its registry is slower than .com.
  Nameserver changes can take 24 to 48 hours to propagate. Don't panic if
  Vercel still shows an error after an hour.

- These instructions assume kia.ni should show the portfolio site in this
  repo. If kia.ni should instead open the kiani.vc curriculum platform, skip
  the Vercel step and set the DNS records to match whatever host kiani.vc
  points to.

## Why Claude couldn't do this directly

The Claude Code cloud session runs in an isolated Linux container with a clone
of the repo and a terminal. It has no connection to the user's screen, mouse,
or browser, and no Vercel or 101domain credentials.

Options for letting Claude help more next time:

- Use the Claude in Chrome extension to drive the logged-in browser.
- Add a `VERCEL_TOKEN` secret to the Claude Code environment settings and
  start a new session. Claude can then use the Vercel CLI to add the domain
  to the project. The 101domain DNS side still needs to be done by hand,
  since 101domain has no CLI.
