# alp-yenten (Yenten Full Node)

This is a clean and minimal version of the Yenten full node, adapted and maintained by [Alplix](https://coff.ee/alplix).  
Hosted at: [https://github.com/alplix/alp-yenten](https://github.com/alplix/alp-yenten)

---

## ✅ Features

- Headless full node (no wallet, no GUI)
- Works over IPv4, IPv6, and Tor
- Custom node identifier: `/alplix-yenten:(coff.ee/alplix)/`
- Auto-start for both Tor and Yenten node on system boot
- Fully documented for Linux, macOS, BSD, Gentoo

---

## 📦 Supported Systems

- Debian, Ubuntu, Mint
- Arch, Manjaro
- Gentoo
- macOS (via Homebrew)
- FreeBSD, OpenBSD

---

## 📁 Step-by-Step Installation

### 1. Clone the Repository

```bash
git clone https://github.com/alplix/alp-yenten.git
cd alp-yenten
```

---

### 2. Install Dependencies

#### Debian / Ubuntu:

```bash
sudo apt update
sudo apt install build-essential libtool autotools-dev automake pkg-config \
  libssl-dev libevent-dev bsdmainutils git curl libboost-all-dev tor
```

#### Arch Linux / Manjaro:

```bash
sudo pacman -Syu base-devel git boost openssl libevent tor
```

#### Gentoo:

```bash
sudo emerge --ask sys-devel/gcc sys-devel/make sys-devel/automake \
  dev-libs/boost dev-libs/openssl dev-libs/libevent net-vpn/tor
```

#### macOS:

```bash
brew install automake autoconf libtool pkg-config boost openssl@3 libevent tor
```

#### BSD:

```bash
pkg install git gmake boost-libs openssl libevent autoconf automake libtool tor
```

---

## 🔨 Build Instructions

```bash
./autogen.sh
./configure --disable-wallet --without-gui --disable-tests
make -j$(nproc)
```

---

## 🌐 Full Network Configuration (IPv4 + IPv6 + Tor)

Edit `~/.yenten/yenten.conf` (create file if it doesn’t exist):

```ini
listen=1
discover=1
onlynet=ipv4
onlynet=ipv6
onlynet=onion
bind=0.0.0.0
bind=[::]
proxy=127.0.0.1:9050
listenonion=1
```

---

## 🧅 Enable and Auto-Start Tor (REQUIRED)

To activate Tor support and ensure it starts before your node:

```bash
sudo systemctl enable tor
sudo systemctl start tor
```

---

## 🖥️ Auto-Start Node at Boot (with Tor)

1. Create a new systemd service file:

```bash
sudo nano /etc/systemd/system/yenten.service
```

2. Paste this configuration:

```ini
[Unit]
Description=Yenten Full Node (alplix-yenten)
After=network.target tor.service
Requires=tor.service

[Service]
ExecStart=/home/youruser/alp-yenten/src/yentend -conf=/home/youruser/.yenten/yenten.conf
User=youruser
Restart=always

[Install]
WantedBy=multi-user.target
```

> Replace `/home/youruser` with your actual home directory path and username.

3. Enable and start the service:

```bash
sudo systemctl daemon-reexec
sudo systemctl enable yenten
sudo systemctl start yenten
```

Your node will now **always start after Tor** on boot, with access to all networks (clearnet + onion).

---

## 📡 Peer Identity

Your node will appear to other peers as:

```
/alplix-yenten:(coff.ee/alplix)/
```

---

## 🧪 Check Connectivity

```bash
netstat -tunlp | grep yentend
```

You should see:

- 0.0.0.0:9333 (IPv4)
- :::9333 (IPv6)
- `.onion` in peer logs (Tor)

---

## 🔗 Links

- Project: [github.com/alplix/alp-yenten](https://github.com/alplix/alp-yenten)
- Maintainer: [coff.ee/alplix](https://coff.ee/alplix)
- More projects: [github.com/alplix](https://github.com/alplix)

---

This node is part of the decentralized network infrastructure operated by **Alplix**.
