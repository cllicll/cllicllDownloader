# 🎬 cllicllDownloader

**YouTube MP3 & MP4 downloader — best‑quality output, smart fallbacks, macOS‑first experience.**

`cllicllDownloader` is a lightweight Python CLI that grabs YouTube videos in **the highest possible MP4** and/or **MP3 at 320 kbps**.
It uses **smart fallback chains** (separate streams → format 22 → format 18) to stay reliable even when formats are restricted.

> Works great on **all modern Macs** (Intel & Apple Silicon), uses your home folders by default, and can optionally read **browser cookies** to access age‑restricted / account‑limited videos.

---

## ✨ Features

- 🎧 **MP3 at 320 kbps** — best audio + optional embedded thumbnail cover
- 🎥 **MP4 merge** — best video + best audio via FFmpeg, prefers MP4
- 🔄 **Smart fallback chain** — `bv*+ba` → `22 (720p)` → `18 (360p)`
- 🍪 **Cookie support** — Chrome, Brave, Edge, Vivaldi, or Safari (macOS Keychain prompt)
- 🏠 **Safe default paths** — `~/Music/cllicllDownloader/Music` (MP3), `~/Movies/cllicllDownloader/Video` (MP4)
- 🧰 Robust flags: bitrate, custom output dirs, disable thumbnail embedding, etc.
- 🧱 Clear error messages and KeyboardInterrupt handling

---

## 📦 Requirements

- macOS 10.13+ (Intel or Apple Silicon)
- Python 3.9+
- Homebrew (recommended)
- **FFmpeg** and **yt-dlp**

---

## ⚙️ Installation (3 steps)

```bash
# 1) Install dependencies
which brew >/dev/null 2>&1 || /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install ffmpeg yt-dlp python@3.12 pipx
pipx ensurepath

# (Apple Silicon users may also run for the current shell)
# eval "$(/opt/homebrew/bin/brew shellenv)"

# 2) Make the script executable
chmod +x clliclldownloader

# 3) Put it on your PATH
mkdir -p ~/.local/bin
mv clliclldownloader ~/.local/bin/
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

> If Gatekeeper quarantines the file, you can clear it with:
>
> ```bash
> xattr -dr com.apple.quarantine ~/.local/bin/clliclldownloader
> ```

---

## 🚀 Usage

```bash
# Best MP3 (320 kbps) → ~/Music/cllicll/Music
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID"

# Best MP4 → ~/Movies/cllicll/Video
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type video

# Both MP4 + MP3
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type both

# With browser cookies (for age‑restricted/private formats)
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --cookies

# Custom output + turn off thumbnail embedding
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type mp3 \  --music-dir ~/Desktop/Music --no-thumb
```

### Default Output Paths

| Type | Path                     |
| ---- | ------------------------ |
| MP3  | `~/Music/cllicllDownloader/Music`  |
| MP4  | `~/Movies/cllicllDownloader/Video` |

---

## 🔧 Options

| Flag          | Description                                                    |
| ------------- | -------------------------------------------------------------- |
| `--type`      | `mp3`, `video`, or `both` (default: `mp3`)                     |
| `--cookies`   | Try reading browser cookies (Chrome/Brave/Edge/Vivaldi/Safari) |
| `--bitrate`   | MP3 bitrate in kbps (default: `320`)                           |
| `--music-dir` | Output folder for MP3 files                                    |
| `--video-dir` | Output folder for MP4 files                                    |
| `--no-thumb`  | Disable embedding the thumbnail cover into MP3                 |

---

## 🔐 Cookies & Keychain

When you use `--cookies`, macOS may ask for Keychain permission so `yt-dlp` can read cookie databases securely.
Granting access allows the tool to fetch videos that require login (e.g., age‑gated content).

**Tip:** If Safari is restricted on your macOS version, Chrome/Brave generally work best.

---

## 🧰 Troubleshooting

- **FFmpeg not found**  
  Install with Homebrew: `brew install ffmpeg`

- **Blocked by Gatekeeper**  
  Clear the quarantine attribute:

  ```bash
  xattr -dr com.apple.quarantine ~/.local/bin/clliclldownloader
  ```

- **Cookie extraction failed**  
  Try a different browser (Chrome/Brave). Ensure the browser was closed before running.

- **“Unsupported URL” or format errors**  
  Ensure the URL is a standard YouTube watch URL. The tool falls back to `22` (720p) or `18` (360p) automatically if separate streams are not available.

---

## ❓ FAQ

**Q: Does it download playlists?**  
A: The tool is set to `--noplaylist`. You can wrap it with a small shell script to read a list of URLs line‑by‑line if needed.

**Q: Can I change the filename pattern?**  
A: Yes, modify `outtmpl` (e.g., `%(uploader)s - %(title)s.%(ext)s`) in the script.

**Q: Can I use it on Linux/Windows?**  
A: It will likely work with minor tweaks. This project focuses on macOS defaults and Keychain behavior.

---

## 🗺️ Roadmap

- Optional playlist mode
- Filename pattern presets
- `pipx`/Homebrew formula for one‑line install
- Progress bars and richer logging switch

---

## 🤝 Contributing

Issues and PRs are welcome! Please open an issue with clear reproduction steps or a proposal before large changes.

---

## 🔒 Security Notes

- Cookie access is local and permissioned by macOS Keychain per browser.
- Review the script before installing to understand exactly what it does.

---

## 🪪 License

MIT License © 2025 Celal İÇELLİ
