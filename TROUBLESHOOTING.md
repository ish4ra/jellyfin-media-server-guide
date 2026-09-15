# Troubleshooting

Use this order: **identify the layer that is failing before changing settings.** Jellyfin problems usually belong to storage/permissions, metadata, networking, client compatibility, transcoding, or plugins.

## Library is empty / folders do not appear

Check:

- the Jellyfin process/container can actually read the host path;
- Docker bind mounts point to the correct host directory;
- the service account has traverse/read permission on every parent directory;
- NAS shares are mounted before Jellyfin starts;
- SELinux/AppArmor rules are not blocking access where applicable.

For a Windows service, confirm the service account has at least read/execute access to the media path.

## Wrong poster / wrong movie match

Fix in this order:

1. Rename the folder to `Movie Name (year)`.
2. Make the media filename match the folder name.
3. Add a provider ID such as `[imdbid-tt...]` when needed.
4. Check for stale `.nfo` files.
5. Use Identify / Refresh Metadata.

Do not install five metadata plugins to compensate for bad filenames.

## Playback buffers on LAN

First determine whether the stream is Direct Play or Transcoding.

If **Direct Play** buffers:

- test wired Ethernet;
- check Wi-Fi signal/congestion;
- check storage read speed;
- try another client;
- check whether the file bitrate is unusually high.

If **Transcoding** buffers:

- verify hardware acceleration is actually active;
- check CPU/GPU usage;
- inspect the transcode log;
- put the transcode cache on SSD;
- test with subtitles disabled;
- reduce output bitrate temporarily to isolate throughput issues.

## CPU is at 100%

Likely causes:

- software video transcoding;
- subtitle burn-in;
- HDR tone mapping without effective hardware acceleration;
- unsupported codec/profile;
- audio transcoding combined with other work;
- background library/plugin tasks.

Open the Jellyfin dashboard while the stream is playing and inspect the playback/transcode reason.

## Hardware acceleration checkbox is enabled but CPU is still high

A checked box is not proof of acceleration.

Verify:

- correct vendor method (QSV/NVENC/VA-API/AMF);
- GPU device visible to Jellyfin/container;
- correct permissions/groups;
- Jellyfin FFmpeg is installed;
- the codec is supported by that GPU generation;
- transcode logs show the hardware decoder/encoder.

## 4K HDR looks washed out

Common causes include incorrect tone mapping or a client forcing an unnecessary transcode.

Test:

1. Direct Play to an HDR-capable display/client.
2. Disable subtitles.
3. Compare HDR Direct Play vs HDR-to-SDR transcode.
4. Check Jellyfin's tone-mapping configuration for your GPU.

## Subtitles cause transcoding

Image-based subtitles such as PGS may force burn-in on clients that cannot render them directly.

Try:

- an SRT subtitle;
- a different client;
- client-side subtitle rendering options;
- disabling subtitles to confirm they are the trigger.

## Remote access works locally but not outside home

Check:

- public IP vs CGNAT;
- router forwarding/reverse proxy;
- firewall rules;
- DNS record;
- HTTPS certificate;
- ISP inbound restrictions;
- whether you are testing from a genuinely external network (mobile data, not the same Wi-Fi).

Do not expose discovery UDP ports to solve a remote HTTP problem.

## Reverse proxy gives login loops / broken WebSocket behavior

Verify the reverse proxy configuration against current Jellyfin documentation for your proxy. Confirm forwarded headers and WebSocket handling, and check whether Jellyfin's known-proxy/local-network settings match your architecture.

## Plugin disappeared or broke after Jellyfin upgrade

Assume compatibility first, not corrupted Jellyfin.

1. Check the plugin upstream release page.
2. Confirm it supports your exact Jellyfin major version.
3. Remove/disable web-modification plugins first.
4. Restart Jellyfin.
5. Test with a clean browser session/cache.
6. Reinstall a compatible plugin version only after core Jellyfin works.

## Intro Skipper detects intros but no button appears

Detection and client UI support are separate pieces.

Check:

- Intro Skipper version supports Jellyfin 12;
- Jellyfin FFmpeg meets the plugin requirement;
- analysis has completed;
- your client actually supports the skip segment/UI behavior;
- any optional File Transformation/UI dependency required for that specific web enhancement is installed and compatible.

## Jellyfin is slow after adding thousands of files

Let the initial scan finish. Metadata downloads, image processing, trickplay generation and plugins can create heavy temporary load.

Then consider:

- database/config on SSD;
- scheduled heavy tasks outside viewing hours;
- reducing unnecessary metadata providers;
- avoiding duplicate libraries pointing at the same media;
- external search tools only if default search is genuinely a bottleneck.

## Logs to check

The most useful evidence is usually:

- main Jellyfin server log;
- FFmpeg/transcode log for playback failures;
- Docker/container logs;
- reverse-proxy logs for remote-access failures.

When asking for help, include the relevant log section and explain what client/file triggered it instead of posting only "it buffers".
