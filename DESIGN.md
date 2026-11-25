# daystack design system

One design system for every daystack app. Phone is the first app to use it.

**Formula:** Apple iOS structure and look + Samsung One UI reachability and swipe gestures + Google Material You color and Android behavior.

Most of this lives in `commons` so every app gets it. App-specific screens are at the bottom.

---

## 1. Principles

1. **Thumb first.** Content and actions sit in the lower two-thirds. Top of a root screen is a tall header, not controls.
2. **Calm surfaces.** Grey grouped background, white (or dark grey) rounded cards, hairline separators. No shadows, no ripples.
3. **Text actions over floating buttons.** No FAB. Actions are "+", "Edit", "Done" in the title bar.
4. **Color means something.** Accent for interactive, green for call/on, red for destructive/end, blue for message.
5. **Android where it matters.** System back, predictive back, dynamic color, default-app roles all keep working.

## 2. Tokens

Defined in `commons/src/main/res/values/daystack_tokens.xml` (+ `values-night/`).

### Color (light / dark)

| Token | Light | Dark | Use |
|-------|-------|------|-----|
| `ds_bg_grouped` | `#F2F2F7` | `#000000` | Screen background behind cards |
| `ds_bg_card` | `#FFFFFF` | `#1C1C1E` | Card / row background |
| `ds_bg_card_pressed` | `#E5E5EA` | `#2C2C2E` | Row pressed highlight |
| `ds_bg_field` | `#7676801F` | `#7676803D` | Search field, keypad buttons |
| `ds_label` | `#000000` | `#FFFFFF` | Primary text |
| `ds_label_secondary` | `#3C3C4399` | `#EBEBF599` | Subtitles, timestamps |
| `ds_label_tertiary` | `#3C3C434D` | `#EBEBF54D` | Placeholders, chevrons |
| `ds_separator` | `#3C3C434A` | `#54545899` | Hairlines (0.5dp) |
| `ds_accent` | dynamic, fallback `#007AFF` | dynamic, fallback `#0A84FF` | Links, selected tab, toggles |
| `ds_green` | `#34C759` | `#30D158` | Call button, switch on, swipe-to-call |
| `ds_red` | `#FF3B30` | `#FF453A` | End call, delete, missed calls |
| `ds_blue_msg` | `#007AFF` | `#0A84FF` | Swipe-to-message |

**Accent:** when the user picks the "system" theme, `ds_accent` = Material You `primary`. Otherwise iOS blue or the user's chosen color. Green/red never change.

### Type (Inter)

| Style | Size | Weight | Line | Use |
|-------|-----:|--------|-----:|-----|
| `LargeTitle` | 34sp | Bold | 41 | Root screen header |
| `Title2` | 22sp | Bold | 28 | In-call name, section heroes |
| `Headline` | 17sp | SemiBold | 22 | Inline nav title, row title emphasis |
| `Body` | 17sp | Regular | 22 | Row title |
| `Subhead` | 15sp | Regular | 20 | Row subtitle |
| `Footnote` | 13sp | Regular | 18 | Section headers/footers, timestamps |
| `Caption` | 11sp | Medium | 13 | Tab bar labels, keypad letters |
| `KeypadDigit` | 36sp | Regular | 40 | Keypad digits |

Use `sp`, respect system font scale. Letter-spacing: Inter at large sizes −0.02em.

### Shape and spacing

- Card radius **12dp**. Alert radius **14dp**. Sheet top radius **12dp**. Search field radius **10dp**. Buttons: full pill.
- Screen side margin **16dp**. Row min height **44dp** (single line) / **60dp** (two line). Row inner padding 16dp horizontal.
- Separator inset: starts at text start (after avatar), 0.5dp.
- Section gap **24dp**, header 13sp secondary text 6dp above card.
- Touch target min **44dp**.

### Icons

- **Material Symbols Rounded, FILL = 1**, weight 400, grade 0, 24dp. Imported as vector drawables, prefix `ic_ds_`.
- Tab bar icons filled when selected, same filled glyph tinted secondary when not.
- No SF Symbols.

