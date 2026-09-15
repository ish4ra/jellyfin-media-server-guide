# Plugins

Jellyfin is usable without any third-party plugins. Start lean, then add features you can explain to yourself in one sentence.

> Jellyfin 12 is a major-version transition. Confirm a third-party plugin explicitly supports your installed Jellyfin version before installing or upgrading it.

## My recommended order

### 1. OpenSubtitles — useful for many libraries

**Type:** Official Jellyfin project plugin

Downloads subtitles from OpenSubtitles. It is a sensible first plugin if your media frequently lacks local subtitles.

Why install it:

- convenient subtitle fetching;
- maintained under the Jellyfin GitHub organization;
- less manual subtitle hunting.

Considerations:

- requires an OpenSubtitles account/API setup depending on current service requirements;
- downloaded subtitles may require write access if you save them beside media.

### 2. Intro Skipper — strongest community recommendation

**Type:** Third-party community plugin

Intro Skipper analyzes episode audio to detect recurring intro sequences. In 2026 Reddit recommendation threads it is one of the most consistently recommended Jellyfin additions.

Current upstream requirements for the Jellyfin 12 branch include Jellyfin 12+ and a sufficiently new Jellyfin FFmpeg build.

Important Jellyfin 12 behavior:

- Intro Skipper itself handles detection/segments;
- modern UI integration is not simply the old "plugin modifies Jellyfin Web" model;
- some extra web UI behavior can use the **File Transformation** plugin;
- client support varies, so test the client you actually watch on.

Install it because you want intro/credit skipping, not because every server "must" have it.

### 3. Playback Reporting — optional analytics

**Type:** Official Jellyfin project plugin

Collects and visualizes user/media playback activity.

Good for:

- shared/family servers;
- seeing what is actually watched;
- troubleshooting usage patterns;
- basic server statistics.

Skip it if you know you will never look at analytics. Several community discussions include users who installed statistics plugins and then never used the data.

## Good optional plugins

### Reports

**Type:** Official Jellyfin project plugin

Generates activity/media reports and can export data. Useful for admins managing larger libraries; unnecessary for a small single-user server.

### Kodi Sync Queue

Useful if Kodi is a major Jellyfin client in your house. It improves synchronization workflows for Jellyfin-for-Kodi style setups. Do not install it just because you own one TV.

### Merge Versions

Useful for libraries containing multiple encodes/cuts/versions of the same title. Test how your clients present grouped versions before reorganizing a large collection around it.

## UI-heavy third-party plugins

These are interesting, but they are the first group I would remove when debugging a major Jellyfin upgrade.

### Home Screen Sections

Adds a more configurable, streaming-service-style home screen. Version 3.0 added Jellyfin 12 support in September 2026.

It can depend on compatible releases of related web-extension plugins such as File Transformation and Plugin Pages. Read its upstream README/release notes before installing.

### File Transformation

A framework/dependency used by several community web-interface plugins. Do not install it simply because it sounds useful; install it when another plugin you intentionally use requires it.

### Jellyfin Enhanced / Kefin-style UI tweaks

These projects can add extensive web-interface customizations. They are popular with users who want a Netflix-like experience, but they also increase the number of moving parts after a major Jellyfin update.

Treat them as **advanced/customization**, not core server components.

## Search plugins

Meilisearch/Jellysearch-style integrations can improve search in very large libraries, but they add another service and index to maintain. The default Jellyfin search is enough for many home libraries.

Use external search only when you have actually measured a search problem.

## Companion services are not Jellyfin plugins

### Seerr

Seerr is a separate media request/discovery manager, not a Jellyfin plugin. In 2026 the Jellyseerr and Overseerr projects unified under **Seerr**, with Jellyfin and Emby support in the shared project.

Use it when multiple people use your server and you want a clean request workflow.

### Client companion plugins

Apps such as Streamyfin or Moonfin may offer their own optional companion server plugins for synchronized settings or Seerr integration. Only install the companion plugin if you use that client and need those features.

## Community consensus: less is usually better

Recent Jellyfin Reddit threads repeatedly recommend Intro Skipper, while UI stacks vary heavily between users. A long list of web-modification plugins can look impressive but increases upgrade risk.

A strong default is:

```text
OpenSubtitles
+ Intro Skipper (if you watch episodic content)
+ Playback Reporting (only if you want analytics)
```

Then add one feature at a time.

## Upgrade checklist for plugins

Before a Jellyfin major upgrade:

1. Back up Jellyfin.
2. List every non-official plugin you depend on.
3. Check each upstream project for explicit compatibility with the new Jellyfin version.
4. Update plugins first when the maintainer instructs you to.
5. Disable/remove incompatible web-modification plugins if necessary.
6. Upgrade Jellyfin.
7. Re-enable extras gradually and test playback after each group.

This makes it far easier to identify whether a problem comes from Jellyfin itself or a plugin.
