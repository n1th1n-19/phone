# Testing

## Setup

- Real phone over USB (`adb devices` shows it) or an emulator with a SIM-capable image.
- Phone must be set as the **default phone app** to handle calls (Android role `ROLE_DIALER`).
- Install: `adb install -r <apk>`.

## Primary: mobile-agent-testers plugin

1. Run the `mobile-agent-testers:setup` skill once SDK and device are ready.
2. Give the agent the checklist below as its script.
3. It drives the app on the device and returns screenshots + pass/fail per item.
4. Failures get fixed, then the same run is repeated.

If the plugin can't attach to the device, use the manual flow below.

## Fallback: manual adb

```sh
adb shell am start -n org.daystack.phone/.activities.MainActivity
adb exec-out screencap -p > shot.png
adb shell input swipe 100 800 900 800 300   # swipe right on a row
adb logcat -s AndroidRuntime                  # crashes
```

## Checklist

### Rebrand
- [ ] Launcher shows "Phone" with daystack icon
- [ ] About shows daystack name, repo link, Fossify credit row
- [ ] No "Fossify" text anywhere in the UI (all tabs, settings, dialogs)
- [ ] Installs alongside stock Fossify Phone without conflict

### Navigation
- [ ] Four tabs switch; selected tint correct; re-tap scrolls to top
- [ ] Large header collapses to inline title on scroll, expands at top
- [ ] System back and predictive back behave

### Keypad
- [ ] Digits, `*`, `#`, long-press 0 → `+`
- [ ] Backspace + long-press clear
- [ ] Contact suggestion appears
- [ ] Green button places call (single SIM and dual SIM picker)

### Recents / Contacts / Favorites
- [ ] Swipe right → call starts
- [ ] Swipe left → SMS app opens with number
- [ ] Short swipe → springs back, no action
- [ ] Rows without number don't swipe
- [ ] Setting off → swipe disabled
- [ ] Edit mode delete + Clear all with confirmation
- [ ] All / Missed filter

### Calls
- [ ] Outgoing: timer, mute, speaker, keypad DTMF, end
- [ ] Incoming: accept, decline, message reply, full-screen when locked
- [ ] Call waiting / add call / merge

### Appearance
- [ ] Light and dark
- [ ] Dynamic color on: accent follows wallpaper
- [ ] Dynamic color off: iOS blue
- [ ] 200% font size: nothing clipped
- [ ] TalkBack: swipe actions available as custom actions

### Performance
- [ ] Cold start under ~1s on a mid phone
- [ ] Contacts list scrolls smoothly with 1000+ contacts
