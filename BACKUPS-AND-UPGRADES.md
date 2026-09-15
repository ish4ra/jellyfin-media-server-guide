# Backups and upgrades

Jellyfin upgrades can migrate the database. A backup is not optional if you care about being able to roll back.

## The key rule

**Back up before every major Jellyfin upgrade and before experimenting with unstable builds.**

Jellyfin does not provide a general downgrade mechanism after migrations have run. Restoring a pre-upgrade backup is the reliable rollback path.

## Built-in backup

Recent Jellyfin versions provide built-in backup/restore functionality from the server administration interface.

Use it before major upgrades and keep at least one copy outside the Jellyfin server disk.

Typical backup locations documented by Jellyfin include:

- Debian/Ubuntu packages: `/var/lib/jellyfin/data/backups`
- Windows user install: `%LOCALAPPDATA%\Jellyfin\data\backups`
- Windows service install: `%PROGRAMDATA%\Jellyfin\Server\data\backups`
- container installs: inside the persistent config/data volume, depending on image/layout

The exact path matters less than one principle: **a backup stored only on the same failing disk is not a real backup strategy.**

## Docker backup strategy

At minimum, preserve the persistent Jellyfin config/data volume.

Example layout:

```text
/srv/jellyfin/
├── config/
├── cache/
└── backups/
```

Your large media library normally does not need to be duplicated every time you back up Jellyfin's database/config, but your media itself should have whatever backup/redundancy strategy matches how replaceable it is.

## Before upgrading

1. Check Jellyfin release notes.
2. Check important third-party plugin compatibility.
3. Create a Jellyfin backup.
4. Copy/verify the backup outside the server's main data path.
5. Record the currently installed Jellyfin version.
6. Record any custom reverse-proxy or hardware-transcoding configuration.
7. Upgrade.
8. Test login, library scan, Direct Play, hardware transcode, subtitles and remote access.
9. Keep the old backup until you are confident the new version is stable.

## Plugin-heavy servers

For a major Jellyfin release, third-party plugins are often the most fragile part of the upgrade.

A safer sequence is:

```text
backup
→ confirm plugin compatibility
→ update/disable incompatible plugins
→ upgrade Jellyfin
→ test core playback
→ re-enable custom UI/plugins gradually
```

This is especially important for plugins that modify Jellyfin Web files or rely on internal APIs.

## Restore principle

If an upgrade applies migrations and the old version can no longer read the data, do not try to "fix" the migrated database by hand unless you know exactly what you are doing.

Restore the backup that matches the older Jellyfin version, then start the matching server version.

## Back up more than the database

Also preserve:

- reverse proxy configuration;
- Docker Compose files;
- environment variables/secrets in a secure location;
- custom plugin repository URLs;
- hardware device mappings/group IDs;
- notes about media mount paths;
- custom CSS, if you use it.

A tiny `SERVER-NOTES.md` stored privately can save hours during a rebuild.
