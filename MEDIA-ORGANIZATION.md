# Media organization

Good naming is the foundation of a clean Jellyfin library. Fix names before adding extra metadata plugins.

## Movies

Recommended structure:

```text
Movies/
└── Dune (2021) [imdbid-tt1160419]/
    ├── Dune (2021) [imdbid-tt1160419].mkv
    └── Dune (2021) [imdbid-tt1160419].en.srt
```

The year and metadata-provider ID are optional, but they greatly reduce bad matches for remakes, similarly named films and regional releases.

A simpler valid layout is:

```text
Movies/
└── Dune (2021)/
    └── Dune (2021).mkv
```

## TV shows

Recommended structure:

```text
Shows/
└── Breaking Bad (2008)/
    ├── Season 01/
    │   ├── Breaking Bad S01E01.mkv
    │   └── Breaking Bad S01E02.mkv
    └── Season 02/
        └── Breaking Bad S02E01.mkv
```

Use `Season 01`, not just `S01`, for season folder names. Keep episodes inside season folders instead of mixing season folders and episode files at the same level.

For difficult matches you can include provider IDs in series names as well:

```text
Series Name (year) [tvdbid-12345]/
```

## Subtitles

Sidecar subtitles should normally sit beside the media file with the same base name.

```text
Movie Name (2026).mkv
Movie Name (2026).en.srt
Movie Name (2026).si.srt
```

Language tags make multi-language libraries much easier to manage.

## Multiple movie versions

Keep alternate cuts/encodes organized rather than creating unrelated duplicate folders. Jellyfin can group versions when naming is consistent, but client behavior may differ. Test your main clients before reorganizing a large library.

## Local artwork and NFO files

Jellyfin can use local metadata and `.nfo` files. Typical NFO filenames include:

- `movie.nfo` for movies;
- `tvshow.nfo` for series;
- `season.nfo` for seasons;
- an episode NFO matching the episode filename.

Local metadata takes precedence over remote providers, so stale NFO files can also cause confusing metadata. If a title refuses to refresh correctly, check for old local metadata before blaming the scraper.

## Library permissions

For maximum safety, Jellyfin only needs read access to media folders for normal playback.

Give Jellyfin write access only if you intentionally want features such as:

- deleting media through Jellyfin;
- saving downloaded subtitles beside the media;
- writing NFO metadata or artwork into media folders.

A read-only media mount is a strong default for Docker servers.

## Metadata troubleshooting order

When an item matches incorrectly:

1. Check folder and filename.
2. Add the release year.
3. Add an IMDb/TMDb/TVDB provider ID when needed.
4. Check for stale local NFO files.
5. Use Jellyfin's Identify/Refresh Metadata tools.
6. Only then consider extra metadata providers/plugins.
