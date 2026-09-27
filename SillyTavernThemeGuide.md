# Customizing F.A.Y.E. OS

So you want F.A.Y.E. in a different outfit. Good news: you don't need to wait for me. Bad news: you're going to be staring at hex codes for a bit. It's fine. You'll survive.

This guide covers recolouring the theme, editing the profile cards and loading screen, and recolouring the regex cards to match.

> **This guide is for the SillyTavern themes only** (the `.json` files in `sillytavern-themes/`). The Lumiverse `.lumitheme` files are built differently, so none of this applies to them.

**Don't want to touch any CSS?** Skip to [The Lazy Route](#the-lazy-route-let-an-ai-do-it) at the bottom.

---

## Before You Start

**Pick a base.** Start from the theme closest to what you want:

| If you want... | Start from |
|---|---|
| A light theme | F.A.Y.E. OS ☆ |
| A dark theme | F.A.Y.E. OS ☾ |

**Pick your editing method.**

- **Inside SillyTavern:** User Settings → UI Theme → select the base theme → scroll down to the **Custom CSS** box. Edit, then hit the **Save as** button next to the theme dropdown and give it a new name so you don't overwrite the original.
- **In a text editor (recommended):** open the theme `.json` in VS Code, Notepad++, or similar. Find & Replace makes recolouring *way* faster. Change the `"name"` field at the top, save, then import it through User Settings → UI Theme → Import.

> **Heads up if you're editing the `.json` directly:** all the CSS lives inside one string, so backslashes are doubled. `\A` in the Custom CSS box shows up as `\\A` in the file. Don't "fix" it.

**Grab a contrast checker.** [WebAIM's contrast checker](https://webaim.org/resources/contrastchecker/) is free and takes two hex codes. You'll want it later.

---

## Part 1: Recolouring the Theme

Recolouring happens in three places. Do all three or things will look half-dressed.

### Step 1: The Palette (`:root`)

Near the top of the CSS is a block that looks like this:

```css
:root {
  --bunny-pink:        #9896bb;
  --bunny-pink-dark:   #5d6da5;
  --bunny-rose:        #344979;
  ...
}
```

Yes, they're all called "bunny" and yes, most of them aren't pink. Legacy naming. Don't worry about it.

Here's what each one actually controls:

| Variable | What it paints | Light theme tip | Dark theme tip |
|---|---|---|---|
| `--bunny-pink` | Main panels: buttons, character name bar, section headers, slider knobs | Your main accent, mid-tone | Your main accent, mid-tone |
| `--bunny-pink-dark` | Secondary accent: user name bar, button hover, checked checkboxes, scrollbar | Darker than `--bunny-pink` | Darker than `--bunny-pink` |
| `--bunny-rose` | The darkest accent: outlines, borders, window frames, lorebook badge, progress bar | Very dark | Very dark |
| `--bunny-blush` | App background, code blocks, reasoning box | Very pale | Near-black |
| `--bunny-cream` | Message boxes, text inputs, dropdowns | Near-white | Dark, but a little lighter than `--bunny-blush` |
| `--bunny-muted` | Toggle sliders, a few hover states | Usually same as `--bunny-pink` | Usually same as `--bunny-pink` |
| `--bunny-text` | Main text everywhere | Dark | Light |
| `--bunny-text-light` | Italics, icons, secondary text | A bit lighter than `--bunny-text` | A bit darker than `--bunny-text` |
| `--bunny-border` | Most borders and outlines | Mid-tone | Mid-tone |
| `--bunny-win-title` | Window title text | White usually works | White usually works |
| `--bunny-win-bg` | Popups, drawers, side panels | Pale, a little darker than `--bunny-blush` | Dark, a little lighter than `--bunny-blush` |

**Easiest approach:** pick 3 colours (a pale one, a mid one, a dark one), then fill in the table with lighter/darker versions of those.

### Step 2: The Hardcoded Colours

This is the annoying part. Some colours (mostly the gradient title bars) aren't using the variables, so changing `:root` alone won't catch them. You need to Find & Replace these **everywhere below the `:root` block.**

**If you started from ☆ (light):**

| Find | What it is | Replace with |
|---|---|---|
| `#5d6da5` | Gradient ends on title bars, h1 box shadow, h3 text | Your `--bunny-pink-dark` |
| `#9896bb` | Gradient middle on title bars, dashed heading lines | Your `--bunny-pink` (or something between your two accents) |
| `#344979` | User title bar gradient, h1 border, heading text, popup close button | Your `--bunny-rose` |
| `#e2e2f0` | Code block, blockquote, and h1 backgrounds | Your `--bunny-blush` |
| `#c6c6e8` | User message box background | Your `--bunny-win-bg` |
| `#1e2a47` | Popup close button border | Something darker than your `--bunny-rose` |
| `#ffffff` | Title bar text | Leave it, unless your title bars are pale |

**If you started from ☾ (dark):** same idea, plus these extras:

| Find | What it is | Replace with |
|---|---|---|
| `#111626` | User message box, code blocks, h1 and h4 backgrounds, reasoning box | Something between your `--bunny-blush` and `--bunny-cream` |
| `#c8caee` | Heading text | Your `--bunny-text` |
| `#b0aed8` | Heading lines, h3 text | Your `--bunny-text-light` |
| `#16193a` | Drawer panel background | Your `--bunny-win-bg` |
| `#0a0f20` | Popup close button border | Something near-black |

**There are also a handful of `rgba(...)` values** for see-through stuff like the chat background tint, the lined-paper lines in message boxes, the dot pattern, and shadows. Search for `rgba(` and swap the first three numbers for your colour's RGB (keep the last number, that's the transparency). Most colour pickers show RGB right next to the hex.

