<div align="center">
  <h1>BITS HiLink</h1>
  <p>
    <a href="https://bits.co.id">
      <img src="https://img.shields.io/badge/Banten%20IT%20Solutions-BITS%20HiLink-00C853?style=for-the-badge&logoColor=white" alt="BITS HiLink" />
    </a>
  </p>
  <p>
    Drop-in LuCI app for Huawei HiLink modem on OpenWrt &mdash; monitor signal, SMS, and data usage from the web interface with a modern BITS theme.
  </p>
  <br>
  <p>
    <img src="https://img.shields.io/badge/OpenWrt-00A1E9?style=flat&logo=openwrt&logoColor=white" alt="OpenWrt" />
    <img src="https://img.shields.io/badge/LuCI-3D5780?style=flat" alt="LuCI" />
    <img src="https://img.shields.io/badge/Huawei-FF0000?style=flat&logo=huawei&logoColor=white" alt="Huawei" />
    <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black" alt="JavaScript" />
    <img src="https://img.shields.io/badge/license-MIT-green?style=flat" alt="MIT License" />
  </p>
</div>

---

## ✨ Features

| Feature               | Description                                                                                              |
| --------------------- | -------------------------------------------------------------------------------------------------------- |
| **Modem Dashboard**   | Real-time signal quality (RSSI/RSRP/SINR/RSRQ), network band, SIM status, dan WAN IP.                    |
| **Cell Info**         | PCI, EARFCN, frequency, bandwidth, LAC/Cell ID (Hex/Dec) untuk 4G &mdash; render pakai `poll` 3 detik.   |
| **SMS Inbox**         | Baca, tandai dibaca, dan hapus SMS masuk langsung dari LuCI.                                             |
| **Data Traffic**      | Statistik sesi + total usage + throughput real-time.                                                     |
| **Configuration**     | Set IP / user / pass modem via UCI (`/etc/config/hilink`).                                               |
| **Dark Mode**         | Semua view responsif terhadap `body.dark` bawaan LuCI theme.                                             |
| **Precompiled CGI**   | Binary `hilink_api` (statically linked) &mdash; tanpa runtime dep tambahan.                              |
| **Automated Release** | semantic-release builds `.ipk` + `.apk` dan publish GitHub Release di tiap conventional commit.          |

## 🛠️ Tech Stack

| Layer        | Technology                                                      |
| ------------ | --------------------------------------------------------------- |
| **Runtime**  | OpenWrt (LuCI)                                                  |
| **Backend**  | `rpcd` ACL, CGI binary `hilink_api`                             |
| **Language** | JavaScript (LuCI AMD views + polling `require poll`)            |
| **Theme**    | Inline CSS + dark mode (`body.dark`)                            |
| **Build**    | `bash` + `tar` (ipk) + `apk-tools v3` (apk) &mdash; no SDK      |
| **Release**  | semantic-release + GitHub Actions                               |

---

## 📁 Project Structure

```text
BITS-HiLink/
├── .github/
│   ├── dependabot.yml             # dep update (npm + actions)
│   └── workflows/
│       └── release.yml            # semantic-release + build .ipk/.apk + attach asset
├── luci-app-bitshilink/
│   ├── htdocs/
│   │   ├── cgi-bin/
│   │   │   └── hilink_api -> /usr/bin/hilink_api   # CGI handler
│   │   └── luci-static/resources/
│   │       ├── icons/hilink-*.png
│   │       └── view/bitshilink/
│   │           ├── details.js     # modem dashboard + signal metrics
│   │           ├── message.js     # SMS inbox
│   │           └── config.js      # ip/user/pass form
│   └── root/
│       ├── etc/
│       │   └── config/hilink
│       └── usr/
│           ├── bin/hilink_api              # precompiled CGI (static ARM aarch64)
│           └── share/
│               ├── luci/menu.d/luci-app-bitshilink.json
│               └── rpcd/acl.d/luci-app-bitshilink.json
├── scripts/
│   └── prepare.js                 # sync version + build .ipk/.apk (semantic-release)
├── build.sh                       # SDK-less .ipk + .apk packer (bash + tar + apk-tools)
├── control                        # ipk metadata
├── postinst                       # reload ACL/menu + pastikan hilink_api executable
├── conffiles                      # jangan timpa /etc/config/hilink saat upgrade
├── package.json                   # semantic-release + plugins
├── package-lock.json              # npm lockfile (npm ci)
├── .releaserc.json                # release plugins (git + github)
└── LICENSE
```

---

## 🚀 Quick Start

### Prerequisites

- OpenWrt device (22.03+), dengan feed `luci` terpasang

### 1. Download

Grab package dari [Releases](https://github.com/Banten-IT-Solutions/BITS-HiLink/releases):
- `.ipk` untuk OpenWrt 22.03–24.10 (`opkg`)
- `.apk` untuk OpenWrt 25.12+ (`apk`)

### 2. Install

```sh
# OpenWrt 22.03–24.10 (opkg)
opkg install luci-app-bitshilink_<version>_all.ipk

# OpenWrt 25.12+ (apk)
apk add luci-app-bitshilink_<version>_all.apk
```

### 3. Use

Buka LuCI (`Services → BITS HiLink`), lalu set IP / user / pass modem di tab **Configuration** (default `192.168.8.1` / `admin` / `admin123`).

---

## 🏗️ Build

SDK-less `.ipk` + `.apk`. Butuh `apk-tools v3` (`apk mkpkg`) di `PATH`. Di CI sudah di-cache; lokal install `apk-tools` 3.x atau set `APK_BIN=<path/to/apk>`.

```sh
./build.sh
# output: dist/luci-app-bitshilink_<version>_all.ipk
#         dist/luci-app-bitshilink_<version>_all.apk
```

---

## 🚀 Release

Release otomatis pakai [semantic-release](https://semantic-release.gitbook.io) + [Conventional Commits](https://www.conventionalcommits.org). Tulis commit conventional:

| Commit                           | Bump       |
| -------------------------------- | ---------- |
| `fix: ...`                       | patch      |
| `feat: ...`                      | minor      |
| `BREAKING CHANGE:` in body       | major      |

Push ke `main` dan workflow build `.ipk` + `.apk` (build.sh + apk-tools) lalu publish ke GitHub Release.

---

## 📄 License

Distributed under the MIT License. See `LICENSE`.

---

<div align="center">
  <strong>BITS HiLink</strong> Developed with ❤️ by <a href="https://bits.co.id"><strong>Banten IT Solutions</strong></a>
</div>