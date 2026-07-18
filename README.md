```
██████╗ ███████╗██╗   ██╗███████╗ █████╗ ██╗
██╔══██╗██╔════╝██║   ██║██╔════╝██╔══██╗██║
██████╔╝█████╗  ██║   ██║█████╗  ███████║██║
██╔══██╗██╔══╝  ╚██╗ ██╔╝██╔══╝  ██╔══██║██║
██║  ██║███████╗ ╚████╔╝ ███████╗██║  ██║███████╗
╚═╝  ╚═╝╚══════╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝╚══════╝
```

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![zshrs plugin](https://img.shields.io/badge/zshrs-native%20plugin-blue.svg)](https://github.com/MenkeTechnologies/zshrs)

### `[OPEN THE REPO IN A BROWSER — COMPILED]`

> *"reveal — one word, and the repo's GitHub page is open."*

## `[NATIVE ZSHRS PLUGIN]`

[gh_reveal](https://github.com/MenkeTechnologies/gh_reveal) — `reveal` opens the current git repository's GitHub page (and any Heroku app pages) in your browser — ported to a **native [zshrs](https://github.com/MenkeTechnologies/zshrs) plugin**. A faithful reimplementation in Rust: the platform opener detection, the `git remote -v` parsing, the SSH/HTTPS URL normalization, and the Heroku dashboard handling are all reproduced.

### [`zshrs`](https://github.com/MenkeTechnologies/zshrs) &middot; [`znative`](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZNATIVE.md) &middot; [`upstream`](https://github.com/MenkeTechnologies/gh_reveal)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Usage](#0x02-usage)
- [\[0x03\] How it works](#0x03-how-it-works)
- [\[0xFF\] License](#0xff-license)

---

## [0x00] OVERVIEW

```text
reveal              → open this repo's GitHub page(s)
reveal upstream     → open only the remote(s) matching "upstream"
reveal ../other     → open the repo(s) in the given directory
reveal              → (outside any repo) open your GitHub repositories page
```

---

## [0x01] INSTALL

```sh
znative load MenkeTechnologies/zshrs-reveal
```

Put that one line in your `.zshrc`. [znative](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZNATIVE.md), zshrs's package manager, installs the plugin on the first shell start — clones it, runs `cargo build --release`, and `zmodload -R`s the resulting `libreveal` — then loads it from the store, zero-network, on every start after. Then `reveal` opens the current repo.

### Manual build

```sh
cargo build --release
zmodload -R ./target/release/libreveal.dylib   # .so on Linux
reveal
```

---

## [0x02] USAGE

Run `reveal` inside a git repository to open its GitHub page. Arguments filter the remotes by substring (`reveal upstream` opens only the `upstream` remote). Run it in a non-repo directory to open your GitHub repositories page, or pass directory arguments to reveal the repos they contain. Set `$GITHUB_ACCOUNT` to override the account used for the no-repo page (otherwise `git config user.name`).

---

## [0x03] HOW IT WORKS

`reveal` picks the platform opener (`open` on macOS, `xdg-open` on Linux, the Windows shell under WSL), then decides by context: outside a repo with no arguments it opens `github.com/<account>?tab=repositories`; outside a repo with directory arguments it reveals each; inside a repo it reads `git remote -v`, keeps the fetch lines (filtered by any arguments), normalizes each SSH/HTTPS URL to `host/user/repo` (dropping scheme, userinfo, port, and `.git`), and opens `https://<that>` — Heroku remotes instead open their dashboard and `herokuapp.com` URLs. Identical to gh_reveal.

---

## [0xFF] LICENSE

MIT. Ported from [MenkeTechnologies/gh_reveal](https://github.com/MenkeTechnologies/gh_reveal) (MIT). See [LICENSE](LICENSE).
