# Eat Slow Quest — Deploy Guide

A single-page PWA. No build step, no backend.

## Files
- index.html          — landing page (what it is, why slow eating, how to play)
- play.html           — the entire game
- feedback.html       — feedback form (emails you via FormSubmit)
- manifest.webmanifest — PWA manifest (name, icons, standalone display)
- sw.js               — service worker (offline caching)
- icons/              — app icons (192, 512, apple-touch 180)

## Deploy (pick one, ~5 minutes)

### Netlify (easiest)
1. Go to https://app.netlify.com/drop
2. Drag this whole folder onto the page
3. Done — you get a URL like https://something.netlify.app

### GitHub Pages
1. Create a repo, push these files to the root of the main branch
2. Settings → Pages → Source: main branch, / (root)
3. URL: https://<user>.github.io/<repo>/

### Cloudflare Pages
1. pages.cloudflare.com → Create project → Direct upload
2. Upload this folder

## Install on your phone
- iPhone: open the URL in Safari → Share → "Add to Home Screen"
- Android: open in Chrome → menu → "Install app" / "Add to Home screen"

After the first load it works fully offline. Progress is saved per
device (localStorage); move it between devices with the in-game
Save Code buttons in "My meals". The installed app opens straight
into the game (play.html); the website root shows the landing page.

## Feedback form (one-time setup)
The form on feedback.html posts to FormSubmit (formsubmit.co), which emails each
message to the address in the form's `action`. After deploying, send one test
message: FormSubmit emails you an activation link, and nothing is delivered
until you click it. FormSubmit then gives you a random alias; put it in place of
the email address in the form's `action` to keep your address out of the page source.

## Updating the game
Replace play.html and bump the CACHE name in sw.js (e.g. v19 → v20) so
installed clients pick up the new version.

index.html is generated: edit ../landing-src/landing.tpl.html and run
`python3 landing-src/build_landing.py` from the parent folder. It pulls the
Gobblin and food sprites from play.html, so the landing page always matches the game.

## Balance notes
- Bites per meal come from typical portion ÷ forkful (see FOOD_TYPES in play.html).
- Gobblin HP = the damage you'd deal chewing every bite for ~18s (combos and crits
  included), times foeStrength(): 0.6 for the first meals, 1.0 by meal 15, 1.1 by meal 30.
