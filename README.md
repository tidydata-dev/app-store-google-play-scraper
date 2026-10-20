# App Store & Google Play Scraper — Python, Node.js & cURL examples

Get **app data from the Apple App Store and Google Play** in one table — rating, ratings count, **installs**, price, version, release/update dates, genre, **developer website and support email** — for a list of apps, **keyword searches (with rank)**, **all apps of a developer** or **Apple top charts**. Any country. **$0.50 per 1,000 apps**.

Hosted on Apify: **[App Store & Google Play Scraper](https://apify.com/tidydata/app-store-google-play-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/app-store-google-play-scraper").call(run_input={
    "searchTerms": [
        "habit tracker"
    ],
    "searchStores": [
        "appstore",
        "googleplay"
    ],
    "countries": [
        "us"
    ],
    "maxResults": 30
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['store', 'position', 'title', 'rating', 'installs', 'developerEmail']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/app-store-google-play-scraper').call({"appleCharts": ["top-grossing"], "countries": ["us"], "maxResults": 100});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~app-store-google-play-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"appleCharts": ["top-grossing"], "countries": ["us"], "maxResults": 100}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
