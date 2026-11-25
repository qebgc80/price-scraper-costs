# Price Scraper Costs: What Does One Request Really Cost, Will It Hold Up on Amazon and Walmart, and Which Plan Do You Need? (Full Plan and Credit Table)

Two billing models dominate price scraping, and they behave very differently the moment a page fails. Apify's universal price scraper charges per successful row and publishes its misses. ScraperAPI sells credits, where an Amazon request costs five of them before you add rendering or premium proxies. Which one ends up cheaper depends less on the headline rate than on two numbers most comparison posts skip: the per-run URL cap and the credit multiplier for the sites you actually target.

Below is ScraperAPI's full plan line-up, what a product page really consumes in credits, a realistic success-rate baseline taken from a competitor's own published measurement, and the cases where a free Chrome extension beats both.

> **Quick pick:** test on the 7-day trial (5,000 credits, no card), then stay on the free plan (1,000 credits/month, 5 concurrent threads) if you're checking a few dozen SKUs. Move to Hobby (100,000 credits, 20 threads) once weekly runs pass roughly a thousand pages. Multi-country price checks need Business or above, because country-level geotargeting doesn't start until there. 👉 [Start the 7-day trial with 5,000 free credits](https://www.scraperapi.com/signup?fp_ref=coupons)

## The full plan line-up

| Plan | Credits per billing period | Concurrent threads | Geotargeting | Analytics history | Start |
| --- | --- | --- | --- | --- | --- |
| Free | 1,000 | 5 | — | — | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Hobby | 100,000 | 20 | US & EU | 30 days | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | US & EU | 30 days | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Business | 3,000,000 | 100 | Country-level | Unlimited | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Scaling | 5,000,000 | 200 | Country-level | Unlimited | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | Country-level | Unlimited | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | Country-level | Unlimited | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Enterprise | 22,000,000+ | 500+ | Country-level | Unlimited | [ Sign up](https://www.scraperapi.com/signup?fp_ref=coupons) |

Three billing behaviours matter more than the credit count:

- **Credits don't roll over.** Your balance resets when the subscription renews, so an oversized plan is money burned, not banked.
- **Auto-upgrade is opt-in on Hobby through Professional.** Leave it on and your account climbs to the next tier when credits run out. Leave it off and scraping pauses until renewal.
- **Pay-as-you-go only unlocks at Scaling and above**, where you set a monthly spending cap. On Hobby, Startup or Business, running dry means upgrading or waiting.

Annual billing takes 10% off every tier (Enterprise is custom). The published plan table lists credits, concurrency, geotargeting and analytics history rather than dollar amounts, so confirm the figure on the tier you're choosing when you sign up. The credit allowance is the part you can compare directly, and it's also the part that decides your real cost.

## What one product page actually costs in credits

A "credit" is not a request. Standard pages cost one credit, but the sites price teams care about cost more:

| Request type | Credits |
| --- | --- |
| Standard page | 1 |
| Amazon, Walmart, eBay | 5 |
| Google, Bing (incl. subdomains) | 25 |
| LinkedIn | 30 |
| Bot-protection bypass (Cloudflare, DataDome, PerimeterX/HUMAN, Akamai) | 10 |
| JS rendering (`render=true`) | 10 |
| Premium proxies (`premium=true`) | 10 |
| Screenshot | 10 |
| Premium proxies + JS rendering | 25 |
| Ultra premium (paid plans only) | 30 |

Geotargeting, sessions, device type, output format and auto parsing add nothing.

Do the arithmetic and Hobby's 100,000 credits is 100,000 ordinary pages, 20,000 Amazon product pages at the base e-commerce rate, or about 4,000 requests on the premium-proxy-plus-rendering line at 25 credits each. Same plan, a 25x spread depending on what you point it at. Anyone quoting "100K requests" without that table is quoting the easiest possible page.

ScraperAPI's docs point to a Domain Cost Estimator in the dashboard that returns the exact cost for any URL, and a `max_cost` parameter that caps a single scrape. Both are worth running on five of your real targets before you pick a tier, because a marketplace listing behind bot protection is not a blog post.

## Where price scraping breaks, and who pays for the break

The failure modes are dull and consistent. A dead product URL rarely returns an error; shops quietly redirect it to the parent category, which still has a heading and a "from" price, and a naive scraper hands you that as your product. Challenge screens return a normal HTTP 200 with a title on them. Page footers parse as titles.

Apify, which competes with ScraperAPI on exactly this job, publishes the checks it uses to reject those rows: the row has to carry a price or a product identifier, the page has to be a real page rather than an interstitial, and it has to be the product you asked for. Rejected rows land in a failure list, free of charge. Its measurement, published on the tool's page: 999 of 1,100 retail domains returned a usable product page, and across 41 live product URLs on 41 shops in 12 countries it delivered 31 rows, 30 of them with a price. On URLs that were still live products, 31 of 37 came back. Four of the ten misses were the input's fault, not the scraper's.

Treat that as the baseline for any price scraper, including this one: expect roughly one in six URLs to come back empty or rejected even on a well-maintained list, and expect the misses to cluster on dead listings and category redirects. If a vendor promises 100%, ask what happens to the row that got redirected to a category page.

Which raises the billing question that decides your monthly invoice. On per-success pricing, misses cost nothing. ScraperAPI's model charges per request from the credit table above, so a request that returns a blocked page or a wrong product still consumes credits unless you catch it downstream. Its docs describe `max_cost` per scrape and the domain estimator; if you're about to push a 50,000-URL batch through, ask support directly how failed or empty responses are treated on your plan. That one answer can move your effective cost per usable row by 15–20%.

Parsing noise adds a second layer. European shops write prices as `1.499,00` where US shops write `1,499.00`, and promotional copy ("Was $89.99", "Save 30%", "from $12.99") sits in the same node as the real price. Any pipeline you build has to normalise the number format, keep the currency, and separate the current price from the pre-discount one, otherwise your repricing feed quietly compares a strikethrough to a sale price.

ScraperAPI's own guidance for staying unblocked is the standard four: rotate IPs, put random intervals between requests, use a headless browser, and route CAPTCHAs to a solving service. Its pitch is that the rotating proxy pools, CAPTCHA and anti-bot handling, JS rendering and automatic retries are included on every plan rather than sold as add-ons, which is what the credit multipliers above are paying for.

## Picking by scenario, not by tier name

**A few dozen SKUs checked weekly.** The free plan's 1,000 credits and 5 threads cover around 200 Amazon pages a month, or 1,000 ordinary pages. For pure ad-hoc checks with no code, Thunderbit's free Chrome extension takes a pasted URL list or a .txt/.csv upload and returns product name, URL, current price and original price, exportable to Sheets, Airtable, Notion, CSV or JSON. Neither option needs a card.

**Hundreds to 10,000 URLs on a schedule.** This is where ScraperAPI's DataPipeline fits: a no-code scheduler that monitors up to 10,000 product listings, queries or URLs per project and outputs HTML, JSON or CSV. Compare that cap directly against per-run limits elsewhere; Apify's price scraper takes 100 URLs per run, so 10,000 URLs is 100 runs. Per-run capacity multiplied by run frequency is your monthly throughput, and it's the number that decides whether a cheap-looking tool needs a workaround.

**Multi-country price comparison.** Localised data from 150+ countries is available across plans, but country-level geotargeting starts at Business. If you need a German price as a German shopper and a Japanese price as a Japanese shopper, Hobby and Startup's US & EU targeting won't get you there.

**Marketplace catalogues at scale.** Four structured endpoints return parsed JSON or CSV instead of raw HTML: Amazon Search, Walmart Search, Google Shopping and eBay Products. For Amazon specifically there are two routes, building a Scrapy-based scraper and maintaining it, or calling an endpoint that hands back priced records. The structured endpoints are the reason to pay credits rather than write selectors.

**Millions of requests.** The Async Scraper takes a single `Post()` request and handles jobs at that volume, which is the tier of usage where per-run caps stop mattering and concurrency (20 to 500+ threads across the plans) becomes the constraint.

👉 [See the price-monitoring setup and the four structured endpoints](https://scraperapi.com/solutions/price-monitoring?fp_ref=coupons)

## How the alternatives compare

| Tool | Billing model | Verified detail |
| --- | --- | --- |
| ScraperAPI | Credits, per request | Amazon/Walmart/Google Shopping/eBay endpoints; 5,000 trial credits; 99.9% uptime guarantee; 150+ countries; 20–500+ threads |
| Apify price scraper | Per successful row | 100 URLs per run; up to 33 output fields; 999 of 1,100 domains measured; rejected rows not billed |
| Octoparse | Subscription | $89/month Standard, $249/month Professional, custom Enterprise (figures from a vendor-run review, not Octoparse's own page) |
| Thunderbit | Free Chrome extension | Current and original price; exports to Sheets, Airtable, Notion, CSV, JSON; scheduled runs |
| Clay | Platform subscription | Chrome extension plus the Claygent AI scraper; free plan and three paid tiers, no public per-tier pricing in the review |

The Octoparse numbers come from a review published by Clay, which ranks its own product first, so treat the prices as indicative and the ranking as marketing. Two user-report themes from that same review are worth knowing: Octoparse draws complaints about a complicated UI and slow support, and Webscraper.io about a learning curve and occasional crashes.

## FAQ

**Can a price scraper handle Amazon?** Yes, via a dedicated endpoint rather than the standard page route, at 5 credits per request. Amazon product pages, reviews and offers are all covered, and Walmart, Google Shopping and eBay have equivalents.

**What do I do when requests start getting blocked?** Rotate IPs, randomise intervals between requests, render with a headless browser, and use a CAPTCHA-solving service. On a managed API, those four are the vendor's job; the credit multipliers for bot-protection bypass (10) and JS rendering (10) are what you pay for it.

**How far do the free credits stretch?** The 7-day trial gives 5,000 credits with no card, which is 5,000 standard pages or 1,000 Amazon requests at base rate. When it ends you drop to the free plan's 1,000 credits and 5 threads per month.

**Do unused credits carry over?** No. The balance resets at renewal, and the annual plans grant the full year's credits upfront, usable on any timeline, with early renewal available if you burn through them.

**Is there a refund?** Seven days, no questions asked, arranged through support. Cancellation from the dashboard costs nothing.

**Can I look up prices by EAN code instead of URL?** Not on URL-based scrapers. Apify's current price scraper rejects bare EAN or GTIN codes outright and points to a separate EAN-to-offers tool; codes are skipped without charge. If code-based lookup is your workflow, pick a tool built for it rather than a URL scraper.

## Before you commit

The decision comes down to three lines:

- **Under ~1,000 pages a month, one country:** free plan, or a free browser extension if you don't need scheduling.
- **1,000 to 100,000 pages a month, US/EU pricing:** Hobby, and run the Domain Cost Estimator on your five hardest URLs first.
- **Country-level pricing or more than 100,000 pages:** Business and up, where geotargeting and unlimited analytics history arrive together.

Two things to confirm at signup. The first is the dollar amount on the specific tier, since the published table lists allowances and the annual option takes 10% off. The second is what happens when credits run out on your account: auto-upgrade on means a silent jump to the next tier, auto-upgrade off means scraping stops until renewal, and pay-as-you-go with a spending cap isn't available below Scaling.

👉 [Start on the free plan or Hobby and confirm the credit allowance against your own URL list](https://www.scraperapi.com/signup?fp_ref=coupons)
