# Setup

The system and tools required by the course. The initial setup is performed in [lab 1](./LAB.md).

## Operating system

A [UNIX-like](https://en.wikipedia.org/wiki/Unix-like) system is required: Linux, macOS, or Windows through WSL 2.

Pick your path:

- **Linux**: done, keep the system up to date.
- **macOS**: UNIX under the hood. Install the Xcode Command Line Tools (`xcode-select --install`) and [Homebrew](https://brew.sh/).
- **Windows**: install [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install) with `wsl --install` - a real Ubuntu inside Windows. Two rules:
  1. Work inside the Linux filesystem (`~/`), never in `/mnt/c/...`.
  2. Connect the editor to WSL: with VS Code, the [WSL extension](https://code.visualstudio.com/docs/remote/wsl) and `code .` from the Ubuntu terminal.

> [!WARNING]
> Git Bash is a minimal bash emulation bundled with Git for Windows: fine for a few `git` commands, not a dev environment (no package manager, no real UNIX filesystem). Same for PowerShell. Use WSL.

Alternatives to WSL 2 for Windows users:

- **Dual-boot**: native performance, the full Linux experience, but a heavier setup.
- **Virtual machine**: a way to try Linux without touching the host system, slower and more resource-hungry.

For a dual-boot or a VM, pick a [Linux distribution](https://en.wikipedia.org/wiki/List_of_Linux_distributions):

- [Ubuntu](https://ubuntu.com/), easy (recommended)
- [Pop!_OS](https://pop.system76.com/), easy
- [Fedora](https://getfedora.org/), medium
- [Arch Linux](https://archlinux.org/), hard
- [NixOS](https://nixos.org/), hard

> [!NOTE]
> Teacher will use Arch Linux with GNOME desktop environment.

## Git

[Git](https://git-scm.com/) is the version control system of the course, used from the command line. Don't forget to configure your username and email.

## Code editor

Any code editor works, some are more beginner-friendly than others:

- [Visual Studio Code](https://code.visualstudio.com/), easy (recommended)
- [Zed](https://zed.dev/), easy
- [Vim](https://www.vim.org/)/[Neovim](https://neovim.io/), hard

It is also recommended to install a lightweight text editor for quick edits (like [Sublime Text](https://www.sublimetext.com/)).

> [!NOTE]
> Teacher will use Visual Studio Code.

## Browser

At least two browsers, to test on different engines:

- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Chrome](https://www.google.com/chrome/)

> [!NOTE]
> Teacher will use Firefox.

## Node.js

[Node.js](https://nodejs.org/) runs JavaScript outside of the browser. Install it through a version manager - never the installer or the system package manager: versions must be switchable per project, live in the home directory, and require no `sudo`.

> [!NOTE]
> A "system Node.js" may already exist (`which node`): distributions ship an old version for their own tooling. Leave it alone - the version manager's Node takes precedence in the `PATH`.

- [fnm](https://github.com/Schniz/fnm) (recommended): fast, switches automatically per project (`.nvmrc` / `.node-version` files), featured on the [official download page](https://nodejs.org/en/download)
- [nvm](https://github.com/nvm-sh/nvm): the historic choice, same workflow, slower
- [mise](https://mise.jdx.dev/): polyglot version manager (Node, Python, ...), for those managing several languages

> [!NOTE]
> Teacher will use fnm.

NPM, the default package manager, comes with Node.js. Alternatives are [pnpm](https://pnpm.io/) (faster, disk-efficient) and [Yarn](https://yarnpkg.com/).

> [!NOTE]
> Teacher will use pnpm.

## Password manager

A password manager stores credentials in an encrypted vault behind a master password, and protects against phishing and password reuse. Popular open-source solutions:

- [Bitwarden](https://bitwarden.com/)
- [KeePass](https://keepass.info/)
- [pass](https://www.passwordstore.org/)
