# Build a daily idea card from JSON

The [example page](index.html) turns Bucketlist.nl's daily JSON response into a small card. It uses browser JavaScript, a select box and a button. The idea can be an experience or a personal goal: a place to visit, something to learn, or another activity worth considering.

Open the example in a browser, choose English or Dutch, and press **Load today's idea**. The page requests data only after that click. You do not need an API key or a Bucketlist account. The page's interface stays in English; the selected language applies to the idea.

## Follow one click

`loadDream()` disables the controls while it works and hides the previous card. It builds a URL from the public endpoint and the selected language. For English, that URL is:

```text
https://bucketlist.nl/api/droom-van-de-dag?scope=wereld&lang=en
```

The request asks for JSON. Its `credentials: 'omit'` setting avoids sending browser credentials, and `referrerPolicy: 'no-referrer'` omits the referring page. The receiving server still sees an ordinary request; these settings do not make it anonymous.

Two steps happen before the page can use the result: awaiting the HTTP response, then awaiting the decoded JSON body. An HTTP error does not necessarily reject the `fetch()` promise, so the example checks `response.ok` before reading the body. [MDN explains this distinction](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#handling_the_response).

## Read the data you actually received

The response includes a feed date, selected language and a nested `dream` object. The renderer uses these fields:

| JSON field | On the card |
| --- | --- |
| `date` | Date supplied by the feed |
| `language` | Language attribute for the idea text |
| `dream.title` | Heading |
| `dream.theme` | Topic, when present |
| `dream.destination` | Destination, when present |
| `dream.canonicalUrl` | Link to the full idea |

For example, a real English response on 11 September 2026 described getting a certificate in a field that interests you. Its theme was `Learning and craft` and its destination was `null`. A missing destination does not mean “the Netherlands” or “anywhere”: the card simply leaves that part out. The response does not supply a `description` field for this card.

`readDream()` checks the fields the example needs before returning a smaller display object. It also accepts idea links only on the HTTPS Bucketlist.nl origin and under `/ervaringen/`. That is a deliberate rule for this particular feed. If you adapt the example to another provider, review its documented link format instead of deleting the check without a replacement.

## Put text on the page

The card uses `textContent` for the title and metadata. Feed text becomes visible text, rather than being interpreted as HTML. The separately checked URL becomes the link's `href`.

The feed date remains a calendar label. It is not converted into a local midnight timestamp, which could make a visitor in another time zone see the previous day. The selected feed language is attached to the title and metadata so assistive technology can distinguish it from the English interface.

The example uses the returned canonical link. It does not add campaign parameters. Some destination pages contain labelled affiliate options; the card says so. It makes no claims about the availability, price or suitability of an activity.

## Plan for an empty or failed response

The page has three useful states: waiting, a valid card, and a readable failure message. It handles an unavailable response, malformed data and an eight-second timeout. Controls are restored in `finally`, and the timeout is cleared. There is no automatic retry loop.

The API supplies a daily selection. Clicking again can return the same idea, and normal HTTP caching may satisfy a request. A rapid polling interval would add little value to this example. Keep the provider's cache behavior and content cadence in mind when adapting it.

## Try three small changes

1. **Show the theme separately.** Move it above the title while keeping an absent theme harmless. Use the existing response rather than adding another API request.
2. **Explain a missing destination.** Use a neutral label such as “No destination supplied” if that helps your interface. Do not invent a location from the title or the chosen scope.
3. **Compare the states.** With developer tools, try an offline request and inspect how the controls recover. Then restore normal connectivity. A successful server-side API check alone does not prove that a browser page renders correctly.

The [machine-readable API contract](https://bucketlist.nl/brand/droom-van-de-dag.openapi.json) describes the wider response, five supported languages and optional theme filters. This example exposes English and Dutch, uses the world feed, and does not display image fields or grant rights to reuse media.

The source and live browser example are maintained together in this repository. The code licence applies only to the example code, not API content or linked experiences.