> **Order matters!** If one hex code is used for two different things and you only want to change one of them, replace the longer, more specific string first (like `background:#9896bb`) *before* doing a blanket replace of `#9896bb`. Otherwise the blanket replace eats both and you'll be very confused.

### Step 3: The Theme Colour Pickers

SillyTavern has its own colour settings under User Settings → **Theme Colors** (Main Text, Italics Text, Quote Text, UI Background, and so on). These live outside the Custom CSS, and a few things (like quoted dialogue in chat) still use them.

Set them to match your palette:

| Picker | Set it to |
|---|---|
| Main Text | Your `--bunny-text` |
| Italics Text | Your `--bunny-text-light` |
| Underlined Text | One of your accents |
| Quote Text | Your `--bunny-text-light` (or an accent, if it's readable) |
| UI Background / Chat Background | Your `--bunny-blush`, fairly transparent |
| UI Border | Your `--bunny-border` |

Then Save.

### Step 4: The Contrast Patch

At the very bottom of every F.A.Y.E. OS theme is a block that starts with:

```css
/* ===== CONTRAST PATCH ...
```

This forces certain text and icons to be readable on that specific theme's colours. After you recolour, **its values may be wrong for your new palette** (especially if you switched from light to dark or vice versa).

Easiest fix: **delete the whole block**, then check the stuff in the troubleshooting table below. Only add back what actually disappeared.

### Step 5: Check Your Contrast

Throw your text and background pairs into the contrast checker.

- **Body text on message boxes:** aim for **4.5 or higher.**
- **Headers, icons, title bar text:** aim for at least **3.**

The classic trap is a **mid-tone accent** (not really light, not really dark). Neither white nor dark text reads well on it. If that happens, nudge `--bunny-pink` lighter or darker until one of them clearly wins.

### Troubleshooting: "I can't see the..."

Add any of these at the bottom of your CSS and swap in a colour that's readable on that background.

| Invisible thing | Paste this |
|---|---|
| Timestamp, `...`, or pencil icon on the character name bar | `.mes .ch_name .timestamp, .mes .ch_name [class*="fa-"], .mes .ch_name div[class*="fa-"], .mes .ch_name::after { color: #COLOUR !important; }` |
| Same, but on the user name bar | `.mes[is_user="true"] .ch_name .timestamp, .mes[is_user="true"] .ch_name [class*="fa-"], .mes[is_user="true"] .ch_name i[class*="fa-"], .mes[is_user="true"] .ch_name::after { color: #COLOUR !important; }` |
| Character or user name | `.mes .ch_name .name_text { color: #COLOUR !important; }` |
| Button text or icons | `.menu_button, button, .btn { color: #COLOUR !important; } .menu_button i, button i { color: inherit !important; }` |
| Extension or settings section headers | `.inline-drawer-toggle.inline-drawer-header, .inline-drawer-toggle.inline-drawer-header::after { color: #COLOUR !important; }` |
| profile.exe / user.exe title text | `.mes .mesAvatarWrapper::before { color: #COLOUR !important; }` |
| Top nav labels (AI, SYS, WLD...) | `.drawer-icon::before { color: #COLOUR !important; }` |
| Popup X button | `.popup_close, .ui-dialog-titlebar-close { color: #COLOUR !important; }` |
| Lorebook count badge | `div.drawer-toggle.drawer-header::after { color: #COLOUR !important; }` |

Still stuck? Right-click the invisible thing → **Inspect**, and the dev tools will tell you exactly what it's called.

---

## Part 2: Profile Cards

The little `profile.exe` / `user.exe` windows above each message are pure CSS, which means you can rewrite them however you want.

> **Note:** CSS can't tell characters apart, so every character shares the same card text. Same goes for users. The `#40` and `86.3s` in the corner are SillyTavern's message ID and generation time, not part of the card text.

### Window Titles

Search for `mesAvatarWrapper::before`. You'll find two:

```css
.mes .mesAvatarWrapper::before {
  content: " ★ profile.exe" !important;
  ...
}
.mes[is_user="true"] .mesAvatarWrapper::before {
  content: "☆ user.exe" !important;
  ...
}
```

The first one is the character's card, the second is yours. Change whatever's between the quotes. `waifu.exe`, `victim.exe`, `crashout.exe`, go wild.

### Card Text

Search for `mesAvatarWrapper::after`:

```css
.mes .mesAvatarWrapper::after {
  content: "> loading char.bmp\A> status: online \25CF\A> mood: chill (>w<)\A> hp: \2665 \2665 \2665 \2665 \2665\A> ready to chat \2665" !important;
  ...
}
```

It looks cursed, but it's just a few codes:

| Code | What it does |
|---|---|
| `\A` | New line |
| `\25CF` | ● |
| `\2665` | ♥ (filled heart) |
| `\2661` | ♡ (empty heart) |
| `\2605` | ★ |
| `\2606` | ☆ |

Kaomoji and most symbols can just be typed or pasted in directly, like `(˘ω˘)` or `✧`.

So if you wanted:

```
> loading char.bmp
> status: feral ●
> mood: plotting (¬‿¬)
> hp: ♥ ♥ ♡ ♡ ♡
> ready to cause problems
```

You'd write:

```css
content: "> loading char.bmp\A> status: feral \25CF\A> mood: plotting (¬‿¬)\A> hp: \2665 \2665 \2661 \2661 \2661\A> ready to cause problems" !important;
```

A few rules so it doesn't break:

- Keep the whole thing inside **one pair of double quotes.**
- If you want a literal `"` inside the text, write it as `\"`.
- Keep `!important;` on the end.
- Keep it to around five lines. The card has a fixed height, so extra lines get cut off.

**Colours:** the card text uses `--bunny-text`, the card background uses `--bunny-cream` (character) and `--bunny-blush` (user), and the title bar uses the gradients from Step 2.

### The Heading Window

If you use `# Heading 1` in chat, it becomes a little window too. Search for `.mes_text h1::before` and change the `content: '☆'` to whatever symbol you want in its title bar.

---

## Part 3: The Loading Screen

Search for `#load-spinner::before`:

```css
#load-spinner::before {
  content: "☆ System                       _  [ ]  X\A\ALoading F.A.Y.E. O.S..." !important;
  ...
}
```

- **The spaces are real.** This one uses `white-space: pre`, so every space is kept. The pile of spaces between `System` and `_ [ ] X` is what pushes the window buttons to the right edge. If you make the title longer, delete some spaces. If you make it shorter, add some. Eyeball it until it looks right.
- `\A\A` is a blank line, then the message.
- Change `Loading F.A.Y.E. O.S...` to whatever you want: `Waking up F.A.Y.E...`, `Consulting the lore...`, `Please hold...`, etc.

**Colours:**

| Part | Controlled by |
|---|---|
| Window background | `--bunny-cream` |
| Window border | `--bunny-rose` |
| Drop shadow | `--bunny-pink-dark` |
| Title bar | The gradient in `#load-spinner::before`: `linear-gradient(to bottom, #5d6da5 0px, #9896bb 26px, ...)`. The first two hex codes are the top and bottom of the title bar. |
| Progress bar blocks | `--bunny-rose` |
| Progress bar background | `--bunny-blush` |
| Text | `--bunny-text` |

**Speed:** find `animation: bunny-load 2.5s` in `#load-spinner::after` and change `2.5s`. Lower is faster.

### Other Flavour Text You Can Change

While you're in there:

| Search for | What it is |
|---|---|
| `> waiting for user input...` | The placeholder text in the chat input box |
| `> SYSTEM CALIBRATING...` | The label on the reasoning/thinking dropdown |
| `_ [] X` and `_ [ ] X` | The fake window buttons on title bars |
| `content: "AI"`, `"SYS"`, `"WLD"`... | The top navigation labels |

---

## Part 4: Recolouring the Regex Cards

The Status Report, Commentary, and Transmigration cards come from the regex files, not the theme, so they need recolouring separately.

1. Open the regex `.json` closest to your new theme in a text editor.
2. Find & Replace the colours using the table below. It's for the ☆ set, but the other sets follow the same layout.
3. Change the symbol at the start of each `"scriptName"` so you can tell your version apart from the originals.
4. Import it through Extensions → Regex → Import, and **disable the old set** so they don't both fire.

**☆ regex colours:**

| Find | What it is |
|---|---|
| `background:#6b63b5;color:#eceaf8;` | Card header bar + its text. **Replace this whole string first!** (see below) |
| `#6b63b5` | Card border, labels (`> LOC:`, `> MENT:`...), TX subtitle strip, Faye's avatar border |
| `#eceaf8` | Status Report card body |
| `#f0eefb` | Commentary and Transmigration card body |
| `#e0ddf5` | Faye's speech bubble |
| `#2a2560` | Card text, plus the dark top band on Transmigration cards |
| `#c8c4f0` | Text on that dark Transmigration band |
| `#b8b2e0` | Divider lines |
| `rgba(107,99,181,.12)` | Tinted stats box behind PHYS/MENT |
| `color:#fff` | Transmigration subtitle strip text |

> `#eceaf8` is used for **both** the header text and the card body. That's why you replace the full `background:#6b63b5;color:#eceaf8;` string first. Then a blanket replace of `#eceaf8` only hits the card body.

Every piece of text in the cards has its colour set directly, so the theme can't override it. The one exception is **markdown the AI writes inside a card** (like `*italics*` or `"quotes"`), which will use your theme's italics and quote colours. Another reason to set up those Theme Colors from Step 3.

---

## The Lazy Route: Let an AI Do It

No shame. Any decent chat AI (Claude, ChatGPT, Gemini, etc.) can recolour the theme for you. You'll still have to test it, but you won't have to hunt down hex codes.

### Option A: Get a Find & Replace List (recommended)

The theme file is long, and AIs sometimes cut long files off halfway or quietly "tidy up" things they shouldn't touch. It's safer to have the AI tell you *what* to change, and do the replacing yourself in a text editor.

1. Open the theme `.json` you want to base yours on, copy **everything**, and paste it into the chat along with this prompt:

```
This is a SillyTavern UI theme. I want to recolour it to [DESCRIBE YOUR COLOURS, e.g. "lavender and silver, dark mode" or "sage green and cream, light mode"].

Please give me a find-and-replace table covering:
1. Every --bunny-* variable in the :root block, with the new value and what it controls.
2. Every hardcoded hex colour outside the :root block.
3. Every rgba() colour outside the :root block.
4. The json fields main_text_color, italics_text_color, underline_text_color, quote_text_color, blur_tint_color, chat_tint_color, user_mes_blur_tint_color, bot_mes_blur_tint_color, shadow_color and border_color.

Rules:
- If the same hex is used for more than one thing, tell me which longer, more specific string to replace first.
- Check contrast: body text on message boxes should be at least 4.5:1, and headers, icons and title bar text on their backgrounds at least 3:1. Tell me the ratios.
- Tell me if anything in the CONTRAST PATCH block at the bottom needs changing for the new palette.
- Don't change anything except colours.
```

2. Do the replacements in your text editor (VS Code, Notepad++, etc.), change the `"name"` field, and import it.

### Option B: Get the Whole File Back

Faster, but riskier. Same idea, but ask for the full file:

```
This is a SillyTavern UI theme. Recolour it to [DESCRIBE YOUR COLOURS].
Only change colour values — don't touch anything else, don't remove or reorder rules, and don't shorten the CSS.
Change the "name" field to "[YOUR THEME NAME]".
Return the complete JSON file in one code block.
```

**Before you import it, check that:**

- The file ends properly with a `}`. If it looks cut off, the AI ran out of room, so use Option A instead.
- It's valid JSON. Paste it into [JSONLint](https://jsonlint.com/) and it'll tell you if something's broken.

### Want Custom Card Text or a Loading Screen Message Too?

Add this to either prompt:

```
Also change the profile card text (.mesAvatarWrapper::after), window titles (.mesAvatarWrapper::before) and the loading screen text (#load-spinner::before) to fit a [VIBE, e.g. "sleepy witch", "corporate hacker"] theme.
Keep the same format: use \A for new lines, keep it to 5 lines per card, and keep the loading screen spacing so the _ [ ] X buttons stay on the right.
```

### Regexes Too

Paste the regex `.json` with:

```
These are SillyTavern regex scripts that render HTML cards. Recolour them to match this palette: [PASTE YOUR NEW HEX CODES].
Only change colour values. Keep every other part of findRegex and replaceString exactly the same.
Change the symbol at the start of each scriptName to [YOUR SYMBOL].
Return the complete JSON.
```

Whichever route you take, **test it in SillyTavern before sharing it.** AIs are confident, not always correct. Check the name bars, the buttons, the extension headers, and a Status Report card, since those are where things like to go invisible.

---

## Sharing Your Recolour

Made something cute? You're welcome to share it! Just:

- Change the theme `"name"` so it doesn't clash with the official ones.
- Give credit back to the repo.

Want it added to the repo? Open a pull request or DM me on Discord!
