# Sources and research notes

Last reviewed: September 2026.

This guide prioritizes official Jellyfin documentation for server behavior and uses upstream project documentation plus recent community discussions for optional clients/plugins.

## Official Jellyfin documentation

- Documentation home: https://jellyfin.org/docs/
- Installation: https://jellyfin.org/docs/general/installation/
- Windows installation: https://jellyfin.org/docs/general/installation/windows/
- Debian/Ubuntu and Linux installation: https://jellyfin.org/docs/general/installation/linux/
- Container installation: https://jellyfin.org/docs/general/installation/container/
- Setup wizard: https://jellyfin.org/docs/general/post-install/setup-wizard/
- Hardware selection: https://jellyfin.org/docs/general/administration/hardware-selection/
- Hardware acceleration: https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/
- Intel acceleration: https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/intel/
- AMD acceleration: https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/amd/
- Networking: https://jellyfin.org/docs/general/post-install/networking/
- Plugins: https://jellyfin.org/docs/general/server/plugins/
- Backup and restore: https://jellyfin.org/docs/general/administration/backup-and-restore/
- Movie naming: https://jellyfin.org/docs/general/server/media/movies/
- TV show naming: https://jellyfin.org/docs/general/server/media/shows/
- NFO metadata: https://jellyfin.org/docs/general/server/metadata/nfo/
- Client list: https://jellyfin.org/downloads/clients/all/

## Official Jellyfin project plugins

- OpenSubtitles: https://github.com/jellyfin/jellyfin-plugin-opensubtitles
- Playback Reporting: https://github.com/jellyfin/jellyfin-plugin-playbackreporting
- Reports: https://github.com/jellyfin/jellyfin-plugin-reports

## Community plugins / clients checked

- Intro Skipper: https://github.com/intro-skipper/intro-skipper
- Home Screen Sections: https://github.com/IAmParadox27/jellyfin-plugin-home-sections
- Streamyfin: https://github.com/streamyfin/streamyfin
- Moonfin: https://github.com/Moonfin-Client/Moonfin-Core
- Wholphin: https://github.com/damontecres/Wholphin

Third-party projects change quickly around major Jellyfin releases. Their inclusion here is not a guarantee of compatibility with every Jellyfin 12.x point release. Check upstream release notes before installation.

## Seerr

Jellyseerr and Overseerr unified under the Seerr project in 2026.

- Announcement: https://docs.seerr.dev/blog/seerr-release/
- Migration guide: https://docs.seerr.dev/migration-guide/

## Recent community research

Recent r/jellyfin discussions reviewed while preparing the September 2026 version of this guide included:

- "Plugin recommendations for 12.0?" — September 2026
- "Which Jellyfin plugins are actually worth installing vs just sitting there unused?" — August 2026
- "Must have plugins of 2026" — February 2026
- "What are your must-have or life-changing plugins in 2026?" — March 2026
- "Any plug-ins you'd say are a must have?" — June 2026
- Jellyfin 12 upgrade experience threads from September 2026

The recurring takeaway was not "install everything". Intro Skipper receives unusually consistent praise; UI/customization stacks vary significantly and can introduce extra upgrade friction.

## How to keep this guide current

When a new Jellyfin major release appears:

1. Check official migration/release notes.
2. Verify hardware acceleration docs for FFmpeg/package changes.
3. Check every third-party plugin listed here for explicit compatibility.
4. Review maintained clients for current server-version support.
5. Update this source page and the `Last reviewed` date.
