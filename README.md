# Adsjoint integration demo

A React + Vite chat example using `@adsjoint/sdk@1.1.0` in explicit test mode.
Replies are simulated locally; no OpenAI account, API key, or credits are needed.

## Install and run

Ask admin@adsjoint.ai for package access and your publisher name and ID.
Package installation requires your own GitHub personal access token **(classic)**
with `read:packages` permission and an account authorized to download the SDK.
This GitHub token is separate from publisher credentials. No separate Adsjoint API key is required.

```sh
git clone https://github.com/adsjoint/adsjoint-demo.git
cd adsjoint-demo
```

Create a local `.npmrc` (ignored by Git):

```ini
@adsjoint:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${NODE_AUTH_TOKEN}
```

Set `NODE_AUTH_TOKEN` in your local environment to your GitHub token. Do not commit
or share the token or put it in browser code. Then run:

```sh
npm ci
npm start
```

Open the local URL Vite prints. Replace the example publisher name and ID in `src/App.jsx`
with your assigned values when integrating into your own app.

## Test ads

The demo explicitly initializes the SDK with `mode: "test"`. Send either exact,
case-sensitive marker:

- `ADSJOINT_DEMO_TRAVEL`
- `ADSJOINT_DEMO_GAME`

The simulated AI reply repeats the marker and can display the corresponding demo ad.
Ordinary words such as "travel" and "game" do not trigger demo ads. Only travel and
game demo inventory is bundled. Test events are console-only, do not enter production
analytics, and never consume campaign budgets.

Frequency limits still apply: each category displays once per page lifetime, with
at least two new AI messages since the previous ad. To try the second category,
send an ordinary message first, then its marker. Reload to reset the in-memory limits.

## Integrate into your app

```jsx
import { initAdsjoint, initAdLogic, InlineAdBanner } from "@adsjoint/sdk";

// Initialize once before rendering ads.
initAdsjoint({
  publisherName: "your_app",
  publisherId: "YOUR_PUBLISHER_ID",
  mode: "test",
});
initAdLogic(true);

// Place below a completed AI response:
<InlineAdBanner message={{ sender: "ai", text: responseText }} />
```

## Production

SDK 1.1.0 defaults to `mode: "production"`; `"prod"` is also accepted.
Production never displays bundled demo ads, even when a demo marker appears.
Without a configured campaign backend or eligible campaign, no ad is displayed.

```js
initAdsjoint({
  publisherName: "your_app",
  publisherId: "YOUR_PUBLISHER_ID",
  mode: "production",
  backend: {
    url: "https://YOUR_PROJECT.supabase.co",
    anonKey: "YOUR_PUBLIC_ANON_OR_PUBLISHABLE_KEY",
  },
});
initAdLogic(true);
```

Coordinate backend setup with Adsjoint before enabling production. Never use a
Supabase service-role key in the browser. The campaign schema and setup instructions
are in [the SDK repository](https://github.com/adsjoint/sdk/blob/main/sdk/CAMPAIGNS.md)
(repository access may be required). Providing backend configuration alone does not
create campaign tables. The pilot backend needs authentication/rate limiting and
fraud protection before paid rollout.

Campaign matching happens locally. The production campaign path sends the matched
category and publisher ID for delivery, then a delivery ID and event type for
impressions/clicks; it does not send conversation text. Creative images load from
external image hosts and ad clicks open their destination websites.

Contact: admin@adsjoint.ai
