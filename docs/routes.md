# Route and redirect plan

Demo build. Domain and `/` are unchanged. Canonical and `og:url` always point at `https://www.kayrichinsure.com`, never a preview or vercel.app host.

## Routes

| Production route | File in this project | Title |
|---|---|---|
| `/` | `Home.dc.html` | Insurance Agency in Kaufman, TX \| KayRich Insurance Agency |
| `/auto-insurance/` | `Auto-Insurance.dc.html` | Auto Insurance in Kaufman, TX \| KayRich Insurance Agency |
| `/home-insurance/` | `Home-Insurance.dc.html` | Home Insurance in Kaufman, TX \| KayRich Insurance Agency |
| `/life-insurance/` | `Life-Insurance.dc.html` | Life Insurance in Kaufman, TX \| KayRich Insurance Agency |
| `/business-insurance/` | `Business-Insurance.dc.html` | Business Insurance in Kaufman, TX \| KayRich Insurance Agency |
| `/race-car-insurance/` | `Race-Car-Insurance.dc.html` | Race Car Insurance in Texas \| KayRich Insurance Agency, Kaufman TX |
| `/service-area/` | `Service-Area.dc.html` | Service Area: Kaufman County Insurance \| KayRich Insurance Agency |
| `/service-area/forney/` | `Forney.dc.html` | Insurance Agency in Forney, TX \| KayRich Insurance Agency |
| `/service-area/terrell/` | `Terrell.dc.html` | Insurance Agency in Terrell, TX \| KayRich Insurance Agency |
| `/about/` | `About.dc.html` | About Kayleigh Phillips-Richards \| KayRich Insurance Agency, Kaufman TX |
| `/reviews/` | `Reviews.dc.html` | Reviews \| KayRich Insurance Agency, Kaufman TX |
| `/contact/` | `Contact.dc.html` | Contact KayRich Insurance Agency \| Kaufman, TX |

Shared components, not routes: `Site-Nav.dc.html`, `Site-Footer.dc.html`, `Quote-Band.dc.html`.

## Redirects (301, permanent)

```
/book-online                                        -> /contact/
/service-page/auto-home-life-commercial-quote       -> /contact/
/pricing-plans/plans-pricing                        -> /
/booking-calendar/auto-home-life-commercial-quote   -> /contact/
```

The fourth is not in the approved list but is reachable from `/book-online` on the live site, so it needs a destination or it becomes a 404. Flagged for approval rather than assumed.

## Carried over from the live site

- `meta name="google-site-verification"` `_BI3ClyBLiJ6ect366NLAYOK5UVtCG6CCjPdR8-1txs` (homepage)
- `meta name="msvalidate.01"` `43A893FCD8E8503EA51AC3A30F729A7F` (homepage)
- `og:site_name` `KayRich Insurance`
- Google Maps directions link target, unchanged
- Facebook share URL, unchanged

## Dropped deliberately

- `meta name="format-detection" content="telephone=no"`. It suppressed tap-to-call on mobile, which is the site's primary conversion path.
- Five placeholder social URLs pointing at bare platform homepages.
- Two duplicate copies of the Facebook share URL.
- The `Log In` member-area entry point, which had nothing behind it.

## NAP, locked

```
KayRich Insurance Agency
107 N Jackson St
Kaufman, TX 75142
(214) 773-3938
```

Rendered identically in `Site-Footer.dc.html`, `Contact.dc.html`, `Service-Area.dc.html`, the homepage CTA band, and every `InsuranceAgency` JSON-LD block. Phone links use `tel:+12147733938` and display as `(214) 773-3938`.
