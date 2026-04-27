# Autoscript Multiport Fahri Alimudin - Ubuntu 22.04/24.04 LTS

Paket ini adalah revisi autoscript VPN agar lebih aman dipasang pada **Ubuntu 22.04 LTS** dan **Ubuntu 24.04 LTS** arsitektur **x86_64/amd64**.


## Revisi SENEN

- Output akun VMess, VLess, dan Trojan pada menu create/trial sudah dibuat direct-domain agar lebih kompatibel saat link dicopy ke NetMod.
- Menu Bot dihapus dari menu utama. File menu bot dan request notifikasi Telegram bawaan juga dibersihkan supaya paket lebih ringan.
- Struktur repository disederhanakan menjadi `setup.sh`, `README.md`, `media/`, `menu/`, dan `slowdns/`.

## Ringkasan perbaikan Ubuntu 24.04

- Status OpenVPN di menu diperbaiki agar membaca unit Ubuntu modern: `openvpn-server@server-tcp` dan `openvpn-server@server-udp`, bukan hanya `openvpn.service`.
- Installer OpenVPN diperbarui untuk Ubuntu 24.04: enable instance server TCP/UDP, aktifkan `tun`, IP forwarding, NAT `iptables`, dan `netfilter-persistent`.
- Menu `run` diperbaiki agar status service memakai `systemctl is-active`, sehingga service `active (exited)` seperti `netfilter-persistent` dan `rc-local` tidak salah terbaca OFF.
- `rc-local` dibuat ulang sebagai service systemd compatibility dan langsung di-enable.
- `Autoreboot` sekarang dicek dari `/etc/cron.d/daily_reboot` + service `cron`, bukan disamakan dengan `rc-local`.
- Konfigurasi Xray dibuat ulang lebih rapi dan tetap menyimpan marker akun `#vless`, `#vmess`, `#trojanws`, `#ssws`, `#vlessgrpc`, `#vmessgrpc`, `#trojangrpc`, dan `#ssgrpc` agar menu add/renew/delete tetap bekerja.
- File menu `prot`, `run`, dan `restart` diperbarui untuk daftar port dan service Ubuntu 24.04.
- `setup.sh` bisa memakai file lokal dari ZIP ini atau mengambil dari repository raw GitHub.

> Catatan: file binary bawaan seperti `ws`, `udp-mini`, `ftvpn`, `dnstt-server`, `dnstt-client`, dan paket `noobzvpns.zip` dipertahankan dari paket awal. Saya memperbaiki script, config, service, dan menu; binary tertutup tersebut tidak saya decompile.

---

## Syarat VPS

- Ubuntu **22.04 LTS** atau **24.04 LTS** fresh install.
- RAM minimal 1 GB disarankan.
- Login root.
- Domain/subdomain sudah mengarah ke IP VPS via **A record**.

Contoh DNS:

```text
vpn.domainanda.com  A  IP_VPS_ANDA
```

---

## Link bash instalasi 1x paste

Upload isi ZIP ini ke repository GitHub Anda, contoh:

```text
https://github.com/saliminwaskinahzzxxcc/abcde
```

Lalu jalankan 1x paste di VPS fresh install:

```bash
apt update -y && apt install -y wget curl ca-certificates && wget -q https://raw.githubusercontent.com/saliminwaskinahzzxxcc/abcde/main/setup.sh && chmod +x setup.sh && ./setup.sh
```

Jika nama repository berbeda, ganti URL raw pada perintah di atas. Contoh format:

```bash
apt update -y && apt install -y wget curl ca-certificates && wget -q https://raw.githubusercontent.com/USER/REPO/main/setup.sh && chmod +x setup.sh && REPO="https://raw.githubusercontent.com/USER/REPO/main/" ./setup.sh
```

Saat installer meminta domain, isi domain yang sudah diarahkan ke IP VPS:

```text
Masukan Domain: vpn.domainanda.com
```

Setelah selesai, VPS akan reboot otomatis.

---

## Cara install dari file ZIP tanpa GitHub

Upload `SENEN.zip` ke VPS, lalu jalankan:

```bash
sudo -i
apt update -y && apt install -y unzip wget curl ca-certificates
unzip SENEN.zip
chmod +x setup.sh
chmod +x setup.sh
./setup.sh
```

Mode ini memakai file lokal di folder ZIP, sehingga cocok untuk testing sebelum upload ke GitHub.

---

## Patch VPS yang sudah terinstall

Paket SENEN ini sudah dirapikan untuk fresh install. File patch terpisah tidak disertakan agar struktur repository tetap sederhana.

---

## Port publik terbaru

| Port | Protocol | Service / Keterangan | Status akses |
|---:|:---:|---|---|
| 22 | TCP | OpenSSH direct | Publik |
| 2222 | TCP | OpenSSH alternatif | Publik |
| 2223 | TCP | OpenSSH alternatif/backend | Publik |
| 109 | TCP | Dropbear extra | Publik |
| 143 | TCP | Dropbear default | Publik |
| 80 | TCP | HAProxy HTTP, SSH WS, Xray WS Non TLS | Publik |
| 55 | TCP | HAProxy HTTP alternatif | Publik |
| 8880 | TCP | HAProxy HTTP alternatif | Publik |
| 2082 | TCP | HAProxy HTTP alternatif | Publik |
| 443 | TCP | HAProxy TLS, SSH WS TLS, Xray WS TLS, Xray gRPC, OpenVPN SSL/WS-SSL | Publik |
| 8443 | TCP | HAProxy TLS alternatif | Publik |
| 2087 | TCP | HAProxy TLS alternatif | Publik |
| 2096 | TCP | HAProxy TLS alternatif | Publik |
| 81 | TCP | Nginx download config OpenVPN | Publik |
| 88 | TCP | SlowDNS client local tunnel | Publik jika dipakai |
| 1194 | TCP | OpenVPN TCP direct | Publik |
| 2200 | UDP | OpenVPN UDP direct | Publik |
| 53 | UDP | SlowDNS DNS entry, redirect ke 5300 | Publik jika SlowDNS dipakai |
| 5300 | UDP | SlowDNS server backend | Publik/redirect |
| 7100 | UDP | BadVPN / UDP mini 1 | Publik |
| 7200 | UDP | BadVPN / UDP mini 2 | Publik |
| 7300 | UDP | BadVPN / UDP mini 3 | Publik |
| 7443 | TCP | NoobzVPN SSL | Publik |
| 8080 | TCP | NoobzVPN TCP | Publik |

