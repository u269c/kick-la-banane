# Contributing

Thanks for your interest in KICK LA BANANE! Contributions are **welcome**: bug reports, fixes, new skins, new songs, translations, ideas.

**Please set your expectations, though:** this is a family hobby project, maintained in spare time. Replies to issues and pull requests may be **slow or may not happen at all**, and there's no roadmap or support. If something matters to you, feel free to fork it and make it your own; the license (Apache 2.0) allows that.

## Reporting a bug

Open an issue with:
- what you expected, and what happened instead;
- your device and browser (for example, "iPhone 15, Safari, held upright");
- the level and language, if relevant;
- a screenshot or screen recording if you can, which helps a lot.

## Sending a change

1. Keep it a **single self‑contained HTML file**, with no build step, frameworks or external assets. The one exception is the optional Japanese web font, and the game must still work without it.
2. Make small, focused pull requests. One idea per PR is much easier to review.
3. Test before sending:
   - Open `kick-la-banane.html` directly from disk in at least one desktop browser.
   - Check a phone‑sized window in both orientations (your browser's device mode works).
   - Play through at least the level you touched, and check the browser console for errors.
   - If you changed any text, check it in all four languages (flag button on the title screen).
4. Keep the house style: bright, dancing, pixel‑crisp, kind. Text is uppercase. New on‑screen strings should be added to the `LANGUAGES` tables (French, Japanese, Italian; English is the key).

## Easy places to start

- **A new skin:** add an entry to `SKINS` and `SKINPAL` (in `LEVELS, THINGS & SKINS`).
- **A new song or a remix:** add a track to `MUSIC` using the grid notation described at the top of that section, then add it to `TRACKS` so it appears in the jukebox.
- **Translations:** improve the existing ones, or propose a new language by adding a `DICT` table and the patterns in `PAT`.

By contributing, you agree that your contribution is licensed under the project's [Apache License 2.0](LICENSE).
