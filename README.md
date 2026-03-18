# clamav_protonvpn_ubuntu

ClamAV (on-demand scanner + GUI) and ProtonVPN setup for Ubuntu 24.04.4 LTS.

---

## References

- [ClamAV Introduction](https://docs.clamav.net/Introduction.html)
- [ClamAV Scanning](https://docs.clamav.net/manual/Usage/Scanning.html)
- [ProtonVPN — Official Linux install (Ubuntu)](https://protonvpn.com/support/official-linux-vpn-ubuntu)

---

## ClamAV

ClamAV is an open-source (GPLv2) antivirus toolkit maintained by Cisco. `clamtk` is a GUI frontend for on-demand scanning — no background daemon required.

**Minimum requirements:** 3 GiB RAM, 5 GiB free disk space.

### Install

```bash
sudo apt update
sudo apt install clamav clamtk -y
```

### Update signature database

```bash
sudo systemctl stop clamav-freshclam
sudo freshclam
sudo systemctl start clamav-freshclam
sudo systemctl enable clamav-freshclam
```

### Configure freshclam

```bash
sudo nano /etc/clamav/freshclam.conf
```

Find `Checks` and set it to:

```
Checks 1
```

Save with `Ctrl+S` → `Ctrl+X` to exit. Limits signature updates to once per day.

### Usage

Open **ClamTK** from your application menu to scan files and folders on demand.

To scan from the terminal:

```bash
clamscan /path/to/file
clamscan --recursive /home
sudo clamscan --recursive \
  --exclude-dir="^/proc" \
  --exclude-dir="^/sys" \
  --exclude-dir="^/dev" /
```

### Uninstall (no residue)

```bash
sudo apt purge clamav clamtk clamav-freshclam -y
sudo apt autoremove -y
sudo rm -rf /etc/clamav /var/log/clamav /var/lib/clamav
```

---

## ProtonVPN

Open-source, no-logs VPN. Free tier includes servers in 10 countries. Requires a [free Proton account](https://account.proton.me/signup).

### Install

> **Check for the latest version** before running: [protonvpn.com/support/official-linux-vpn-ubuntu](https://protonvpn.com/support/official-linux-vpn-ubuntu). The filename and version number in the `wget` line change with new releases.

```bash
wget https://repo.protonvpn.com/debian/dists/stable/main/binary-all/protonvpn-stable-release_1.0.8_all.deb
sudo dpkg -i ./protonvpn-stable-release_1.0.8_all.deb
sudo apt update
sudo apt install proton-vpn-gnome-desktop -y
```

Reboot after installation, then open **Proton VPN** from your application menu and sign in.

### Uninstall (no residue)

```bash
sudo apt purge proton-vpn-gnome-desktop protonvpn-stable-release -y
sudo apt autoremove -y
sudo rm -rf ~/.config/protonvpn
```
