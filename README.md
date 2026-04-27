<div align="center">

<img src="https://orteil.dashnet.org/cookieclicker/img/favicon.ico" width="80" height="80" alt="Cookie Clicker Icon"/>

# 🍪 Cookie Clicker — Mod Collection

> *A handcrafted collection of JavaScript mods that refine and improve the Cookie Clicker experience.*

[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Cookie Clicker](https://img.shields.io/badge/Cookie%20Clicker-Compatible-c0392b?style=flat-square)](https://orteil.dashnet.org/cookieclicker/)
[![Mods](https://img.shields.io/badge/Mods-2-8e44ad?style=flat-square)](#-mods)
[![License](https://img.shields.io/badge/License-MIT-2ecc71?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-27ae60?style=flat-square)](CONTRIBUTING.md)

---

**[Browse Mods](#-mods) • [Installation](#-installation) • [Compatibility](#-compatibility) • [Contributing](#-contributing)**

</div>

---

## 📖 Table of Contents

- [About](#-about)
- [Mods](#-mods)
  - [No Lucky Cookie](#1--no-lucky-cookie)
  - [Grimoire Mana Lock](#2--grimoire-mana-lock)
- [Installation](#-installation)
  - [Method 1: Browser Console](#method-1-browser-console-quickest)
  - [Method 2: Userscript Manager](#method-2-userscript-manager-recommended)
  - [Method 3: In-Game Mod Menu](#method-3-in-game-mod-menu)
- [How It Works](#-how-it-works)
- [Compatibility](#-compatibility)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🍪 About

This repository contains a collection of quality-of-life JavaScript mods for **[Cookie Clicker](https://orteil.dashnet.org/cookieclicker/)** — the iconic idle game by Orteil. Each mod is designed to be:

- 🪶 **Lightweight** — No bloat, no dependencies, no frameworks.
- 🔧 **Non-destructive** — Mods patch game behaviour without corrupting save files.
- 🧩 **Modular** — Each mod is self-contained and can be used independently.
- 🔍 **Transparent** — Every line of code is readable and documented.

Whether you're a casual clicker or a hardcore optimizer, these mods help you shape the game to fit your own playstyle.

---

## 🧩 Mods

### 1 — 🚫 No Lucky Cookie

> *Tired of wasting a golden cookie on a small luck bonus? This mod eliminates the Lucky outcome entirely from the golden cookie pool.*

| Property | Details |
|---|---|
| **File** | `no-lucky-cookie.js` |
| **Category** | Golden Cookie / RNG |
| **Impact** | Modifies golden cookie effect pool |
| **Reversible** | ✅ Yes — reload the game to restore default behaviour |

#### What it does

In vanilla Cookie Clicker, clicking a **Golden Cookie** can trigger one of several effects — including the `Lucky` effect, which gives you a modest flat cookie bonus based on your current bank. For optimized runs, the `Lucky` reward is often the weakest outcome, especially when you're waiting for a **Frenzy + Click Frenzy** combo chain.

This mod **removes `Lucky`** from the list of possible golden cookie effects so it will never be drawn when you click a shiny cookie.

#### Before & After

| Before | After |
|--------|-------|
| Lucky, Frenzy, Click Frenzy, Elder Frenzy, etc. | Frenzy, Click Frenzy, Elder Frenzy, etc. |
| Random — Lucky can appear anytime | Lucky is fully excluded from the pool |

#### Code Preview

```js
// Removes the "Lucky" effect from the golden cookie effect pool
const originalShimmer = Game.shimmer.prototype.pop;
Game.shimmer.prototype.pop = function () {
  const luckyIndex = Game.goldenCookieChoices.indexOf('Lucky');
  if (luckyIndex !== -1) {
    Game.goldenCookieChoices.splice(luckyIndex, 1);
  }
  return originalShimmer.apply(this, arguments);
};
```

---

### 2 — 🔮 Grimoire Mana Lock

> *Freezes the mana cost of Grimoire spells so they never increase, letting you cast freely without the spiraling price penalty.*

| Property | Details |
|---|---|
| **File** | `grimoire-mana-lock.js` |
| **Category** | Grimoire / Magic |
| **Impact** | Overrides mana cost scaling logic |
| **Reversible** | ✅ Yes — reload the game to restore default behaviour |

#### What it does

The **Grimoire** mini-game in Cookie Clicker assigns a mana cost to each spell. By default, casting spells increases the cost of subsequent casts for a cooldown period — a mechanic meant to throttle spell spam.

This mod **prevents the mana price from increasing** after each cast, keeping spells permanently affordable and allowing you to cast as many times as your current mana pool allows without facing escalating costs.

#### Before & After

| Before | After |
|--------|-------|
| Each cast raises the cost of the next spell | Mana cost stays constant no matter how many casts |
| Requires waiting for prices to reset | Cast freely as long as you have mana |

#### Code Preview

```js
// Prevents mana cost escalation in the Grimoire mini-game
const originalGetCost = Game.Objects.Wizard_tower.minigame.getSpellCost;
Game.Objects.Wizard_tower.minigame.getSpellCost = function (spell) {
  const cost = originalGetCost.call(this, spell);
  // Return base cost only, ignoring the inflation multiplier
  return spell.costMin;
};
```

---

## 🚀 Installation

> **Prerequisites:** You need access to Cookie Clicker running in a browser (Steam version also supported via the developer console).

### Method 1: Browser Console *(Quickest)*

1. Open **Cookie Clicker** in your browser.
2. Press `F12` (or `Ctrl+Shift+I` / `Cmd+Option+I` on Mac) to open the DevTools.
3. Click the **Console** tab.
4. Copy the contents of the desired `.js` mod file from this repository.
5. Paste it into the console and press **Enter**.

> ⚠️ The mod will be active for the current session only. It will need to be re-applied after a page refresh.

---

### Method 2: Userscript Manager *(Recommended)*

For a persistent experience, use a browser extension like **[Tampermonkey](https://www.tampermonkey.net/)** or **[Violentmonkey](https://violentmonkey.github.io/)**.

1. Install **Tampermonkey** from your browser's extension store.
2. Click the Tampermonkey icon → **Create a new script**.
3. Replace the default content with the contents of your chosen mod file, adding the following header at the top:

```js
// ==UserScript==
// @name         Cookie Clicker — No Lucky Cookie
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Removes Lucky from the golden cookie effect pool
// @match        https://orteil.dashnet.org/cookieclicker/*
// @grant        none
// @run-at       document-idle
// ==/UserScript==
```

4. Save the script (`Ctrl+S`).
5. Reload Cookie Clicker — the mod will apply automatically every time you visit the game.

---

### Method 3: In-Game Mod Menu

> Requires Cookie Clicker **v2.031+**

1. From the main screen, go to **Options → Mods → Manage mods → Open mod folder**.
2. Create a new folder for your mod (e.g. `no-lucky-cookie/`).
3. Inside, create an `info.txt` file following the [Cookie Clicker mod format](https://github.com/nikFrans/cookieclicker-mods).
4. Place your mod `.js` file inside the same folder.
5. Enable the mod from the mod menu and reload.

---

## ⚙️ How It Works

Cookie Clicker exposes a rich global `Game` object in the browser's JavaScript environment. All game state, mechanics, and logic is accessible through this object, making it straightforward to extend or override behaviour without touching the game's source files.

Both mods follow the same pattern:

```
1. Wait for the game to fully load
2. Locate the specific function or array responsible for the behaviour
3. Patch it using a closure that wraps the original function
4. Preserve the original so behaviour can be restored on reload
```

This **monkey-patching** approach ensures the mods are safe, reversible, and compatible with future game updates as long as the underlying API doesn't change drastically.

---

## 🔄 Compatibility

| Cookie Clicker Version | No Lucky Cookie | Grimoire Mana Lock |
|---|---|---|
| v2.054 (Latest) | ✅ Compatible | ✅ Compatible |
| v2.048 | ✅ Compatible | ✅ Compatible |
| v2.031 | ✅ Compatible | ✅ Compatible |
| Steam Edition | ✅ Compatible | ✅ Compatible |
| v1.x (Legacy) | ❌ Not supported | ❌ Not supported |

> Mods are tested against the **web version** at [orteil.dashnet.org/cookieclicker](https://orteil.dashnet.org/cookieclicker/). The Steam version should work identically via the in-game console.

---

## ❓ FAQ

<details>
<summary><strong>Will these mods corrupt my save file?</strong></summary>

No. Both mods only modify runtime behaviour in memory. They do not touch save data, cookies, or achievements. Your save file is completely safe.

</details>

<details>
<summary><strong>Can I use both mods at the same time?</strong></summary>

Yes! The mods are fully independent and do not conflict with each other. Load them both via the console or set them both up as userscripts.

</details>

<details>
<summary><strong>Do the mods work with other Cookie Clicker mod collections like Cookie Monster?</strong></summary>

Generally yes, since these mods operate on different parts of the game. However, if another mod also patches the golden cookie logic or the Grimoire cost function, load order may matter. Always load these mods *after* other mods for best results.

</details>

<details>
<summary><strong>Why remove Lucky? Isn't it part of the game?</strong></summary>

Absolutely — and this mod is entirely optional! Lucky has its uses, especially early game. But for players optimizing for big combo chains (Frenzy → Click Frenzy), Lucky can feel like a wasted golden cookie. This mod simply gives you the choice.

</details>

<details>
<summary><strong>Does the Grimoire Mana Lock mod make the game too easy?</strong></summary>

That's the point! 😄 This is a quality-of-life mod for players who find the mana cost escalation frustrating rather than fun. Use it if you want — ignore it if you don't.

</details>

---

## 🤝 Contributing

Contributions are very welcome! If you've written a useful Cookie Clicker mod and want to add it to this collection:

1. **Fork** this repository.
2. Create a new branch: `git checkout -b mod/your-mod-name`
3. Add your mod file with a clear, descriptive name (e.g. `no-wrinklers.js`).
4. Update this `README.md` with a new section for your mod following the existing format.
5. Open a **Pull Request** with a description of what the mod does and why it's useful.

### Guidelines

- ✅ Keep each mod in a single self-contained `.js` file.
- ✅ Add comments explaining what each section of code does.
- ✅ Preserve original game functions using closures so mods are reversible.
- ✅ Test against the latest version of Cookie Clicker before submitting.
- ❌ Do not include external dependencies or network requests.
- ❌ Do not modify or read save data directly.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

Cookie Clicker is created and owned by **Orteil / Dashnet**. This repository is an unofficial fan project and is not affiliated with or endorsed by the Cookie Clicker developers.

---

<div align="center">

Made with ☕ and way too many cookies

⭐ **If these mods helped you, consider leaving a star!** ⭐

</div>
