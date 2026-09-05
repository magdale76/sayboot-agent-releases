# SayBoot agent — releases

Binaries and packages of the **SayBoot agent** for Linux (servers, NAS, mini-PCs, dual-boot PCs).
Source code lives in a private repository; this repo exists only to host releases on a domain separate from sayboot.com.

- Windows users: install from the Microsoft Store (https://apps.microsoft.com/detail/9MVZDLP59914).
- Synology NAS: https://sayboot.com/synology/ (no SSH, Task Scheduler).
- Verify downloads: `sha256sum -c SHA256SUMS --ignore-missing`.

| File | Use |
|---|---|
| `sayboot-agent-linux-amd64` | Intel/AMD 64-bit (PCs, most NAS "+" models) — stable link: `releases/latest/download/sayboot-agent-linux-amd64` |
| `sayboot-agent-linux-arm64` | 64-bit ARM (Raspberry Pi 4/5, Realtek/Annapurna NAS) |
| `sayboot-agent-linux-armv7` | 32-bit ARM (older NAS, Raspberry Pi 2/3) |
| `sayboot-agent_<v>_<arch>.deb` | Debian/Ubuntu package (systemd service, polkit rule, WoL persistence) |

## Quick start — Debian / Ubuntu (systemd)

Open a terminal and run, one line at a time:

```sh
# 1. download and install the package (installs the service, polkit rule, WoL unit)
wget https://github.com/magdale76/sayboot-agent-releases/releases/latest/download/sayboot-agent_0.7.44_amd64.deb
sudo apt install ./sayboot-agent_0.7.44_amd64.deb

# 2. pair: get an 8-character code from the web app → Devices → Add → "Connect without a browser"
sudo sayboot-agent --code XXXXXXXX

# 3. check
systemctl status sayboot-agent          # active (running)
sudo sayboot-agent --diagnose           # Wake-on-LAN report, sent to support
sudo sayboot-agent --fix-wol auto       # arm magic-packet wake and keep it armed at boot
```

Then in the Alexa app / with your voice: "Alexa, discover devices" → "Alexa, turn off <name>" → (PC off) "Alexa, turn on <name>".

Notes:
- Dual boot: a magic packet powers the PC on; which OS starts is decided by the firmware/GRUB default. If Windows is the default, the Linux agent will not come back online after a wake — set Linux as default in GRUB or the UEFI boot order for the test.
- The device name Alexa hears is the Linux hostname at pairing time; rename it in the web app.
- Logs: `journalctl -u sayboot-agent -f`. State: `/var/lib/sayboot/`. Remove: `sudo apt remove sayboot-agent`.
- Other distros without apt: `sudo ./sayboot-agent-linux-amd64 --install-system`, then the same `--code` step.

© SayBoot. The binaries are provided for use with the SayBoot service (https://sayboot.com). Support: hello@sayboot.com
