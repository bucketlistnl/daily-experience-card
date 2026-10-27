# Daily experience card

A small browser example that turns a public JSON response into an accessible idea card. It demonstrates one request after a button click, response validation, safe text rendering, language attributes, an optional destination, and recoverable error states.

Open `index.html` in a modern browser, or serve this folder with a local static server. Choose English or Dutch, then press **Load today’s idea**. Opening the page alone makes no API request. No build step, package installation, account, API key, browser storage or analytics is required.

The [walkthrough](TUTORIAL.md) follows a request from the button to the rendered card and includes three small exercises. The interface is English; the selected language applies to the returned idea. The world feed supplies inspiration, rather than a promise that an activity is available in every country. A repeated request can return the same idea.

The data source is [Bucketlist.nl](https://bucketlist.nl/). Its [OpenAPI contract](https://bucketlist.nl/brand/droom-van-de-dag.openapi.json) documents the response. The request uses `scope=wereld` and either `lang=en` or `lang=nl`. It honours normal browser caching, omits credentials and referrers, times out after eight seconds, and never retries automatically. The receiving server can still keep ordinary request logs.

The example renders feed values as text and accepts experience URLs only on the documented HTTPS origin and `/ervaringen/` path. It preserves the supplied calendar date, leaves an absent destination empty, and handles HTTP errors, invalid data and network failures. It displays no images. Availability, prices and suitability are outside the feed; linked experience pages can contain labelled affiliate options.

This is an original browser integration maintained by Bucketlist.nl, adapted from its previously unpublished HTML example. The separate command-line example is a different implementation. This project is not a booking service or an independent endorsement of the data provider.

The MIT licence covers this repository’s example code and walkthrough. It grants no licence to API content, images, third-party sites or linked experiences.
