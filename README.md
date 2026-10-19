# Kink Jars

Live at [kinks.casa](https://kinks.casa). A single-page questionnaire: pick a color, rate each jar 1–5 (or tag it curious, or tape it shut as a hard limit), share a link or a short code.
Everything runs in the browser. Nothing is uploaded, and answers are only kept in that person's browser (localStorage) so a refresh doesn't wipe them. A shared link carries the sheet after the `#`, a part of the address browsers never send to the host.
The page makes no third-party requests: the fonts are served from the `fonts/` folder, not from Google Fonts.

## Share and compare

"Share my link" gives a link that holds the color and every jar. The same sheet is also shown as a code of 28 letters and digits, like `37A3-FZC3-EHER-6X2X-GDT5-V0VM-BP0R`, short enough to read out or type. The code skips letters that look like digits (I, L, O, U) and ends in a check character, so a typo is refused instead of opening someone else's jars. The page never asks for a name: your preview says "Your kink jars", an opened link says "Your jars" or "Their jars", and a comparison labels the two sides "You" and "Them". There is no image to save.

- **Opening a link** shows that sheet read-only, as a vector picture drawn on the spot, with a "Want to create your own? Click here" footer underneath.
- **Compare with someone:** press "Compare with mine" on their picture, or paste their link or type their code into the compare box. That opens a second read-only picture with its own link: each jar split in two, yours on the left and theirs on the right, a heart on every jar you both rated 4 or 5. Give/receive jars are matched across (your "spanking" sits next to their "being spanked"). The comparison link holds both sheets, so whoever opens it sees the same picture, with their own sheet on the left as "You". It is a snapshot: change your jars and you need to compare again.
- **Use them as my own jars:** under someone's picture, this copies the sheet onto the page as your own. Handy for moving your jars to another device with your own link.

Sheets from a link are never stored. Anyone who has a link or a code can read every sheet in it, and a comparison link gives away both.

## Hosting

kinks.casa is served by a Cloudflare Worker, as static files. Any static host works the same way:

- Publish `index.html` at the root with the `fonts/` folder beside it. Nothing else needs to go up.
- Links leave out `index.html` (`https://kinks.casa/#37A3…`), and the page tidies `index.html` and any `?query` out of the address bar when it opens. So the host has to serve `index.html` for `/`, which every static host does.
- Keep the host from adding anything to the page: no injected analytics or optimization scripts. On Cloudflare that means Web Analytics' automatic setup, Rocket Loader and Zaraz stay off. Anything like that would break the promise that the page makes no third-party requests.

## Customizing

- **Jar labels:** edit the `JARS` array at the top of the `<script>`. Each entry is a list of label lines. The grid is 7 columns wide and grows rows as needed.
- **Explainers:** `ABOUT` holds the one-line text behind each "What's this?" link, keyed by the jar's label. A jar without an entry simply gets no link.
- **Give/receive pairs:** `PAIRS` lists which jars are two sides of one thing, by their position in `JARS`. Keep it in step when you reorder jars.
- **Changing the set starts everyone fresh.** Answers and links are stored by position, so after adding, removing or reordering jars, bump `STORE` and `FORMAT`. Saved answers and links from the old set are then dropped instead of landing on the wrong jars. The current set has 35 jars (storage `kinkjars-v2`, link format `3`).
- **Preset colors:** edit `PRESETS`.
- **Overflow looks:** six styles (drips, puddle, foam with popped lid, geyser, splash, hearts), assigned so neighbors never match. Details are seeded per jar, so every friend's sheet is consistent.
- **Brand:** the "kinks.casa" line and its house-with-a-jar logo sit at the top of the page; the same drawing (`house` in the script) is the tab icon, with the jar filled in the page color. Each row of jars in the picture stands on a wooden shelf.
