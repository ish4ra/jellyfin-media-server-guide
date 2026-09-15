# Remote access

Jellyfin works perfectly well as a LAN-only server. You only need remote access if you want to stream while away from home or share access with trusted users.

## The four common approaches

### 1. VPN — simplest private option

A WireGuard/Tailscale-style VPN is often the easiest secure solution for one person or a small trusted group.

Pros:

- Jellyfin does not need to be directly exposed to the public internet;
- HTTPS/reverse-proxy configuration can be avoided for private use;
- useful for accessing other home services too.

Cons:

- every remote device needs VPN access/configuration;
- less convenient for casual family/friend access.

## 2. Reverse proxy + HTTPS — best normal public-domain setup

Use a domain/subdomain such as:

```text
https://jellyfin.example.com
```

and place Caddy, nginx, Traefik or another maintained reverse proxy in front of Jellyfin.

Caddy is attractive for home users because automatic HTTPS can make the setup simpler.

Typical flow:

```text
Internet
   ↓ HTTPS 443
Reverse proxy
   ↓ local HTTP
Jellyfin :8096
```

Only the reverse proxy should normally be exposed publicly; Jellyfin itself can stay on the private network/container network.

## 3. VPS reverse proxy / tunnel

Useful if your ISP uses CGNAT or inbound connections are difficult. A VPS can act as the public endpoint and forward traffic securely back home.

This adds cost and complexity, so do not start here unless your network actually requires it.

## 4. Direct port forwarding — not recommended

Jellyfin's own networking documentation lists direct internet port forwarding as not recommended.

Forwarding `8096` directly from your router is easy, but it exposes the Jellyfin HTTP service directly. Prefer a VPN or a properly configured HTTPS reverse proxy.

## Ports you should understand

Common defaults:

- `8096/TCP` — Jellyfin HTTP
- `8920/TCP` — Jellyfin HTTPS if configured directly
- `7359/UDP` — local client discovery

Discovery traffic is for local networks and should not be exposed as though it were a remote-access service.

## UPnP

The Jellyfin setup wizard can expose automatic port mapping options. UPnP is convenient but has a larger security surface. Leave automatic mapping disabled unless you specifically understand and want it.

## Reverse proxy checklist

If using a public domain:

- HTTPS certificate works;
- HTTP redirects to HTTPS;
- WebSocket/proxy headers are configured correctly for your proxy;
- Jellyfin knows the expected proxy/network ranges when necessary;
- router/firewall only exposes the ports you intend;
- admin account uses a strong unique password;
- normal users do not receive administrator rights;
- remote bandwidth limits are configured if your upload is limited.

## Upload bandwidth

Remote streaming is limited by your home upload speed.

Jellyfin's hardware guide recommends at least roughly 20 Mbps upload for a comfortable remote-access baseline. If your total upload is under 100 Mbps, leaving headroom instead of allowing Jellyfin to saturate the full connection is sensible.

Remember that one 4K remux can exceed the upload capacity of many residential connections. Remote bitrate limits may intentionally force transcoding.

## CGNAT

If router port forwarding looks correct but inbound connections never reach your network, your ISP may be using CGNAT.

Options include:

- VPN/mesh VPN;
- IPv6 where available and correctly firewalled;
- a VPS/tunnel solution;
- asking the ISP for a public/static IP.

Do not keep changing Jellyfin settings if the network path itself has no public inbound route.
