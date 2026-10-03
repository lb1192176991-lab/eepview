<!-- TODO: centered logo. Add it from assets/brand/ once the brand PR has merged. -->

<h1 align="center">eepview</h1>

<p align="center">A small browser that opens I2P sites and nothing else.</p>

<p align="center">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-blue"></a>
  <a href="https://scorecard.dev/viewer/?uri=github.com/tcivie/eepview"><img alt="OpenSSF Scorecard" src="https://api.scorecard.dev/projects/github.com/tcivie/eepview/badge"></a>
  <a href="https://deps.rs/repo/github/tcivie/eepview?path=src-tauri"><img alt="dependency status" src="https://deps.rs/repo/github/tcivie/eepview/status.svg?path=src-tauri"></a>
</p>

## What it is

eepview is a browser for I2P sites (`*.i2p`). It runs on the web view of your operating system, so it is small. The design goal: no clearnet traffic from the page view, because every page load goes through the I2P HTTP proxy. This is not built yet. See Status.

## Security model

Built (see [ADR 0001](docs/wiki/adr-0001-no-leak-architecture.md)):

| Path | Guard |
|---|---|
| Page loads | A web tab exists only after the I2P proxy passes a check. Every load goes through eepview's own gatekeeper proxy, which forwards only `.i2p` requests. |
| Outproxy | Never used. The gatekeeper forwards only `.i2p` hosts, so a router outproxy cannot carry a page request. eepview itself never asks for a clearnet host. |
| Loopback | Blocked. The gatekeeper refuses it, a page policy and an engine rule list block it inside the page. A page cannot reach local services. |
| Navigation | Only `http(s)://*.i2p` and `*.b32.i2p` can load. |
| WebRTC | Off. WebRTC sends UDP outside the proxy, so no engine can keep it inside I2P. |
| JavaScript | On by default. Pages reach only .i2p hosts: the five guards below JavaScript block everything else. You can turn JS off per site. |

Design target, not built yet:

| Path | Planned guard |
|---|---|
| Outproxy | The managed router has an empty outproxy list, checked at every start. |
| Consent | No network action starts before you agree to it. The installer and the Java update are the only clearnet paths. They run only after a click, to fixed hosts. |

## Features

- Tabs
- Bookmarks
- History
- Find in page
- Router stats (in progress)
- Light and dark theme (in progress)

## Screenshots

<!-- TODO: add real screenshots here when the UI is finished. -->

## Status

Pre-release. Phase 1 (app shell) is in progress. Do not use eepview for anything that needs anonymity yet.

## Build from source

Requirements:

- Rust 1.96 (`rust-toolchain.toml` pins it)
- Node.js 22 or later, and npm
- macOS 14 or later, Windows 10 or later with WebView2, or Linux with WebKitGTK 4.1

```sh
npm ci
npm run tauri dev
```

## Supply-chain checks

CI enforces these checks on every pull request:

- clippy with the pedantic lint group
- complexity limits
- biome
- cargo-deny (bans and sources)
- gitleaks
- zizmor
- actionlint
- Socket.dev

## Going public

CodeQL and Scorecard start when the repo becomes public. See [Going public](https://github.com/tcivie/eepview/wiki/going-public).

## Documentation

All docs live in the [wiki](https://github.com/tcivie/eepview/wiki).

## Credits

eepview uses the [I2P](https://geti2p.net) network. It is not affiliated with the I2P project.

## License

[MIT](LICENSE)
