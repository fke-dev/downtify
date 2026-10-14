<h1 align="center">
  <img width="96" src="assets/brand/ravevault-mark.svg" alt="RaveVault logo">
  <br>
  RaveVault
</h1>

<p align="center">
  <strong>Your self-hosted music vault for ravers &amp; DJs.<br>
  Paste a link, get a perfectly tagged track — straight into your own crate.</strong>
</p>

<div align="center">

[![License: AGPL-3.0](https://img.shields.io/badge/license-AGPL--3.0-e11d2e)](/LICENSE)
[![Based on Downtify](https://img.shields.io/badge/based%20on-Downtify-0b0c0e)](https://github.com/henriquesebastiao/downtify)
[![Made by fke-dev](https://img.shields.io/badge/made%20by-fke--dev-e11d2e)](https://github.com/fke-dev)

</div>

<p align="center">
  <img src="assets/brand/ravevault-preview.png" alt="RaveVault logo, colours and UI elements" width="80%">
</p>

## What is RaveVault?

RaveVault is a self-hosted web app that turns Spotify, YouTube Music and Deezer links into a clean, fully tagged audio library — and plays it back right in your browser. No Spotify API keys, no account, no Premium subscription.

It's built for people who live in the booth: grab that track you heard in a set, rip a whole hard-techno playlist, keep your crates in sync, and download everything as one ZIP when it's time to load up a USB stick.

```
Spotify / YouTube Music link  →  audio match  →  yt-dlp + ffmpeg + mutagen
        (metadata)                (best source)       (download & tag)
```

1. **Metadata** comes from the public Spotify embed pages — no Spotify credentials needed.
2. **Audio match**: the best YouTube Music result is picked by comparing durations, with standard YouTube as a fallback.
3. **Download & tag**: `yt-dlp` fetches the audio, `ffmpeg` converts it, `mutagen` writes title, artist, album, year and cover art.

## Features

| | |
|---|---|
| **Download** | Tracks, albums, playlists and artists from Spotify, YouTube Music and Deezer; free-text search; CSV imports (Soundiiz, TuneMyMusic, Exportify). MP3, FLAC, M4A, OGG or OPUS at the bitrate you choose. |
| **Tags & lyrics** | Title, artist, album, year and cover art in every file, plus plain and time-synced lyrics (LRCLIB, NetEase). |
| **Playlist Monitor** | Watch playlists and artists — new tracks and releases download automatically on your schedule. |
| **Built-in player** | Full-screen player with synced lyrics, editable queue, sleep timer, media keys and a ten-band EQ. |
| **Library** | Albums, artists, playlists and tracks in grid or list view, multi-select, custom playlists, liked songs, *Upgrade library* and *Replace audio*. |
| **ZIP export** | Download any selection, album or playlist as one ZIP — with the folder structure, or **every track loose** (Settings → *ZIP without folders*). Perfect for loading a USB stick. |
| **Discover, Charts & Similar** | New artists suggested from your own library, Deezer charts and similar tracks from YouTube Music radio. |
| **Podcasts** | Subscribe by RSS, link or name; new episodes download on their own. |
| **Users** | Accounts for the whole crew; everyone sees their own tracks, likes and playlists, with an activity log for admins. |
| **Integrations** | Optional Soulseek (slskd) source; Navidrome, Jellyfin and Plex via M3U playlists. |
| **Interface** | Dark & light theme in RaveVault red, installable as an app (PWA), eight languages. |

### What you can paste in

| Input | Example |
|-------|---------|
| Spotify track, album, playlist or artist | `open.spotify.com/track/…` |
| YouTube Music playlist or artist | `music.youtube.com/playlist?list=…` |
| YouTube video | `youtube.com/watch?v=…` |
| Deezer track, album, playlist or artist | `deezer.com/track/…` |
| Free-text search | `artist + track name` |
| Library export (CSV) | Soundiiz, TuneMyMusic, Exportify |

## Run it yourself

RaveVault runs as a single Python service with a Vue frontend. You need **Python 3.11–3.14**, **Node.js** and **ffmpeg**.

```bash
git clone https://github.com/fke-dev/ravevault.git && cd ravevault
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
(cd frontend && npm ci && npm run build)
WEB_GUI_LOCATION=frontend/dist .venv/bin/python main.py web --host 0.0.0.0
```

Open [http://localhost:8000](http://localhost:8000), sign in as **`admin`** / **`downtify`** and change that password right away in **Settings → General**.

Prefer Docker? Build the image from this repo:

```bash
docker build -t ravevault .
docker run -d -p 8000:8000 --name ravevault \
  -v /path/to/music:/downloads \
  -v ravevault_data:/data \
  ravevault
```

`/downloads` holds your audio files, `/data` holds the database and settings — keep both persistent.

> [!NOTE]
> Some internal names — the `DOWNTIFY_*` environment variables, the default password, cookie and storage keys — still carry the original name on purpose, so existing installs and settings keep working.

## Troubleshooting

Most download problems share one cause: YouTube wants a signed-in session. Upload a `cookies.txt` in **Settings → YouTube cookies** and they usually go away. Still rate-limited? Raise *Delay between downloads*, lower *Parallel downloads*, or set `DOWNTIFY_FORCE_IPV4=1`.

## Working on the code

We work with branches and pull requests:

1. Create a branch: `git checkout -b feature/my-idea`
2. Make your changes and run the checks:
   ```bash
   .venv/bin/python -m pytest          # backend
   (cd frontend && npx vitest run)     # frontend
   npx prettier --check frontend/src   # formatting
   ```
3. Push and open a pull request to `main`. After the merge it goes live on the server.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the coding conventions inherited from upstream.

---

> [!WARNING]
> You are responsible for what you download and for any legal consequences. RaveVault does not support unauthorized downloading of copyrighted material. Support the artists you love — buy their music.

## Credits

RaveVault is a modified version of **[Downtify](https://github.com/henriquesebastiao/downtify)** by **Henrique Sebastião** and its contributors. The heavy lifting — download pipeline, player, library and much more — is their work. RaveVault adds its own branding, the red RaveVault theme, the *ZIP without folders* option and other tweaks by [fke-dev](https://github.com/fke-dev) (Jaimy Lorenzo & Mick Labree).

Also standing on the shoulders of [yt-dlp](https://github.com/yt-dlp/yt-dlp), [ytmusicapi](https://ytmusicapi.readthedocs.io/), [mutagen](https://mutagen.readthedocs.io/), [ffmpeg](https://ffmpeg.org/) and [LRCLIB](https://lrclib.net/). Icons by [Font Awesome](https://fontawesome.com/) (CC BY 4.0), loaded via [Iconify](https://iconify.design/).

## License

Licensed under the **GNU Affero General Public License v3.0** — see [LICENSE](LICENSE).
Copyright © Henrique Sebastião and the Downtify contributors; modifications © fke-dev.

Because of the AGPL, the complete source code of RaveVault — including our changes — stays available in this repository for everyone who uses it.
