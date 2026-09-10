# Aura Pulse Fix

Clone this open source public GitHub repo and build project here fully working.

Make sure add all api keys secure never exposed them



https://github.com/kadalesuresh711-lab/aura-pulse-killer.git





# Fix the published second-run stall



## Confirmed cause

Production logs show Agnes returning Cloudflare rate-limit error `1015`. The app then trusts an excessive provider retry delay, so one prompt request remained open for about 15 minutes while heartbeat messages made the page appear to be working. Concurrent per-panel text checks add many Agnes calls during rendering and worsen the limit.



## Changes

- Limit the shared Agnes client to one active text request and cap every retry delay to a few seconds; queued work will stop immediately after Insta Kill.

- Remove redundant Agnes calls from the image-render path. Prompt writing already receives each exact timestamp and script line; rendering will no longer re-check and re-review every panel with the same rate-limited text key.

- Make prompt-stream cleanup unconditional so failed, rejected, and aborted streams cannot remain tracked.

- Bind browser updates to the run that created them, preventing an old run from changing progress or completion after a new run starts.

- Treat Insta Kill as cancellation across the whole render batch instead of converting it into ordinary failed panels.

- Keep timestamp parsing and prompt-to-line mapping unchanged.



## Verification

- Run focused type checks and a local browser test for kill/cancel and consecutive runs.

- Publish the fix, run a small script twice on the published website, and inspect fresh browser and server logs for no long retry wait, no stale completion, and no overlapping Agnes calls.



## Technical details

- Clamp `Retry-After`; error 1015 must use bounded backoff rather than provider-supplied multi-minute waits.

- Use one active Agnes call per server process and cancellation-aware waiting.

- Re-throw `KilledError` from batched panel work.





Pixazo api key 1

03178ba869a446eba82bce98a79fefc3



Pixazo api key 2

048e52aee2094e24bad1b46a0fb15753



Pixazo api key 3



d004a01679f843e7ba090fa1d88c926d



Pixazo api key 4

9379183b074f4655adc0fa351dd4fa29



Anges ai api key use best and free model only:-(use already selected model)

sk-I04D4YBECov6kYvbrk2JRno1VY2xyGgxWeJNb7pOPZ43q5fG

This project was built with [Lovable](https://lovable.dev).

**Live app**: https://pixazo-spark.lovable.app

## Build with Lovable

Continue developing this project in the [Lovable editor](https://lovable.dev/projects/9cdf0412-b72e-4dee-b8c1-d630d068c7e0).

- **Ship faster**: describe what you want to build and Lovable handles the code.
- **Stay in sync**: every change made in Lovable is committed straight to this repository.
- **Full ownership**: this code is yours. Push to `main` on GitHub and your changes sync back into Lovable, ready for your next prompt.

## Development

Prefer working locally? You need Node.js and npm — [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating).

```sh
git clone <this-repository-url>
cd <repository-name>
npm i
npm run dev
```
