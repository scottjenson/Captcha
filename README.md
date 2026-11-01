# Captcha

A single web page that shows how your mouse actually moves when you click an "I'm not a robot" checkbox.

Click **Start test**, then click the checkbox, which appears at a random spot on the screen. The page then plots what your hand did:

- **Your approach to the checkbox:** the path your cursor took, zoomed in so the small curves, wobbles and overshoots are visible. A dashed straight line shows the path a naive bot would take.
- **Speed over time:** how fast the cursor moved. People typically speed up, peak, then slow down to land on the target, often with a small correction at the end. A shaded band shows how long you held the button down.

Numbered points match the same moments on both charts: the start, each speed-up and slow-down, entering the box, mouse down and mouse up. Click **Try again** to start over; each run comes out a little different.

## Running it

Open `captcha.html` in any modern browser. There is nothing to install and no build step. It needs a mouse or trackpad; touch screens have no cursor path to record.

Nothing is sent anywhere: all recording and plotting happens in your browser.

## What this is (and isn't)

Real checkbox captchas such as reCAPTCHA, hCaptcha and Cloudflare Turnstile decide mostly from other signals: your cookies and account history, your IP address's reputation, and details of your browser. Mouse movement is only one weak signal among many, because it is fairly easy to fake. This page isolates that one signal so you can see how variable human input is. It doesn't judge whether you're human.
