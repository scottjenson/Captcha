# Captcha

A one-page experiment that shows what an "I'm not a robot" checkbox could learn from your mouse. The user clicks Start test, then clicks a checkbox dropped at a random spot, and sees their mouse path and speed plotted, blown up so the small wobbles, overshoots and corrections are visible.

## Background
Real checkbox captchas (reCAPTCHA v2, hCaptcha, Cloudflare Turnstile) decide mostly from cookies and account history, IP reputation and browser fingerprinting. Mouse behaviour is one weak signal among many, because it is easy to fake. This page isolates only the behaviour part so people can appreciate how variable human input is. It is a teaching toy: it makes no pass/fail judgement and doesn't need to be polished.

## How the page works
- **Flow:** intro → Start test → checkbox placed at random, at least `MIN_TRAVEL` px from the Start click → click → 700 ms fake spinner → results scroll in. "Try again" reloads the page.
- **Recording:** `pointermove` with `getCoalescedEvents`, in page coordinates, from the Start click until mouse up (so drift while the button is held is captured). Touch input or keyboard activation shows a "no path" message instead of charts.
- **Final reach (`segment()`):** walk back from the last movement before mouse down until a gap longer than `PAUSE_MS`. A hover resting on the box before pressing is *not* a pause. It stays in as a flat zero-speed stretch; treating it as a pause once caused frequent "not enough movement" failures. Then mouse down, any samples while held, mouse up.
- **Path chart:** fits the reach plus the checkbox with uniform scale, so it works wherever the box lands. The grid is 10 px squares, switching to 50 px when zoomed out. It draws one dot per sample and a dashed straight line from the start to mouse down (the path a naive bot would take).
- **Speed chart:** positions resampled every `SAMPLE_MS`, speed measured over a small window. Shows speed rather than acceleration, because acceleration is too noisy to read. A shaded band marks the button held down.
- **Shared numbered markers** on both charts: start, speed peaks and dips (detected over the reach only, with a threshold of 15% of top speed in `findMarks`), entering the box, mouse down, mouse up.
- Canvas drawing reads colours from CSS variables, so dark mode works. Canvases are sharp on high-resolution screens and redraw on resize.

## Decisions so far
- Results stay at the two charts plus captions. An earlier list of the numbered points and a panel of stat boxes (path length, overshoot, click offset, hover time, hold time) were removed as clutter at the user's request.
- Ideas suggested but not built: hovering the speed chart to highlight the matching spot on the path; a fake "bot" path for comparison.
- This repo was repurposed from an earlier Bluesky "Splitter" app. Git history before `captcha.html` belongs to that app.

## Working conventions
- Keep the app a single file (`captcha.html`) with no build step and no libraries; if one is ever needed, load it from a pinned CDN version.
- Discuss and get approval before changing direction or architecture; small fixes the user asked for can go straight ahead.
- The repo is public: keep personal content out of committed files.
- After changing the script, at least run a syntax check: extract the `<script>` body (`sed -n '/<script>/,/<\/script>/p' captcha.html | sed '1d;$d' > /tmp/c.js`) and run `node --check` on it. Mouse behaviour and the look of the charts can only be checked by the user in a browser.

## How to report back

I skim your replies. Write for someone who wants to act, not to be taught.

- Lead with the outcome in one or two sentences: what changed, and whether it works.
- Then only what needs my attention: decisions I should make, risks, things you
  couldn't verify, anything surprising or different from what I asked.
- Don't narrate your steps, restate my request, or explain code I can read in the diff.
- Don't list every file touched unless I ask.
- Default to under ~150 words. Go longer only when the content earns it:
  a real trade-off, a bug with a non-obvious cause, or a design choice I need to weigh in on.
- If there's more worth knowing, end with one line offering it ("Want the details on X?")
  instead of including it.
