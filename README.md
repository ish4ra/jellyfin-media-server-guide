<div align="center">

# 🎬 Jellyfin Media Server Guide

### Build a clean, fast Jellyfin 12.x server that is easy to maintain.

Practical guidance for **installation, media naming, hardware transcoding, clients, plugins, remote access, backups, and troubleshooting** — with a strong bias toward Direct Play and simple setups.

![Jellyfin](https://img.shields.io/badge/Jellyfin-12.x-7B5BF2?style=for-the-badge&logo=jellyfin&logoColor=white)
![Direct Play](https://img.shields.io/badge/goal-Direct%20Play-2ea44f?style=for-the-badge)
![Reviewed](https://img.shields.io/badge/reviewed-September%202026-0969da?style=for-the-badge)

**[Quick start ↓](#-30-minute-setup)** · **[Plugins](PLUGINS.md)** · **[Clients](CLIENTS.md)** · **[Transcoding](HARDWARE-TRANSCODING.md)** · **[Remote access](REMOTE-ACCESS.md)**

</div>

---

## ✨ What a good Jellyfin setup looks like

<table>
<tr>
<td width="33%" valign="top">

### ▶️ Direct Play first

Choose clients that can play your media natively before throwing more CPU/GPU at transcoding.

**Goal:** less server work, faster seeking, fewer playback surprises.

</td>
<td width="33%" valign="top">

### 🧩 Keep plugins intentional

Jellyfin works without a giant plugin stack.

Install a plugin because you can explain exactly what problem it solves.

</td>
<td width="33%" valign="top">

### 🔐 Remote access safely

Prefer private VPN access or an HTTPS reverse proxy.

Avoid exposing raw `8096` as the default solution.

</td>
</tr>
</table>

---

## 🚀 30-minute setup

```text
Install Jellyfin
      ↓
Name media correctly
      ↓
Test Direct Play
      ↓
Enable hardware transcoding only if needed
      ↓
Add a small plugin set
      ↓
Back up
      ↓
Configure safe remote access
```

| Step | Do this | Guide |
|---|---|---|
| **1** | Install Jellyfin | **[INSTALLATION.md](INSTALLATION.md)** |
| **2** | Organize movies / TV correctly | **[MEDIA-ORGANIZATION.md](MEDIA-ORGANIZATION.md)** |
| **3** | Test playback and inspect Direct Play / Transcode status | Jellyfin dashboard |
| **4** | Configure GPU/iGPU acceleration if necessary | **[HARDWARE-TRANSCODING.md](HARDWARE-TRANSCODING.md)** |
| **5** | Add only useful plugins | **[PLUGINS.md](PLUGINS.md)** |
| **6** | Back up config/database | **[BACKUPS-AND-UPGRADES.md](BACKUPS-AND-UPGRADES.md)** |
| **7** | Add remote access | **[REMOTE-ACCESS.md](REMOTE-ACCESS.md)** |

---

## 🧱 Recommended baseline

| Layer | Starting point |
|---|---|
| **Server** | Jellyfin 12.x |
| **Media** | HDD / NAS storage is fine |
| **Config + database** | SSD if available |
| **Playback target** | Direct Play |
| **Transcoding** | Intel QSV / NVIDIA NVENC / AMD VA-API or AMF |
| **Subtitles** | Local subtitles first; OpenSubtitles if useful |
| **Plugins** | Small, deliberate set |
| **Requests** | Seerr for shared servers |
| **Remote access** | VPN or HTTPS reverse proxy |
| **Backups** | Before every major server upgrade |

> **Avoid the beginner trap:** public `8096` + 20 plugins + no backup + everything transcoding.

---

## 📺 Pick the client before upgrading the server

A different client can eliminate transcoding without changing the server at all.

| Platform | Start here | Also test |
|---|---|---|
| 📺 Android TV / Google TV / Fire TV | **Official Jellyfin for Android TV** | Wholphin, Moonfin, Kodi |
| 🤖 Android phone/tablet | **Official Jellyfin** | Streamyfin, Moonfin |
| 🍎 iPhone / iPad / Apple TV | **Official Jellyfin / Swiftfin where appropriate** | Streamyfin, Moonfin |
| 💻 Desktop / browser | **Jellyfin Web** | Community clients as needed |
| 🎞️ Advanced HTPC | **Kodi + Jellyfin** | mpv-based clients |

Test with files that represent your real library: **4K HEVC, HDR, PGS subtitles, high bitrate, TrueHD/DTS-HD/Atmos** if you use them.

**[Full client guide →](CLIENTS.md)**

---

## ⚡ Playback decision tree

```text
                    ┌─────────────────────┐
                    │      Media file     │
                    └──────────┬──────────┘
                               │
                  Can the client play it as-is?
                         ┌─────┴─────┐
                        Yes          No
                         │            │
                   Direct Play ✅     │
                                      ▼
                          Can streams be reused?
                             ┌────┴────┐
                            Yes        No
                             │          │
                       Direct Stream   Transcode
                            ✅          ⚙️
```

### Hardware paths

| Hardware | Typical path |
|---|---|
| **Intel** | Quick Sync Video (QSV) |
| **NVIDIA** | NVENC / NVDEC |
| **AMD on Linux** | VA-API |
| **AMD on Windows** | AMF where supported |

Do not tick every codec blindly. Enable what your hardware actually supports and verify in the dashboard.

**[Hardware transcoding guide →](HARDWARE-TRANSCODING.md)**

---

## 🧩 Plugin shortlist

| Plugin / service | Type | My default | Use it for |
|---|---|---|---|
| **OpenSubtitles** | Official plugin | ✅ Useful | Subtitle fetching |
| **Intro Skipper** | Community | ✅ If wanted | Intro / credits detection |
| **Playback Reporting** | Official plugin | ◻️ Optional | Usage statistics |
| **Reports** | Official plugin | ◻️ Optional | Library/admin reports |
| **Kodi Sync Queue** | Plugin | ◻️ Situational | Kodi-heavy setups |
| **UI modification plugins** | Community | ⚠️ Advanced | Cosmetic / layout changes |
| **Seerr** | Separate service | ✅ Shared servers | Media request workflow |

Jellyfin 12 is a major-version boundary, so third-party plugin compatibility matters more than usual.

**[Read the plugin guide before installing extras →](PLUGINS.md)**

---

## 📁 Media naming: fix this before adding scrapers

<table>
<tr>
<td width="50%" valign="top">

### 🎞️ Movie

```text
Movies/
└── Dune (2021) [imdbid-tt1160419]/
    └── Dune (2021) [imdbid-tt1160419].mkv
```

</td>
<td width="50%" valign="top">

### 📺 TV show

```text
Shows/
└── Breaking Bad (2008)/
    └── Season 01/
        └── Breaking Bad S01E01.mkv
```

</td>
</tr>
</table>

Wrong metadata is often a naming/identity problem, not a “need more metadata plugins” problem.

**[Media organization guide →](MEDIA-ORGANIZATION.md)**

---

## 🌐 Remote access choices

| Method | Best for | Recommendation |
|---|---|---|
| **VPN / private mesh** | Personal/family use | ✅ Easiest safe default |
| **Reverse proxy + HTTPS** | Normal public-domain access | ✅ Good when configured correctly |
| **Raw port-forward to 8096** | Quick testing | ⚠️ Avoid as permanent default |

```text
Private route
Device → VPN → Home network → Jellyfin

Public route
Internet → HTTPS reverse proxy → Jellyfin
```

**[Remote access guide →](REMOTE-ACCESS.md)**

---

## 🛟 Jellyfin 12 upgrade checklist

- [ ] Back up server config/database.
- [ ] Check every important third-party plugin for Jellyfin 12 support.
- [ ] Update or disable incompatible extras.
- [ ] Make sure you know how to restore the backup.
- [ ] Upgrade Jellyfin.
- [ ] Test login, library scan, subtitles, Direct Play and hardware transcoding.
- [ ] Re-enable optional UI/plugin modifications gradually.

**[Backup & upgrade guide →](BACKUPS-AND-UPGRADES.md)**

---

## 🧯 Fast troubleshooting map

| Symptom | Check first |
|---|---|
| Constant buffering | Is the client transcoding? Is bitrate too high? |
| CPU at 100% | Hardware acceleration disabled/misconfigured? |
| Wrong movie/show match | Folder/file naming and IDs |
| Subtitles trigger transcode | Subtitle format + client support |
| Works locally, fails remotely | DNS / reverse proxy / firewall / VPN path |
| Library cannot see media | Filesystem permissions / container mounts |
| UI plugin breaks after upgrade | Plugin compatibility with Jellyfin 12 |

**[Full troubleshooting guide →](TROUBLESHOOTING.md)**

---

## 📚 Complete guide map

<table>
<tr>
<td width="50%" valign="top">

### Build

- **[Installation](INSTALLATION.md)**
- **[Media organization](MEDIA-ORGANIZATION.md)**
- **[Hardware transcoding](HARDWARE-TRANSCODING.md)**
- **[Plugins](PLUGINS.md)**
- **[Clients](CLIENTS.md)**

</td>
<td width="50%" valign="top">

### Operate

- **[Remote access](REMOTE-ACCESS.md)**
- **[Backups & upgrades](BACKUPS-AND-UPGRADES.md)**
- **[Troubleshooting](TROUBLESHOOTING.md)**
- **[Sources](SOURCES.md)**

</td>
</tr>
</table>

---

## 🌐 Related repos

- **[homelab-from-zero](https://github.com/ish4ra/homelab-from-zero)** — build the server underneath Jellyfin
- **[selfhosted-picks](https://github.com/ish4ra/selfhosted-picks)** — other self-hosted apps worth running
- **[open-source-alternatives](https://github.com/ish4ra/open-source-alternatives)** — broader open-source replacements
- **[stremio-nuvio-streaming-setup-guide](https://github.com/ish4ra/stremio-nuvio-streaming-setup-guide)** — focused Stremio/Nuvio client guide

---

## Guide principles

**Direct Play > brute-force transcoding.**  
**Simple plugin stack > fragile customization stack.**  
**Backups > hoping an upgrade works.**  
**Safe remote access > easiest port-forward.**

This guide assumes you are serving media you are authorized to store and access.

---

<div align="center">

### Useful setup reference?

A ⭐ helps other Jellyfin users find it.

**Last reviewed: September 2026**

</div>
