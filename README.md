# Overleague

**Cosmetic, champion-themed screen effects for League of Legends (Windows desktop app).**

When you cast an ability or your ultimate, Overleague plays a short themed animation along the **edges and corners** of your own screen. It is decoration only: it never covers the middle of the screen, never shows information about other players, and never touches the game.

> Overleague isn't endorsed by Riot Games and doesn't reflect the views or opinions of Riot Games or anyone officially involved in producing or managing Riot Games properties. Riot Games, and all associated properties are trademarks or registered trademarks of Riot Games, Inc.

---

## At a glance

| | |
|---|---|
| Platform | Windows 10 / 11, single `.exe` |
| Game | League of Legends (Summoner's Rift and other modes that expose the Live Client Data API) |
| Price | Free (closed testing with a small group) |
| Data source | Riot **Live Client Data API** on the local machine, plus plain screen captures of the player's own ability bar |
| Game memory / files / input | **Not accessed, not modified, not sent** |
| Information about enemies | **None located on screen, none added to the screen** |
| Network | Only GitHub, to check for updates and the tester allow-list. No player data is uploaded. |

## What it looks like

The pictures below are rendered by the app's own preview (no game running). The dark area stands in for the game view.

**Idle** – small corner ornaments, and for some champions a gauge of the player's *own* resource.

![Idle state](docs/effect_idle.png)

**While an ability is active** – a timer for the player's own ability and an edge effect.

![Yone – Soul Unbound](docs/effect_yone.png)

![Zac – Elastic Slingshot](docs/effect_zac.png)

**Ultimate**

![Azir – Emperor's Divide](docs/effect_azir.png)

**The desktop window** – choose which champions' effects are on, and preview them without a game. (The UI is currently in Korean.)

![App window](docs/app_window.png)

## Supported champions

Aatrox, Aphelios, Azir, Master Yi, Mordekaiser, Pantheon, Warwick, Yasuo, Yone, Zac — each with variants that follow the player's current skin. Other champions are listed but locked until their effects are made.

## How it works

1. **Detecting a game.** The app polls `https://127.0.0.1:2999/liveclientdata/` (Riot's official local API). If the active player is on a supported champion, that champion's overlay is created.
2. **Knowing when to play an effect.** In order of preference:
   - **Live Client Data API values** for the active player: champion, skin ID, ability levels, resource value (mana / energy / champion-specific bar), health, movement speed, and the event list (kills and assists that involve the player).
     Example: Azir's abilities are recognised by the mana they cost right after the player presses the key.
   - **The player's own ability bar**, when the API does not expose the needed state (for example, whether an ability icon has switched to its "recast" picture). The app takes an ordinary Windows screen capture (GDI `BitBlt`) of a small rectangle around the player's own ability icons at the bottom of the screen and compares it with the public ability icons.
   - **Key state** of the player's own ability keys (`GetAsyncKeyState`), only to time the animation.
3. **Drawing.** A transparent, click-through, always-on-top window owned by Overleague draws the animation. Mouse and keyboard input pass straight through to the game.

## What it does not do

- No reading or writing of game memory, no code injection, no hooks into the game process, no changes to game files.
- No input is sent to the game; nothing is automated.
- No detection or tracking of enemy champions on screen. No enemy positions, health, cooldowns, summoner spells, wards, or jungle/objective timers are shown.
- No gameplay advice, recommendations, or statistics.
- Nothing is hidden from the game or from anti-cheat: the app is a normal, visible Windows process.
- No player data leaves the PC.

### Complete list of what can appear on screen

- Corner ornaments themed to the player's champion and skin.
- Edge and corner animations when the player casts an ability or ultimate, gets a takedown, or dies.
- Timers and gauges for the **player's own** state: remaining time of the player's own ability, the player's own resource bar, stacks of the player's own passive.
- A red blink of the ornaments while the player's own champion is crowd-controlled (read from the player's own status area).
- Mordekaiser: after the player kills the champion taken into the ultimate, that champion's portrait is shown as a "soul collected" mark (from the kill event in the Live Client Data API).
- Master Yi, on some skins only: a row of the enemy team's champion portraits during the ultimate, greyed out when that champion has died. This uses only the player list and kill events from the Live Client Data API (the same information as the in-game scoreboard); nothing is read from the screen for it.

## Network access

| Destination | Purpose |
|---|---|
| `127.0.0.1:2999` | Riot Live Client Data API (local) |
| `api.github.com`, `raw.githubusercontent.com` | Check for a new version of the app; read the tester allow-list |
| `raw.communitydragon.org`, `ddragon.leagueoflegends.com` | Download public ability icons / champion portraits if they are not already bundled |

## Trying it (for reviewers)

1. Download [`Overleague.exe`](https://github.com/freeiam1125-glitch/champion-overlay/raw/release/Overleague.exe) and run it. It installs itself to `%LOCALAPPDATA%\Overleague` and keeps itself up to date from the `release` branch of this repository.
2. During closed testing the app only turns on for allow-listed testers. Open **로그인 (Login)** at the top right:
   - **라이엇 아이디로 로그인** – enter a Riot ID that has been allow-listed, or
   - **PC 키로 로그인하기** – shows a code for this PC that we can allow-list.

   Reviewers: please open an [issue](https://github.com/freeiam1125-glitch/champion-overlay/issues) (or reply through the Developer Portal) with your Riot ID or the PC code and it will be enabled right away.
3. Select a champion and press **미리보기 (Preview)** to see its effects without starting a game, or start a game (Practice Tool works) on a supported champion.

## Contact

Please use this repository's [issues](https://github.com/freeiam1125-glitch/champion-overlay/issues).
