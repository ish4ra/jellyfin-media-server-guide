# Hardware transcoding

The best playback path is **Direct Play**: the client can decode the original file and Jellyfin simply serves it. Transcoding happens when the client cannot handle the video, audio, subtitle format, bitrate or container.

## Direct Play first

Before buying a stronger GPU, check why a stream is transcoding. Common causes include:

- unsupported video codec/profile;
- unsupported audio codec;
- bitrate limit;
- subtitle burn-in;
- HDR-to-SDR tone mapping;
- container incompatibility;
- a client with a limited playback stack.

A better client can sometimes eliminate more transcoding than a hardware upgrade.

## Supported acceleration families

Jellyfin validates these major methods:

| Hardware | Windows | Linux | macOS |
| --- | --- | --- | --- |
| Intel | Quick Sync (QSV) | QSV | VideoToolbox on Apple hardware |
| NVIDIA | NVDEC/NVENC | NVDEC/NVENC | — |
| AMD | AMF | VA-API preferred | VideoToolbox on Apple hardware |
| Apple Silicon | — | — | VideoToolbox |
| Rockchip RK3588-class | — | RKMPP | — |

Use Jellyfin's own `jellyfin-ffmpeg` package/image. Generic FFmpeg builds can lack Jellyfin-specific acceleration support.

## Intel Quick Sync

Intel iGPUs are excellent Jellyfin hardware for power-efficient home servers.

Use QSV on supported Intel systems. On Linux, confirm a render device exists:

```bash
ls -l /dev/dri
```

You should normally see a `renderD*` device. Container setups must pass the appropriate device through and give the container user permission to use it.

## NVIDIA NVENC

NVIDIA GPUs provide strong decode/encode support and are straightforward on Windows when drivers are installed correctly. Linux containers require the NVIDIA container stack and GPU access to be configured on the host.

Do not assume every codec is supported by every NVIDIA generation. Check the GPU generation against Jellyfin's codec support tables before planning multiple 4K transcodes.

## AMD

- **Windows:** use AMF.
- **Linux:** Jellyfin recommends VA-API for AMD; it is the better-supported open stack.

On Linux, verify `/dev/dri` permissions and use Jellyfin's FFmpeg build.

## HDR and tone mapping

HDR-to-SDR tone mapping is much heavier than ordinary playback. A system that handles several normal hardware transcodes may behave very differently when tone mapping 4K HDR.

If all your playback devices support the original HDR format, Direct Play is preferable. Tone-map only when a client/display actually needs SDR output.

## Subtitle burn-in

Subtitles are a frequent hidden cause of transcoding. Image-based subtitle formats or clients that cannot render a particular subtitle format may force Jellyfin to burn subtitles into the video.

If a file unexpectedly uses high CPU/GPU:

1. disable subtitles temporarily;
2. check the Jellyfin playback/transcode reason;
3. try an SRT/WebVTT text subtitle;
4. compare with another client.

## Transcode cache

Jellyfin writes temporary segments during transcoding. Put the transcode path on SSD or fast storage when possible. Slow storage can bottleneck otherwise capable hardware.

For high-end setups, a RAM disk can work, but only if you understand the memory requirements and failure behavior. SSD is the sensible default.

## Raspberry Pi warning

Do not choose a Raspberry Pi expecting it to be a strong transcoding server. Jellyfin has deprecated the older V4L2 acceleration path for Raspberry Pi and the Pi 5 lacks a hardware encoder. Raspberry Pi can still be useful when your clients Direct Play almost everything.

## Verification checklist

After enabling hardware acceleration:

- play a file that really requires transcoding;
- open the Jellyfin dashboard and confirm a transcode is active;
- inspect the FFmpeg/transcode log;
- verify the expected hardware decoder/encoder is being used;
- check CPU usage is substantially lower than software transcoding;
- test HDR and subtitle cases separately.

Do not consider hardware acceleration "working" just because the checkbox is enabled.
