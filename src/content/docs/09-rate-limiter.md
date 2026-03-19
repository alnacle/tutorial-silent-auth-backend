---
title: "Coding Exercise: Adding a Rate Limiter"
description: "Coding Exercise to add a rate limiter to prevent abuse of the platform"
step: 9
---

Attackers may spam verification requests to:

* harass users (SMS spam)
* increase your costs (SMS is paid traffic)
* degrade service for legitimate users

To mitigate this abuse, let’s add rate limiting to the /verification endpoint:

* Max 3 attempts per phone number per 10 minutes
* Attempts are counted when /verification is called (regardless of whether the request succeeds or fails)
* If the limit is exceeded, return a 429 Too Many Requests error

This rate limiter is in-memory and resets if the server restarts. In production environments, you would use Redis or another shared datastore to ensure rate limiting works across multiple backend instances.

Hints:

* Store attempts in memory (Map or similar). You can use the phone number as key to store an array of timestamps.
* On each request: 
    - read the array
    - Remove timestamp older than 10 minutes
    - If remaining count >= 3, then reject
    - Otherwise append `Date.now()` and continue

How to test it:

You can test the implementation by running 4 quick requests using cURL:

```sh
for i in {1..4}; do
  curl -X POST http://localhost:4000/verification \
    -H "Content-Type: application/json" \
    -d '{"phone":"+34600111222"}'
done
```