## Port backend/local

Port berikut dipakai sebagai backend internal. Jangan dipakai aplikasi lain.

| Port | Protocol | Service / Keterangan |
|---:|:---:|---|
| 10000 | TCP local | Xray API dokodemo-door |
| 10001 | TCP local | Xray VLess WebSocket backend |
| 10002 | TCP local | Xray VMess WebSocket backend |
| 10003 | TCP local | Xray Trojan WebSocket backend |
| 10004 | TCP local | Xray Shadowsocks WebSocket backend |
| 10005 | TCP local | Xray VLess gRPC backend |
| 10006 | TCP local | Xray VMess gRPC backend |
| 10007 | TCP local | Xray Trojan gRPC backend |
| 10008 | TCP local | Xray Shadowsocks gRPC backend |
| 10012 | TCP local | WS-ePro OpenVPN backend |
| 10015 | TCP local | WS-ePro SSH/Dropbear backend |
| 1010 | TCP local | Nginx WS reverse proxy dari HAProxy |
| 1012 | TCP local | Nginx OpenVPN WS reverse proxy |
| 1013 | TCP local | Nginx gRPC reverse proxy |

---

## Keterangan service di menu `run`

| Label menu | Unit/komponen yang dicek |
|---|---|
| Service SSH / TUN | `ssh` / `sshd` |
| Service SSH UDP | `udp-custom` |
| Service OpenVPN SSL / WS-SSL / UDP / TCP | `openvpn-server@server-tcp`, `openvpn-server@server-udp`, fallback `openvpn` |
| Service OHP SSH | mengikuti backend `ws` karena paket awal tidak memiliki unit OHP terpisah |
| Service OHP Dropbear | mengikuti `dropbear` karena paket awal tidak memiliki unit OHP terpisah |
| Service OHP OpenVPN | mengikuti OpenVPN server TCP/UDP karena paket awal tidak memiliki unit OHP terpisah |
| Service WS ePRO | `ws` |
| Service BadVPN 7100/7200/7300 | `udp-mini-1`, `udp-mini-2`, `udp-mini-3` |
| Service Dropbear | `dropbear` |
| Service Haproxy | `haproxy` |
| Service Crons | `cron` |
| Service Nginx Webserver | `nginx` |
| Service Xray VMess/VLess/Trojan/Shadowsocks | `xray` |
| Service Akun NoobzVpn | `noobzvpns` |
| Service Iptables | `netfilter-persistent` active/enabled |
| Service RClocal | `rc-local` active/enabled atau `/etc/rc.local` executable |
| Service Autoreboot | file `/etc/cron.d/daily_reboot` + `cron` aktif |

---

## Perintah setelah install

Masuk ulang ke VPS, lalu jalankan:

```bash
menu
```

Cek status lengkap:

```bash
run
```

Restart semua service:

```bash
restart
```

Cek port aktif:

```bash
ss -tulpen
```

Cek status OpenVPN Ubuntu 24.04:

```bash
systemctl status openvpn-server@server-tcp --no-pager
systemctl status openvpn-server@server-udp --no-pager
```

Cek service utama:

```bash
systemctl status xray nginx haproxy ws dropbear cron netfilter-persistent rc-local --no-pager
```

---

## Struktur file penting

```text
setup.sh                  Installer utama Ubuntu 22.04/24.04
media/config.json         Konfigurasi Xray dengan marker akun
media/haproxy.cfg         Konfigurasi HAProxy multiport
media/xray.conf           Konfigurasi Nginx reverse proxy Xray
media/nginx.conf          Konfigurasi Nginx utama
media/openvpn             Installer/repair OpenVPN Ubuntu 24.04
media/dropbear.conf       Konfigurasi Dropbear
media/sshd                Konfigurasi OpenSSH
media/tun.conf            Konfigurasi WS-ePro backend SSH/OpenVPN
menu/             Paket menu terbaru
```

---

## Troubleshooting cepat

Jika `run` masih menampilkan OFF:

```bash
restart
run
```

Jika OpenVPN belum ON:

```bash
systemctl daemon-reload
systemctl enable --now openvpn-server@server-tcp openvpn-server@server-udp
journalctl -u openvpn-server@server-tcp -n 50 --no-pager
journalctl -u openvpn-server@server-udp -n 50 --no-pager
```

Jika Iptables OFF:

```bash
apt install -y iptables iptables-persistent netfilter-persistent
systemctl enable --now netfilter-persistent
netfilter-persistent save
netfilter-persistent reload
```

Jika RClocal OFF:

```bash
chmod +x /etc/rc.local
systemctl daemon-reload
systemctl enable --now rc-local
```

Jika Autoreboot OFF:

```bash
systemctl enable --now cron
cat /etc/cron.d/daily_reboot
```
