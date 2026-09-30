# CloakWatch — automated scanner information

If you found this page because **`CloakWatch/1.0` appeared in your web server logs**, this
explains who we are and what we were doing.

## What CloakWatch is

CloakWatch is an academic measurement project studying **cloaked gambling SEO-spam on
India's public-sector web** (`.gov.in`, `.nic.in`, `.ac.in`, `.edu.in`).

Indian government and university websites have been hijacked in repeated waves to promote
illegal betting and gambling apps. The attack uses *cloaking*: the compromised server
checks who is asking and serves different content to search engine crawlers, to mobile
visitors arriving from Google, and to administrators visiting directly. The result is that
the site's own staff typically see nothing wrong, while the domain's search ranking is used
to promote gambling spam and mobile visitors are redirected to betting apps.

We measure how widespread this is, how long it takes to get fixed, and which techniques are
used. Findings are reported to affected site owners and to CERT-In.

## What our scanner does

- Sends **HTTP GET requests** for publicly accessible pages, the same way a browser does.
- Requests a small number of pages per site — typically the home page, plus URLs your own
  `sitemap.xml` declares.
- Requests each page from a few different browser profiles, because comparing them is how
  cloaking is detected at all.
- Records the response: status code, headers, redirects, page title and visible text.
- Rate-limits itself to **no more than one page load every 5 seconds per site**, and backs
  off when a server returns 429 or 503, honouring `Retry-After`.

## What our scanner never does

- **No logins, no form submissions, no password guessing.**
- **No vulnerability scanning, exploitation, or any attempt to gain access.**
- **No directory brute-forcing or guessed URLs.** We only request your home page and URLs
  your own sitemap or robots.txt points at.
- **No downloading of applications or binaries.** Where a compromised site redirects to an
  APK, we record the URL and headers and deliberately abort before any file is transferred.
- **No attempt to alter anything** on your site.

Everything we do is a read-only request for a page you already serve publicly.

## How to block or exclude us

### robots.txt

```
User-agent: CloakWatch
Disallow: /
```

**We honour this for every request we make.** This is worth stating precisely, because our
scanner deliberately sends several different user-agent strings — measuring cloaking
*requires* observing what a site serves to different visitors, and that is the entire
phenomenon under study. A `Disallow` rule naming `CloakWatch` (or `*`) suppresses the
request across **all** of those profiles, not merely the one that identifies itself. If a
path is disallowed for us, we do not fetch it under any user-agent.

We also honour `Crawl-delay` where it is longer than our own 5-second interval.

### By email

Email **cloakwatch.research@gmail.com** with the hostname and we will exclude it entirely.
No justification needed, and we will confirm once it is done. Excluded hosts are dropped
before any request is made, including liveness checks.

## If we found a problem on your site

If our scan indicates your site is serving cloaked spam, we will contact the address in
your site's `security.txt` or contact page, and CERT-In. We do **not** publish the names of
affected sites while they are still compromised — only aggregate statistics — and we hold
per-site details for at least 90 days or until the issue is resolved, whichever comes
first.

If you would like us to re-check your site after a fix, email us and we will.

## Contact

- **Email:** cloakwatch.research@gmail.com
- **Project:** CloakWatch, an academic measurement study
- **Affiliation:** Independent research project
- **Operating since:** 2026-09-30

We are happy to answer questions about the methodology or to share the data we hold about
your site.
