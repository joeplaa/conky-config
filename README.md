# Joeplaa Conky

Based on Pomaria-Side Conky: <https://www.github.com/SZinedine/pomaria-side>

## Hardware specs

- PC: Lenovo P520
- CPU: Intel Xeon W-2135 (6-core, 12 threads)
- GPU: Nvidia RTX 5060Ti
- SSD: Samsung NVMe PCIe gen 3x4
- NIC: dual setup — `eno1` (1 Gbit onboard); Mellanox port 0 as `encx31` with `br0` (bridge for QEMU); Conky uses `eno1` and `encx31` for throughput, `eno1` and `br0` for IPs

## Download and Launch

```sh
git clone https://www.github.com/joeplaa/conky-config
cd conky-config

conky -c conkyrc
# or
./start_conky.sh
```

Create a local `env.lua` in the repo root (required by `abstract.lua`; the file is gitignored). An empty file is enough unless you add your own variables.

## Edit / customize

In `settings.lua`:

- `THEME` — e.g. `blue dark`, `monochrome light`, `gruvbox dark`, `gruvbox light`, and other variants defined in that file
- `net_interface0` / `net_interface1` — interfaces for throughput graphs
- `ip_interface0` / `ip_interface1` — interfaces for local IP display
- `download_rate_max` / `upload_rate_max` — WAN speed caps for bars
- `net_rate_max` / `net_rate_max2` — LAN speed caps
- `nvme_max` — NVMe throughput cap for STORAGE I/O bars
- `net_fs_mounts` and `net_fs_refresh_rate` — network mount paths and refresh interval for STORAGE
- `cpu_cores`, warning colors, WAN/VPN IP refresh rates, and other knobs at the top of the file

In `start.lua` — layout and which sections are drawn.

In `conkyrc` — `own_window_argb_value = 160` controls window transparency (0–255).

## Launch on startup (Ubuntu)

Go to "Activities" or "Applications" and search for "Startup". Open the "Startup Applications Preferences" app.

Add a new program:

- Name: `Conky PC`
- Command: `<path to repository>/conky-config/start_conky.sh`
- Comment: leave blank or enter whatever you like

Or use `systemd`:

- Edit the example unit in the repo: `home/joep/.config/systemd/user/conky.service` — set `ExecStart`, `WorkingDirectory`, and `Environment=DISPLAY=...` (find display with `echo $DISPLAY`)
- Copy it to `~/.config/systemd/user/conky.service`
- Enable: `systemctl --user enable conky.service`
- Start: `systemctl --user start conky.service`

## Dependencies

- Conky 1.10+
- cairo
- imlib2
- NVIDIA driver and `nvidia-smi` (GPU metrics)
- `curl` (public WAN IP and VPN IPv6)
- `natpmpc` (VPN IPv4 via NAT-PMP)
