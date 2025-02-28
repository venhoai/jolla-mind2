# Nix Package Manager

[Nix](https://nixos.org/nix/) is a package manager that manages software and their dependencies independently from the underlying OS. With Nix, all software and their dependencies are contained in a single dedicated file system location. This makes Nix a good fit when hacking M2, which ships with only minimal command-line tools available by default.

With Nix installed, you may not need to install additional packages into the OS. You also may not need to use `devel-su` or `nerdctl` for most of the command-line tools or libraries you might need.

* [Installation](install.md)
* [Home Manager](home-manager.md)
* [Tailscale](tailscale.md)
