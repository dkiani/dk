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

Keep 101domain's own nameservers (ns1/ns2.101domain.com) and edit the DNS
records. In 101domain, open kia.ni, go to DNS Records, and make the zone look
like this, nothing else:

    Name     TTL      Type   Value
    kia.ni   6 hours  A      76.76.21.21
    www      6 hours  A      76.76.21.21

Delete the default parking and privateemail records (the 52.60.87.163 A
records, both MX records, the SPF TXT, and the default._domainkey DKIM TXT).

Why not Vercel's nameservers: the .ni registry runs a zone check before it
will delegate to new nameservers, and it refuses any nameserver that is not
already serving an SOA record for the domain. Vercel's nameservers do not
serve the zone until after delegation, so the change fails with
"Zone Check Failed, No SOA Record Returned". The A record route avoids the
check entirely. This was done on 13 Oct 2026 and propagated the same day.

## Step 3: Wait, then verify

1. Go back to the Vercel Domains page and click Refresh.
2. Once it shows a green check, Vercel issues the SSL certificate automatically.
3. kia.ni is live. Update the Instagram bio link.

---

## Things to know

- .ni is Nicaragua's country domain. Registration itself took four days to
  be approved by the registry (submitted 9 Oct, active 13 Oct 2026).

- kia.ni is attached to the Vercel project `dk1`, the same project that
  serves the kiani.vc home page. kia.ni therefore shows exactly what
  kiani.vc shows. It is not attached to the `dk` portfolio project.

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
