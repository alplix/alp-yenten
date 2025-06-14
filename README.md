# 🛠️ Cross-Platform Build & Install Instructions for alplix-yenten

This document provides step-by-step instructions for building and running the Yenten full node (`alplix-yenten`) across major UNIX-like systems, including Debian-based Linux, Arch, Gentoo, macOS, BSD, and more.

---

## 📦 REQUIRED DEPENDENCIES (ALL SYSTEMS)

- gcc / clang
- g++
- make / autotools
- pkg-config
- OpenSSL
- Boost
- libevent
- Berkeley DB (optional, for wallet support)

---

## 🔧 SYSTEM-SPECIFIC INSTRUCTIONS

### 🐧 Debian / Ubuntu / Linux Mint

```bash
sudo apt update
sudo apt install build-essential libtool autotools-dev automake pkg-config   libssl-dev libevent-dev bsdmainutils git curl libboost-all-dev
```

### 🐧 Arch Linux / Manjaro

```bash
sudo pacman -Syu base-devel git boost openssl libevent
```

### 🐧 Gentoo Linux

```bash
sudo emerge --ask sys-devel/gcc sys-devel/make sys-devel/automake   sys-devel/autoconf dev-libs/boost dev-libs/openssl dev-libs/libevent
```

> You may need to enable the correct `USE` flags for Boost and OpenSSL.

### 🍎 macOS (with Homebrew)

```bash
brew install automake autoconf libtool pkg-config boost openssl@3 libevent
```

> Ensure OpenSSL path is available to the build system:
```bash
export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
```

### 🐚 FreeBSD / OpenBSD

```bash
pkg install git gmake boost-libs openssl libevent autoconf automake libtool
```

---

## 🔨 BUILD

```bash
./autogen.sh
./configure --disable-wallet --without-gui --disable-tests
make -j$(nproc)
```

> On BSD systems, replace `make` with `gmake`.

---

## 🚀 RUNNING THE NODE

```bash
./src/yentend -printtoconsole
```

You can create a minimal `yenten.conf` in `~/.yenten/` to configure ports, Tor, peers, etc.

---

## 🔄 SYSTEMD AUTO-START (Linux)

Create a file: `/etc/systemd/system/yenten.service`

```ini
[Unit]
Description=Yenten Full Node (alplix-yenten)
After=network.target

[Service]
ExecStart=/path/to/src/yentend -conf=/path/to/yenten.conf
User=youruser
Restart=always

[Install]
WantedBy=multi-user.target
```

Then:

```bash
sudo systemctl daemon-reexec
sudo systemctl enable yenten
sudo systemctl start yenten
```

---

## 🧅 TOR SUPPORT (Optional)

Install and start Tor:

```bash
sudo apt install tor
```

In `yenten.conf`:

```
onlynet=onion
proxy=127.0.0.1:9050
listen=1
discover=1
bind=127.0.0.1
```

---

## 📁 DATA DIRECTORY

Default directory: `~/.yenten`

To use a custom data directory:
```bash
./src/yentend -datadir=/your/custom/path
```

---

## 📌 TROUBLESHOOTING

- Missing Boost: Ensure development headers are installed
- OpenSSL not found: Set `CPPFLAGS` and `LDFLAGS`
- `cannot find -lboost_*`: Use static linking or install full Boost suite

---

Maintained by [Alplix](https://coff.ee/alplix) | [GitHub](https://github.com/alplix)
