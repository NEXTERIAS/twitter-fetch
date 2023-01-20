# twitter-api-fetch

[![codecov](https://codecov.io/gh/NEXTERIAS/twitter-api-fetch/branch/main/graph/badge.svg?token=9A7VFTMH3R)](https://codecov.io/gh/NEXTERIAS/twitter-api-fetch)
[![Tests](https://github.com/NEXTERIAS/twitter-api-fetch/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/NEXTERIAS/twitter-api-fetch/actions/workflows/tests.yml)

**twitter-api-fetch** provides fetch-like access to the Twitter API.

## Usage

### Deno

```mjs
import { oauth1a } from "https://deno.land/x/twitter_api_fetch@2.0.0/mod.ts";

const fetcher = await oauth1a({
  consumerKey: "Your consumer key here.",
  secretConsumerKey: "Your consumer key (secret) here.",
  accessToken: "Your access token here.",
  secretAccessToken: "Your access token (secret) here.",
});

const response = await fetcher("/2/tweets", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    text: "大いなる力には、大いなる責任が伴う",
  }),
});

console.log(response);
console.log(await response.json());
```

### Node.js

```sh
npm i twitter-api-fetch

# Yarn
yarn add twitter-api-fetch

# pnpm
pnpm i twitter-api-fetch
```

```mjs
import { oauth1a } from "twitter-api-fetch";

const fetcher = await oauth1a({
  consumerKey: "Your consumer key here.",
  secretConsumerKey: "Your consumer key (secret) here.",
  accessToken: "Your access token here.",
  secretAccessToken: "Your access token (secret) here.",
});

const response = await fetcher("/2/tweets", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    text: "大いなる力には、大いなる責任が伴う",
  }),
});

console.log(response);
console.log(await response.json());
```

### Cloudflare Workers

```ts
import { oauth1a } from "twitter-api-fetch";

export interface Env {
  TWITTER_API_ACCESS_TOKEN: string;
  TWITTER_API_CONSUMER_KEY: string;
  TWITTER_API_SECRET_ACCESS_TOKEN: string;
  TWITTER_API_SECRET_CONSUMER_KEY: string;
}

export default {
  async fetch(request, env, context) {
    const fetcher = oauth1a({
      consumerKey: env.TWITTER_API_CONSUMER_KEY,
      secretConsumerKey: env.TWITTER_API_SECRET_CONSUMER_KEY,
      accessToken: env.TWITTER_API_ACCESS_TOKEN,
      secretAccessToken: env.TWITTER_API_SECRET_ACCESS_TOKEN,
    });
    const response = await fetch(
      "https://api.twitter.com/2/users/910317474951786496/tweets",
    );

    if (!response.ok) {
      return new Response("Internal Server Error", { status: 500 });
    }

    const tweets = await response.json();

    console.log(tweets);

    return new Response(JSON.stringify(tweets));
  },
};
```

## Authentication

- [x] OAuth 1.0a User Context
- [ ] OAuth 2.0 Authorization Code Flow with PKCE
- [ ] App only
- [ ] Basic authentication

## License

```txt
MIT License

Copyright (c) 2022 The NEXTERIAS

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
