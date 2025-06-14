# alplix-yenten (Yenten Full Node)

This is a clean and minimal version of the Yenten full node, adapted and maintained by [Alplix](https://coff.ee/alplix).  
It is hosted at: [https://github.com/alplix/alp-yenten](https://github.com/alplix/alp-yenten)

---

## ✅ Features

- No wallet or GUI – lightweight node only
- IPv4, IPv6, and Tor support
- Custom peer identifier: `/alplix-yenten:(coff.ee/alplix)/`
- Cross-platform support for Linux, BSD, macOS, Gentoo, and more

---

## 📦 Supported Platforms

- Debian / Ubuntu / Linux Mint
- Arch / Manjaro
- Gentoo
- macOS (Homebrew)
- FreeBSD / OpenBSD

---

## 📁 Step-by-Step Installation

### 1. Clone the Repository

```bash
git clone https://github.com/alplix/alp-yenten.git
cd alp-yenten
```

### 2. Install Required Packages

#### Debian / Ubuntu:
```bash
sudo apt update
sudo apt install build-essential libtool autotools-dev automake pkg-config \
  libssl-dev libevent-dev bsdmainutils git curl libboost-all-dev
```

#### Arch Linux / Manjaro:
```bash
sudo pacman -Syu base-devel git boost openssl libevent
```

#### Gentoo:
```bash
sudo emerge --ask sys-devel/gcc sys-devel/make sys-devel/automake \
  dev-libs/boost dev-libs/openssl dev-libs/libevent
```

#### macOS (Homebrew):
```bash
brew install automake autoconf libtool pkg-config boost openssl@3 libevent

# Configure paths for OpenSSL:
export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
```

#### FreeBSD / OpenBSD:
```bash
pkg install git gmake boost-libs openssl libevent autoconf automake libtool
```

---

## 🛠️ Build the Node

```bash
./autogen.sh
./configure --disable-wallet --without-gui --disable-tests
make -j$(nproc)
```

> ⚠️ On BSD systems, use `gmake` instead of `make`.

---

## 🚀 Run the Node

```bash
./src/yentend -printtoconsole
```

Use a minimal configuration file at `~/.yenten/yenten.conf` for persistent settings.

---

## 🧅 Enable Tor (Optional)

```bash
sudo apt install tor
```

Example `yenten.conf`:

```
onlynet=onion
proxy=127.0.0.1:9050
listen=1
discover=1
bind=127.0.0.1
```

---

## 🔄 Auto-Start on Boot (systemd)

Create `/etc/systemd/system/yenten.service`:

```ini
[Unit]
Description=Yenten Full Node (alplix-yenten)
After=network.target

[Service]
ExecStart=/home/youruser/alp-yenten/src/yentend -conf=/home/youruser/.yenten/yenten.conf
User=youruser
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl daemon-reexec
sudo systemctl enable yenten
sudo systemctl start yenten
```

---

## 📌 Peer Identifier

This node will appear in the peer list as:

```
/alplix-yenten:(coff.ee/alplix)/
```

---

## 🔗 Links

- Project page: [https://github.com/alplix/alp-yenten](https://github.com/alplix/alp-yenten)
- Maintainer: [https://coff.ee/alplix](https://coff.ee/alplix)
- More projects: [https://github.com/alplix](https://github.com/alplix)

---

This repository is part of the decentralized node infrastructure maintained by **Alplix**.