### Motion

- Spring: damping ratio 0.85, stiffness ~400 (`SpringForce.STIFFNESS_MEDIUM` area) for sheets, tab switches and swipe snap-back.
- Push/pop: horizontal slide 300ms.
- Press: background shifts to `ds_bg_card_pressed`, no ripple.
- Respect "remove animations" system setting.

### Material (blur)

- Tab bar and collapsed toolbar: `ds_bg_card` at 80% alpha + `RenderEffect` blur 30px on API 31+. Solid color below API 31.
- Hairline separator on the edge touching content.

## 3. Components (in `commons`)

### 3.1 Large-title header (One UI + iOS)

```
┌──────────────────────────────┐
│                     Edit  +  │  ← action row (text/icon buttons, accent)
│                              │
│                              │  ← empty space, header ≈ 35–40% of screen
│ Recents                      │  ← LargeTitle
│ 3 missed calls               │  ← optional Subhead subtitle
│ ┌──────────────────────────┐ │
│ │ 🔍 Search                │ │  ← rounded search field
│ └──────────────────────────┘ │
├──────────────────────────────┤
│  content…                    │
```

- On scroll the header collapses; the title fades into a centered `Headline` inline title in a blurred bar.
- Header expands back when the list returns to top. Snap to fully open/closed.
- Landscape and short screens: header height drops to iOS size (no One UI empty space).
- Implemented by restyling `MyAppBarLayout` + `MySearchMenu`.

### 3.2 Inset-grouped list

- Rows inside a 12dp card, 16dp side margins.
- Drawables: `ds_row_single`, `ds_row_top`, `ds_row_middle`, `ds_row_bottom` (corner radii by position).
- Optional leading avatar (40dp circle) or icon in rounded square (29dp, 7dp radius, colored).
- Trailing: chevron `›` (tertiary), `ⓘ` info button (accent), value text (secondary) or switch.

### 3.3 Bottom tab bar

- 49dp + bottom inset, blurred background, top hairline.
- Icon 24dp above `Caption` label. Selected = accent, others = `ds_label_secondary`.
- Re-tapping the selected tab scrolls to top.

### 3.4 Switch

- iOS pill: 51×31dp track, 27dp white thumb with soft shadow. On = `ds_green`, off = `ds_bg_field`.
- Restyle `MyMaterialSwitch`.

### 3.5 Alerts and sheets

- **Alert** (confirmations): 270dp wide, 14dp radius, centered title (Headline) + message (Footnote), buttons separated by hairlines. Destructive button red, default button semibold.
- **Sheet** (pickers, options, forms): bottom sheet, 12dp top radius, 36×5dp grabber, medium + large detents.
- **Action sheet** (long-press menus): stacked rounded group + separate "Cancel" card.
- Restyle `MaterialAlertDialog.App` and Fossify's dialog helpers.

### 3.6 Edit mode

- "Edit" in header → rows show red ⊖ on the leading edge; tap reveals red "Delete" on trailing edge. "Done" exits.
- Replaces Fossify's checkbox action mode visually; selection logic stays.

### 3.7 Swipe actions (One UI)

```
 ← swipe left                     swipe right →
┌─────────────────────┬──────┐  ┌──────┬─────────────────────┐
│ John Appleseed      │ 💬   │  │ 📞   │ John Appleseed      │
│ mobile              │ blue │  │green │ mobile              │
└─────────────────────┴──────┘  └──────┴─────────────────────┘
```

- Swipe **right → call**, swipe **left → message**.
- Colored background and filled icon reveal under the row. Icon scales up at the trigger threshold (40% of row width) with a haptic tick.
- Release past threshold → action fires, row springs back. Release before → springs back, nothing happens.
- Message opens the default SMS app (`ACTION_SENDTO`, `smsto:<number>`), so it works with or without daystack Messages.
- Built on `ItemTouchHelper` as a shared helper in `commons` (`DsSwipeCallHelper`) so Contacts and Messages can reuse it.
- Setting: "Swipe to call or send messages" (default on). When off, no swipe at all.
- Rows without a number don't swipe.

