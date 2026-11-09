# How KICK LA BANANE was designed

This is a short write‑up of the shape of the game, what each part is trying to do, and how the code is laid out. It's meant for anyone curious, or anyone who wants to tinker.

## The idea

A tiny, joyful runner a kid and a parent can both enjoy:

- **Instantly readable:** one big yellow hero, three lanes and a handful of colour rules. You learn them by kicking things.
- **Loud and bright:** a Commodore 64 look (raster bars, a `READY.` prompt, a chunky 5×7 bitmap font) pushed to very saturated yellows. *Everything* in the interface bops to the beat.
- **Forgiving:** there's no game over. Losing all your hearts sends you back to the start of the level (or of the zone, in the last level) with 1 second of invincibility. Only that level's points are lost, so the cost is real but small.
- **Shareable:** one HTML file that works on any device. Double‑click it, drop it on a website, or add it to a phone's home screen.

## Shape of the game

```
TITLE ──► LEVEL (countdown → run → clear) ──► MERCHANT ──► next LEVEL … ──► VICTORY
  │                                              ▲
  └─► JUKEBOX                                    └─ BUY / SWITCH skins with banana points
```

- **Camera and world:** a pseudo‑3D corridor seen from behind the banana, in the spirit of Temple Run. The world is projected with a simple pinhole model (`proj(x, y, z)`); the road is drawn as strips from far to near, then objects and the banana, sorted by depth.
- **Three lanes:** you change lane, jump (gravity is about 6× weaker on the Moon), kick, or slam down from the air. The slam also counts as a kick.
- **Kicking** is the main verb. The coloured blocks are a small vocabulary of rules:
  - **Red:** never kick it.
  - **Yellow:** a wall.
  - **Green:** explodes.
  - **Pink:** pushes you into another lane.
  - **Orange:** blinds you for a moment.
  - **Purple:** makes you fall and roll.
  
  Chests hold the power‑ups. Every animal and obstacle can be kicked away for points; holes have to be jumped.
- **Scoring budget:** each level is generated randomly, then its points are scaled so that a *perfect* run earns that level's target. The targets climb from 500 points (Jungle) to 1,500 (last level). That way skin prices stay balanced however the random layout falls: a perfect level buys about one skin.
- **Speed curve:** every level ramps from its own starting speed to a faster one. Rows of obstacles are spaced in *seconds of reaction time*, not distance, so higher speed never makes a level impossible. The Moon gets extra spacing for its huge jumps.

## The levels and their intentions

Each world adds one new idea on top of the basics, and has its own scenery, its own music style and its own low side scenery.

| World | New idea | Feel |
|---|---|---|
| **Jungle** | The basics: holes to jump, monkeys charging at you, fallen trees | Warm, friendly, cumbia |
| **Savannah** | **Moving targets**: gazelles zig‑zag, crocodiles wander across lanes, lions charge | Golden hour, samba drums |
| **City** | **Things you can't jump**: taxis are too tall, so dodge them or kick them | Busy streets, a cheering crowd, salsa brass |
| **Sewers** | **Low visibility**: a flashlight cone inside a dark tunnel | Spooky and funny (censored piles), tango |
| **Arctic** | **Losing control**: every landing on ice makes you twirl for 1 s, and lane changes slide | Cool, bright, bossa nova |
| **Moon** | **Verticality and threats from behind**: 6× jumps, satellites at jump height, buggies that come up behind you with a warning | Wide open, a crowd of astronauts, reggaeton |
| **Last level** | **Endurance**: all six worlds in order, about twice as long, with breathers (+1 heart, conga break) and checkpoints at each zone | The grand tour |

Between levels, the **merchant** gives the game a calm, silly beat to rest on. He's a cyber‑goth from "far from here", dancing a fluid glow‑stick routine in the Mont Saint‑Sauveur parking lot at sunset. The background crowd's costumes change with the level you just beat: banana skiers, lion skiers, snowboarders in suits, rats, back‑flipping polar bears and astronauts.

## Music

There are eight original songs, each in a different Latin style, plus jingles, all synthesized live with Web Audio:

- **Engine:** comes from the earlier game [BlocSauteur](https://github.com/u269c/BlocSauteur) and keeps its song format (`{ bpm, loop, layers }`). It was extended with:
  - a scheduler that queues notes about a second ahead;
  - Latin percussion voices: clave, cowbell, congas, güiro, surdo, agogô, samba whistle, and so on;
  - an echo effect, slightly detuned doubled leads and stereo panning.
- **Writing music:** songs are written in a small grid notation so bars are easy to count. `g8('C5 . E5 - | …')` means one token per 8th note, `.` is a rest and `-` holds the previous note.
- **Dance clock:** a beat counter follows the music, so the interface, the merchant and the crowds all dance in time with whatever is playing.

## Screens, devices and languages

- **Responsive canvas:** the drawing area takes the screen's shape. In landscape it's 512×384, extended sideways on wide screens. In portrait it's 480 pixels wide with a taller layout and a slightly higher camera, so obstacles stay visible over the banana's head. The HUD stays clear of notches and home bars.
- **Menus:** they have true portrait layouts. In landscape, the original 4:3 layouts sit centred and their backgrounds extend to the edges.
- **Languages:** French (the default), Japanese, Italian and English. Text is translated at the moment it's drawn, so layout code doesn't care which language is active. Japanese uses a pixel web font, sharpened to match the bitmap font.

## Code map

Everything lives in [`kick-la-banane.html`](../kick-la-banane.html). Search for these section banners:

| Section | What's there |
|---|---|
| *(top of the script)* | 5×7 bitmap font, pixel helpers, sprite builder |
| `MUSIC` | Grid notation, chord book, every song |
| `AUDIO` | The BlocSauteur‑derived engine, percussion voices, sound effects |
| `LEVELS, THINGS & SKINS` | Level definitions, obstacle definitions, skin palettes |
| `THE BANANA` | The palette‑swappable pixel banana and its poses |
| `GAME STATE` / `UPDATE` | Level generation, kicking, collisions, power‑ups, restarts, last‑level zones |
| `WORLD RENDERING` / `WORLDS` | Corridor projection, objects, backdrops and per‑world side scenery |
| `MERCHANT SCENE` / `SCENES` | Mont Saint‑Sauveur, the crowd, the merchant and his dance, title, shop, jukebox, clear and victory screens |
| `INPUT` / `MAIN LOOP` | Keyboard, swipes and taps; the frame loop |
| `RESPONSIVE` | Canvas sizing, safe areas, HUD, overlays, touch buttons, portrait layouts |
| `LANGUAGES` | Translation tables and the text engine (bitmap Latin plus pixel Japanese) |

A small debugging hook is exposed as `window.KLB` (game state, `startLevel`, the audio engine and so on) for poking around in the browser console.
