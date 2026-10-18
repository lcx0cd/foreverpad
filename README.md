# ForeverPad — any controller in World of Warcraft: Forever

**Live:** [GitHub Pages](https://lcx0cd.github.io/foreverpad/) · [Cloudflare Pages](https://foreverpad.pages.dev/)


**Single-file web tool** for the **WoW Forever test** (also works for retail / classic): read any generic DInput controller in Chrome, learn its buttons, generate an SDL mapping, and drop it into WoW's `WTF/gamecontrollerdb.txt`.

No driver, no install, no backend, no database.

![ForeverPad card](screenshots/wowpad-card-en.png)

## Why

On macOS, generic DInput pads (Migu, Beitong, Flydigi, no-name Chinese pads…) are plain HID devices. Apple's GameController framework ignores them, so macOS games don't see a gamepad — but **Chrome's Gamepad API does**, and **WoW bundles SDL2**, which only needs a mapping line.

ForeverPad does the last mile:

1. read the pad in Chrome (Gamepad API)
2. guide you to press every key — the target key **flashes on the diagram** (voice prompts too)
3. generate the SDL mapping
4. **Copy mapping** and paste it into the file below, or **Download gamecontrollerdb.txt** and overwrite the existing file

## Screenshots

| UI (EN) | UI (ZH) |
|---|---|
| ![en](screenshots/wowpad-en.png) | ![zh](screenshots/wowpad-zh.png) |

## Quick start

1. Connect the pad in **DInput / 2.4G mode** (not the BLE keyboard mode).
2. Open `index.html` in Chrome or Edge.
3. Click **Start**, press the keys as prompted (pause ~1s each).
4. In **2. Generate mapping**, use **Copy mapping** or **Download gamecontrollerdb.txt**.
5. Open the file below (create it if missing), paste at the end (replace the line for the same GUID if present), save.
6. **Fully restart WoW** → `/console GamePadEnable 1` → bind under **Keybindings → Gamepad**.

## Manual install paths

**Windows**
```
C:\Program Files (x86)\World of Warcraft\_retail_\WTF\gamecontrollerdb.txt
C:\Program Files (x86)\World of Warcraft\_cn_beta_\WTF\gamecontrollerdb.txt
C:\Program Files (x86)\World of Warcraft\_classic_\WTF\gamecontrollerdb.txt
```

**macOS**
```
/Applications/World of Warcraft/_retail_/WTF/gamecontrollerdb.txt
/Applications/World of Warcraft/_cn_beta_/WTF/gamecontrollerdb.txt
/Applications/World of Warcraft/_classic_/WTF/gamecontrollerdb.txt
```

The downloaded file is already named `gamecontrollerdb.txt`, so you can overwrite the original directly (browsers may add `(1)` — just rename it back before overwriting).

## Features

- Live input + **target key flashing** on Xbox/PlayStation/Switch skins
- Short voice prompts (Web Speech, local), language follows the browser (zh/en)
- Generates SDL mapping; GUID from VID/PID with zero CRC (verified on macOS)
- Community lookup by VID/PID in [SDL_GameControllerDB](https://github.com/mdqinc/SDL_GameControllerDB)
- "New species" submission (copy JSON / prefilled GitHub issue)
- AI diagnostics package (copy JSON, paste into any AI)
- Single HTML file, MIT

## How the mapping works

```
03000000618f000031f8000000000000,MIGU MGV2170,platform:Mac OS X,a:b0,b:b1,...
```

- GUID layout: bus `0300` + zero CRC + vendor LE + 0 + product LE + 0 + version + 0
- SDL on macOS matches even with zeroed CRC (tested with SDL 2.32)
- DInput sticks are canonical `a0..a3`; hats are `h0.1/2/4/8`

## Tested

- Migu MGV2170 (VID `0x8F61` / PID `0xF831`, 2.4G DInput dongle)
- macOS 26.5.2 (M4), World of Warcraft 1.60.1 (CN beta / Forever)
- ConsolePort 3.3.10 is blocked by the client's addon restrictions for now — native gamepad bindings work

## Limitations

- BLE keyboard-style mode is invisible to SDL/WoW — use DInput / 2.4G / wired
- macOS GameController framework still ignores these pads (that's why this tool exists)
- Mobile browsers vary in Gamepad API support; learning is best on desktop

## License

MIT
