# Clawd Customizer

A browser-based skin editor for **Clawd**, the Claude Code mascot.
Customize the body color of your Clawd and apply it directly to the Claude Code binary.

🔗 **[Try it live](https://K-Jude.github.io/clawd-customizer)**

---

## Features

- **Pixel editor** — Click individual pixels on the Clawd preview to color them
- **Fill All mode** — Apply a single color to the entire body at once
- **Before / After preview** — Compare the original and your custom design side by side
- **Apply to Claude Code** — Generate and download a PowerShell patch script that writes your chosen color directly into `claude.exe`
- **Reset** — Revert all changes back to the original colors

---

## How to Apply to Claude Code (Windows)

> Only the **Fill All** body color is applied to the binary.
> Per-pixel colors are browser preview only.

1. Select **Fill All** mode and choose your desired body color
2. Click **↓ patch_clawd.ps1** to download the patch script
3. Close Claude Code completely
4. Open PowerShell and run:
   ```powershell
   Set-ExecutionPolicy -Scope Process Bypass; & "C:\Users\<you>\Downloads\patch_clawd.ps1"
   ```
5. Restart Claude Code

A backup of the original `claude.exe` is automatically created as `claude.exe.original` on first run.

### Revert to original

```powershell
Copy-Item "$env:USERPROFILE\.local\bin\claude.exe.original" "$env:USERPROFILE\.local\bin\claude.exe"
```

---

## Limitations

- Only the **body color** can be fully customized (any RGB value)
- The **background/eye color** is limited to RGB values 0–9 per channel (near-black only), due to binary length constraints
- Per-pixel colors cannot be patched into the binary — the Claude Code renderer uses a single body color

---

## Technical Notes

The patch script locates the `clawd_body:"rgb(...)"` color definition inside the compiled `claude.exe` binary and overwrites it with the new color using an exact same-length byte replacement, preserving binary integrity.

---

## License

MIT
