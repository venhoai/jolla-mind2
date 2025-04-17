# Manage your environment with Home Manager

Home Manager is a [Nix](install.md)-powered tool for reproducible management of the contents of users’ home directories. This includes programs, configuration files, environment variables, and systemd user service management.


## Prerequisites

Home Manager is a part of the Nix ecosystem. If you haven't installed Nix yet, follow the [installation guide](install.md) first.


## Install Home Manager

Once you have Nix installed, you can install Home Manager by running the following commands:

```console
nix-channel --add https://github.com/nix-community/home-manager/archive/master.tar.gz home-manager
nix-channel --update
nix-shell '<home-manager>' -A install
```


## First-time activation

After installation, you should be able to use Home Manager with:

```console
. $HOME/.nix-profile/etc/profile.d/hm-session-vars.sh
home-manager switch
```

Usually Home Manager should be activated automatically when you start a new shell, thanks to `.profile` created during the [Nix installation](install.md).


## First-time configuration

Your Home Manager configuration is managed with updating `/home/defaultuser/.config/home-manager/home.nix` and running `home-manager switch` to apply the changes.

Home Manager installs with example `home.nix` with a lot of examples. To see the forest for the trees, it might be good to put that aside and start with a minimal configuration with:

```console
mv /home/defaultuser/.config/home-manager/home.nix /home/defaultuser/.config/home-manager/home.nix.example
```

and

```console
cat << EOF > /home/defaultuser/.config/home-manager/home.nix
{ config, pkgs, lib, ... }:
{
  home.username = "defaultuser";
  home.homeDirectory = "/home/defaultuser";
  home.stateVersion = "24.11";
  home.packages = [
    pkgs.nano
  ];
  programs.home-manager.enable = true;
  programs.bash.enable = true;
  home.file.".profile".source = lib.mkForce (builtins.toFile "profile" ''
    if [ -e "$HOME/.nix-profile/etc/profile.d/nix.sh" ]; then
        . "$HOME/.nix-profile/etc/profile.d/nix.sh"
        exec bash
    fi
  '');
}
EOF
```

with `home-manager switch -b backup` to apply the changes.

This configuration installs `nano` as a minimal editor and enables full `bash` and updates `.profile` that activates Nix on every new shell with Home Manager managed version. (The previous version of `.profile` is backed up as `.profile.backup`.)

After this first-time configuration, you can start adding more packages and configurations to your `home.nix` and using `home-manager switch` to apply the changes.


## Manage Nix configuration

Nix has a few [configuration options](https://nix.dev/manual/nix/2.24/command-ref/conf-file.html?highlight=min-free#available-settings) that can be set in `~/.config/nix/nix.conf`. Home Manager can manage these options with `nix.extraOptions` in `home.nix`.

You should at least configure automatic garbage collection with `min-free` and `max-free` options. For example:

```nix
  nix.package = pkgs.nix;
  nix.extraOptions = ''
    min-free = 1G
    max-free = 10G
  '';
```

Possibly, later you might want to enable experimental features like `nix-command` and `flakes` for [the next-generation Nix command-line interface](https://nix.dev/manual/nix/2.24/command-ref/experimental-commands):

```nix
  nix.package = pkgs.nix;
  nix.extraOptions = ''
    min-free = 1G
    max-free = 10G
    experimental-features = nix-command flakes
  '';
```


## Additional resources

* [Home Manager manual](https://nix-community.github.io/home-manager/index.xhtml)
* [All Home Manager options](https://nix-community.github.io/home-manager/options.xhtml)
