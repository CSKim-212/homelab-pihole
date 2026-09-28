# homelab-pihole
README for PiHole
# Pi-hole DNS Filtering Lab

Network-wide DNS ad/tracker blocking deployed on a Raspberry Pi, 
with a Beelink mini PC configured as a client.

## What I Built
- Headless Raspberry Pi setup (SSH-only, no monitor)
- Static IP configuration via NetworkManager (nmcli)
- Pi-hole DNS filtering service with Cloudflare upstream DNS
- StevenBlack unified blocklist (74k+ domains)

## Setup Steps
1. Flashed Raspberry Pi OS Lite headlessly via Raspberry Pi Imager
2. Enabled SSH via boot partition flag
3. Configured static IP: `sudo nmcli connection modify <conn> ipv4.addresses <ip>/24 ...`
4. Installed Pi-hole: `curl -sSL https://install.pi-hole.net | bash`
5. Pointed a second device (Beelink) at the Pi-hole IP for DNS

## Problem I Diagnosed
After setup, Pi-hole showed 0 blocked queries despite normal browsing. 
Root cause: the client browser had DNS-over-HTTPS (Secure DNS) enabled, 
which bypasses OS-level DNS settings entirely and sends queries directly 
to Cloudflare/Google over HTTPS. Disabling Secure DNS in the browser 
restored expected DNS resolution through Pi-hole, confirmed via:

nslookup doubleclick.net <pi-hole-ip>   # returned 0.0.0.0 (blocked)

## What This Taught Me
- The practical difference between DNS-layer filtering (blocks known bad 
  domains before resolution) vs content-layer filtering (blocks ad 
  elements on the page itself) — DNS filtering can't catch first-party 
  ads served from a site's own domain
- How client-side DNS-over-HTTPS settings can silently bypass network-level 
  DNS security controls, which is relevant to real-world DNS filtering/
  security tooling in enterprise environments

## Tools Used
Raspberry Pi OS Lite, Pi-hole, NetworkManager (nmcli), SSH

## Add Pihole lab writeup
