# Jellyfin Media Server Guide

A practical, opinionated setup guide for building a reliable Jellyfin 12.x media server without turning it into a plugin experiment.

> Last reviewed: September 2026

## What this guide covers

- Installing Jellyfin on Windows, Debian/Ubuntu, Docker, NAS-style Linux hosts
- Organizing movies and TV shows so metadata matches cleanly
- Direct Play vs transcoding and hardware acceleration
- Intel Quick Sync, NVIDIA NVENC, AMD VA-API/AMF
- Recommended plugins and what to avoid installing blindly
- Official and community clients for phones, TVs and desktops
- Safe remote access with VPN or reverse proxy
- Backups, upgrades and rollback planning
- Common playback, subtitle, permission and networking problems

## Recommended setup philosophy

1. **Prefer Direct Play.** Transcoding is a compatibility fallback, not the goal.
2. **Use hardware transcoding when available.** Intel Quick Sync is especially practical for low-power home servers.
3. **Keep the plugin list small.** Install a plugin because you need its feature, not because a screenshot looks good.
4. **Do not expose port 8096 directly to the public internet unless you understand the risks.** Prefer a VPN or HTTPS reverse proxy.
5. **Back up before major Jellyfin upgrades.** Database migrations are not designed for casual downgrades.
6. **Name your media correctly first.** Good folder names solve more metadata problems than extra scrapers do.

## Start here

| Goal | Guide |
| --- | --- |
| Install the server | [INSTALLATION.md](INSTALLATION.md) |
| Organize media | [MEDIA-ORGANIZATION.md](MEDIA-ORGANIZATION.md) |
| Configure transcoding | [HARDWARE-TRANSCODING.md](HARDWARE-TRANSCODING.md) |
| Pick useful plugins | [PLUGINS.md](PLUGINS.md) |
| Choose clients | [CLIENTS.md](CLIENTS.md) |
| Access Jellyfin remotely | [REMOTE-ACCESS.md](REMOTE-ACCESS.md) |
| Back up and upgrade safely | [BACKUPS-AND-UPGRADES.md](BACKUPS-AND-UPGRADES.md) |
| Fix common problems | [TROUBLESHOOTING.md](TROUBLESHOOTING.md) |
| Check source material | [SOURCES.md](SOURCES.md) |

## A sensible starter stack

For most home users:

- **Server:** Jellyfin 12.x
- **Storage:** media on HDD/NAS, Jellyfin config/database and transcode cache on SSD
- **Networking:** wired Gigabit Ethernet where practical
- **Hardware acceleration:** Intel QSV / NVIDIA NVENC / AMD VA-API or AMF depending on platform
- **Plugins:** OpenSubtitles + Intro Skipper only if you actually want them; add Playback Reporting later if useful
- **TV client:** official Jellyfin for Android TV first; test Moonfin or Wholphin if you want a different UI/player
- **Phone/tablet:** official app, Streamyfin, Moonfin, or another maintained client that fits your platform
- **Requests:** Seerr, if you share the server with family/friends and want a request workflow
- **Remote access:** WireGuard/Tailscale-style VPN for the simplest private setup, or Caddy/reverse proxy with HTTPS for normal public-domain access

## Important Jellyfin 12 note

Jellyfin 12 is a major-version transition. Third-party plugins may lag behind the server release even when the server itself is stable. Before upgrading an existing installation:

- create a Jellyfin backup;
- verify important plugins support Jellyfin 12;
- update or temporarily disable incompatible plugins;
- keep the backup until the new version has been stable for you.

## Legal note

Jellyfin is a media server. This guide assumes you are serving media you are authorized to store and access. No third-party plugin or client listed here changes that responsibility.

## Contributing

Corrections are welcome, especially when Jellyfin 12.x changes plugin compatibility or client behavior. Please prefer official Jellyfin documentation and upstream project documentation over copied setup snippets from old forum posts.
