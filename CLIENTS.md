# Clients

The server is only half of the Jellyfin experience. Client codec support determines whether you Direct Play or transcode, and different clients can feel completely different on the same server.

## Official clients first

Start with an official Jellyfin client on each platform. They are the baseline for compatibility and support.

- **Android:** Jellyfin for Android
- **Android TV / Fire TV:** Jellyfin for Android TV
- **iPhone / iPad:** Jellyfin for iOS
- **Apple platforms:** Swiftfin is an official beta/native-style option
- **Roku:** Jellyfin for Roku
- **Web:** the Jellyfin web client included with the server

If the official client meets your needs, there is no requirement to replace it.

## Community clients worth testing

### Streamyfin

A modern open-source Jellyfin client with a strong focus on mobile/Apple/Android experiences. Current features include intro/credit support, trickplay, downloads, Seerr integration and an optional companion plugin for synchronized settings.

Good fit if you want:

- a more modern mobile UI;
- offline/download workflows;
- Seerr integration in the client;
- MPV-based playback.

### Moonfin

Moonfin is a cross-platform open-source Jellyfin/Emby client ecosystem covering mobile, desktop, TV and web targets. It also has an optional server companion plugin for settings synchronization and integrations.

Good fit if you want one similar client experience across several device types.

### Wholphin

An open-source Android TV / Fire TV Jellyfin client with a custom TV-first interface and MPV/ExoPlayer playback options.

It is popular with users who want an alternative to the official Android TV interface. **Check the project's current Jellyfin server compatibility before switching**, especially immediately after a major Jellyfin release.

### Kodi + Jellyfin integration

Kodi can be an excellent living-room client when codec support and Direct Play matter more than a simple app-store experience. It is more involved to configure than a normal Jellyfin client but works well for advanced home-theater setups.

## How to choose a client

Do not choose only by screenshots. Test these files from your own library:

- 1080p H.264 + AAC/AC3;
- 4K HEVC 10-bit;
- HDR10 / Dolby Vision if you use them;
- TrueHD/DTS-HD/Atmos audio if relevant;
- SRT subtitles;
- image-based subtitles such as PGS;
- high-bitrate remux files.

Then check the Jellyfin dashboard to see whether playback is:

- **Direct Play** — ideal;
- **Direct Stream** — usually fine, container/remux adjustment only;
- **Transcode** — the server is converting something.

A client that Direct Plays your library is often a better upgrade than a faster server.

## Suggested starting points

### Android phone/tablet

1. Official Jellyfin for Android
2. Streamyfin
3. Moonfin

### Android TV / Google TV / Fire TV

1. Official Jellyfin for Android TV
2. Wholphin
3. Moonfin
4. Kodi for advanced home-theater users

### iPhone / iPad / Apple TV

1. Official Jellyfin / Swiftfin where appropriate
2. Streamyfin
3. Moonfin

Commercial clients such as Infuse can also be excellent on Apple hardware, but this guide focuses primarily on free/open-source options.

## Client-specific plugin warning

A server-side web UI plugin does not automatically affect every native TV/mobile client. Many custom home-screen/theme plugins only modify Jellyfin Web or clients that embed/use that web interface.

Before installing a visual plugin, ask: **Does my main client actually render this feature?**
