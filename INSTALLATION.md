# Installation

This guide favors supported installation methods and predictable upgrades.

## Windows

For a normal home server, use the official Windows installer.

1. Download the current stable Jellyfin Windows installer from the official Jellyfin downloads page.
2. Run the installer.
3. Finish the setup wizard at `http://localhost:8096`.
4. Add your media libraries.
5. Configure hardware acceleration after basic playback works.

Running Jellyfin as a Windows service is useful for a dedicated always-on machine, but a normal tray install is simpler for most users. If you do use a service, give the service account only the media-folder permissions it actually needs.

## Debian / Ubuntu

Jellyfin provides an official repository setup script for Debian/Ubuntu-family systems.

Verify the script first:

```bash
curl -s https://repo.jellyfin.org/install-debuntu.sh -O
curl -s https://repo.jellyfin.org/install-debuntu.sh.sha256sum -O
sha256sum -c install-debuntu.sh.sha256sum
```

If verification succeeds, inspect it if you want, then run:

```bash
sudo bash install-debuntu.sh
```

After installation:

```bash
sudo systemctl status jellyfin
```

Open:

```text
http://SERVER-IP:8096
```

## Docker on Linux

Docker is a good choice when you already manage services with containers. Jellyfin officially supports container deployments on Linux; Docker on Windows/macOS is not the recommended Jellyfin path, especially when hardware transcoding matters.

Example `compose.yml`:

```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - "8096:8096"
    volumes:
      - ./config:/config
      - ./cache:/cache
      - /path/to/media:/media:ro
    environment:
      - TZ=Asia/Colombo
```

Start it:

```bash
docker compose up -d
```

Then open `http://SERVER-IP:8096`.

### Docker hardware acceleration

Do not copy random `/dev/dri` or NVIDIA examples before checking your hardware. GPU access differs by vendor and host setup. First get normal software playback working, then follow the vendor-specific hardware acceleration guide.

For Intel/AMD on Linux you will usually expose the required `/dev/dri` render device and ensure the container user can access it. NVIDIA normally requires the NVIDIA container runtime/toolkit and the correct device configuration.

## NAS platforms

- **TrueNAS SCALE:** Linux-based and supported as a platform, though the app packaging itself may be maintained outside the core Jellyfin project.
- **Synology:** use the Jellyfin-supported/community package path documented for your DSM version or a Linux container where appropriate.
- **Unraid:** container deployment is common; keep config/appdata on protected storage and media mounts explicit.

Do not treat old TrueNAS CORE / FreeBSD instructions as equivalent to SCALE. Jellyfin does not officially support FreeBSD-based installs because .NET support is the limiting factor.

## First-run wizard

During the wizard:

1. Create the admin account.
2. Add media libraries.
3. Choose metadata language and region.
4. Leave automatic UPnP port mapping disabled unless you have a specific reason to use it.
5. Finish the wizard and confirm local playback before configuring remote access.

## Recommended storage layout

```text
Jellyfin server disk (SSD)
├── Jellyfin config/database
├── Jellyfin cache
└── Transcode temp directory

Media storage
├── Movies
├── Shows
├── Music
└── Other libraries
```

Keeping the database, cache and transcode temp area on SSD makes the interface and transcoding pipeline more responsive while allowing large media files to remain on HDD or NAS storage.
