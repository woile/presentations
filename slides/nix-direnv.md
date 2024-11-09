---
title: Nix & direnv
theme: themes/rusty.css
highlightTheme: vs
---

# Nix & direnv

---

## Installation

- Install `nix` (on mac use [determinate installer](https://github.com/DeterminateSystems/nix-installer))
- Install `direnv` and [hook into your shell](https://direnv.net/docs/hook.html)

```sh
nix profile install direnv
```

- Add nix cache to avoid building everything <!-- .element: class="fragment" data-fragment-index="0" -->

---

## What is Nix?

- package manager
- language
- operating system

----

## What can you do with Nix?

```sh [1-8|1,5]
packages manager
build applications
build docker containers
create virtual machines
create development environments
build ISO
control fleet of servers
templates
```

---

## As a package manager

Replace `apt`, `brew`, `pipx`, `apk`, `cargo install`, etc.

----

### Install package

```sh
nix profile install $PACKAGE
```

### Remove package

```sh
nix profile remove $PACKAGE
```

### Upgrade packages

```sh
nix profile upgrade
```

----

### Repository

[search.nixos.org/packages](https://search.nixos.org/packages)

---

## What is direnv?

> load and unload stuff depending on the current directory

---

## As a development environment

----

```sh
echo "use flake . --impure" > .envrc
touch flake.nix
git add -N flake.nix .envrc # promise we will add them
```

----

#### flake.nix

```nix
{
  description = "A development shell";
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
  };
  outputs = inputs@{ flake-parts, ... }:
  # https://flake.parts/
  flake-parts.lib.mkFlake { inherit inputs; } {
    systems = [ "x86_64-linux" "aarch64-darwin" "x86_64-darwin" ];
    perSystem = { pkgs, ... }: {
      devShells.default = pkgs.mkShell {
        name = "dev";
        # https://search.nixos.org/packages
        buildInputs = [ pkgs.just ];
        shellHook = ''echo "Welcome to the devshell!"'';
      };
    };
  };
}
```

----

```sh
nix flake init -t github:woile/nix-config#devshell
```

----

```sh
direnv allow
```

---

## Upsides

- reproducible environment everywhere
- no need to install anything globally
- easy cache on github actions

---

<!-- .slide: data-auto-animate -->

## Downsides

- slow if binaries are missing or wrongly configured
- hard to debug

---

<!-- .slide: data-auto-animate -->

## Downsides

- slow if binaries are missing or wrongly configured
- very hard to debug

---

<!-- .slide: data-auto-animate -->

## Downsides

- slow if binaries are missing or wrongly configured
- very very hard to debug

---

## Thank you!

![qr code](./assets/qr-code.png)

![mastodon logo](./assets/mastodon.png) <!-- .element:  width="40px" style="margin: 0" --> [@woile](https://hachyderm.io/@woile)
&nbsp;&nbsp;&nbsp;
![github logo](./assets/github.png) <!-- .element:  width="50px" style="margin: 0" --> [woile](https://github.com/woile)

santiwilly@gmail.com