## 4. Phone screens

### 4.1 Structure

Bottom tabs: **Favorites · Recents · Contacts · Keypad**. App opens on the last used tab (default Recents). Replaces Fossify's top ViewPager tabs and FAB dialpad.

### 4.2 Favorites

- Header: "Favorites", actions "Edit" (left) and "+" (right).
- Rows: avatar, name (Body), number label (Subhead secondary, e.g. "mobile"), trailing ⓘ.
- Tap row = call. Swipe = call/message. Edit = reorder handles + delete.

### 4.3 Recents

- Header: "Recents", subtitle "N missed calls" when > 0. Segmented control under title: **All | Missed**.
- Rows: name or number (red if missed), call-type icon + label ("mobile", "outgoing"), trailing time ("10:42", "Yesterday", weekday, date) + ⓘ.
- Grouped repeat calls show count "(3)".
- Tap = call back. Swipe = call/message. Edit → delete rows, "Clear" all with confirmation alert.

### 4.4 Contacts

- Header: "Contacts", "+" action, search field.
- "My card" row at top (own contact, if set).
- Alphabet section headers (sticky), fast-scroll index on the right edge.
- Tap → contact detail sheet (big avatar, name, quick action row: message / call / video / mail as round tinted buttons, then grouped number/email rows).
- Swipe = call/message.

### 4.5 Keypad

```
          +1 555 0123        ⌫
     ┌───┐   ┌───┐   ┌───┐
     │ 1 │   │ 2 │   │ 3 │
     │   │   │ABC│   │DEF│
     └───┘   └───┘   └───┘
      …4 GHI  5 JKL  6 MNO
      …7 PQRS 8 TUV  9 WXYZ
      …  *     0 +     #
              ( 📞 )
```

- 3×4 grid of **78dp circles**, `ds_bg_field` fill, digit `KeypadDigit`, letters `Caption` spaced.
- Spacing 24dp horizontal, 16dp vertical, grid centered, sits low on screen.
- Press: fill lightens/darkens + DTMF tone (setting) + haptic.
- Long-press 0 = "+", long-press 1 = voicemail.
- Number field on top, auto-formatted, shrinks font as it grows; backspace appears only when non-empty (long-press clears).
- Contact match suggestion under the number ("John Appleseed — mobile").
- **Call button:** 78dp green circle, white filled phone icon. Dual-SIM: SIM picker sheet or two call buttons labeled 1/2.

### 4.6 In-call screen

- Background: blurred, darkened contact photo, or dark gradient from accent when no photo.
- Top: name (`Title2`, white), status/timer (Subhead, white 70%).
- Control grid 3×2, 72dp circles, translucent white: **mute · keypad · speaker / add call · video (hidden if unsupported) · contacts**. Active state = solid white with dark icon.
- Bottom: **end call** 72dp red circle.
- Incoming: name + "mobile", slide-free buttons: red decline (left), green accept (right), plus "Remind me" and "Message" small actions. Full-screen notification keeps Android behavior.

### 4.7 Settings

- iOS-style inset-grouped settings: General (default tab, swipe actions, dialpad tones, vibration), Appearance (theme: system/light/dark, accent: dynamic/blue/custom), Calls (block list, speed dial), About.
- About: version, link to GitHub repo, "Based on Fossify. Open source, GPL-3.0" row.

## 5. Accessibility

- All text in `sp`; layouts tolerate 200% font scale (headers wrap, rows grow).
- Contrast ≥ 4.5:1 for text against cards in both modes.
- Swipe actions also reachable via long-press menu and TalkBack custom actions ("Call", "Message").
- Content descriptions on every icon-only button.

## 6. Out of scope (for now)

- Voicemail tab (Android has no common visual voicemail API).
- Contact posters / custom in-call backgrounds.
- Widgets restyle.
