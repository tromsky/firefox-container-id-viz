# firefox-container-id-viz
A CSS userscript to fix the ugly container identification viz

Hey! Do you hate the giant horizontal container identification viz in Firefox 155+? Then this repo is for you!

This custom stylesheet replaces the ugly horizontal viz with a nice vertical one, similar to previous versions of Firefox.

Before

<img width="446" height="88" alt="image" src="https://github.com/user-attachments/assets/f6d4e63a-87e6-4b55-80d3-28c35ffd2a63" />

After

<img width="209" height="51" alt="image" src="https://github.com/user-attachments/assets/6d5303d5-69c4-4f7d-85d4-e9b39f4cfda0" />

Tested on Firefox 157 (macOS) with vertical tabs.

## 1. Enable custom stylesheets

1. In Firefox, go to `about:config`.
2. Set `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`.

## 2. Find your profile folder

1. In Firefox, go to `about:support` and find the **Profile Folder** row. Note the full path it shows, e.g.:
   `~/Library/Application Support/Firefox/Profiles/xxxxxxxx.default-release`
2. **Watch out:** In my case, on macOS, **Show in Finder** opened the parent `Profiles` folder rather than the profile directory itself. The `chrome` folder must go **inside** the `xxxxxxxx.default-release` folder, not next to it.

## 3. Create the stylesheet

Inside the profile folder, create `chrome/userChrome.css`:
- The folder name is lowercase `chrome`.
- The file name is `userChrome.css`, with an uppercase C.
- Use vim or nano to paste the contents of the CSS file in this repo (ensure your editor doesn't add an extension to the file).
- CSS found in `userChrome.css` in this repo.

To hide the line entirely instead, use only this:

```css
.tabbrowser-tab[usercontextid] > .tab-stack > .tab-background > .tab-context-line {
  display: none !important;
}
```

## 4. Restart Firefox

Quit Firefox fully (use a session-saver if you are paranoid about lost windows and tabs), `userChrome.css` is only read at startup.

## Tweaking

| What | Property | Value |
|---|---|---|
| Bar thickness | `width` | `2px` |
| Bar length (bigger = shorter) | `inset-block` | `7px` |
| Bar distance from tab edge | `inset-inline-start` | `1px` |
| Gap before favicon | `margin-inline-start` | `4px` |

## Troubleshooting

If nothing changes, Firefox probably isn't loading the file. To check:

1. In `about:config`, set `devtools.chrome.enabled` and `devtools.debugger.remote-enabled` to `true`.
2. Open the Browser Toolbox with ⌥⇧⌘I (OPTION + SHIFT + COMMAND + I, or **Tools → Browser Tools → Browser Toolbox**), then go to **Style Editor**.
3. If `userChrome.css` isn't listed, re-check the pref, the folder location, and the file name/extension.
4. If it is listed but has no effect, a Firefox update may have changed the markup. Use the **Inspector** tab and search the markup for `tab-context-line` to find the current element and its rules.

When you're done, reset the two devtools prefs with the ↺ button in `about:config`. Keep `toolkit.legacyUserProfileCustomizations.stylesheets` set to `true`.

## Reverting

To revert to stock Firefox:
- Delete the `chrome` directory and its contents from the profile directory.
- In Firefox go to `about:config` and set `toolkit.legacyUserProfileCustomizations.stylesheets` to `false`.
