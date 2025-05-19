# Activating Tailscale

[Tailscale](https://tailscale.com/) is a VPN service that allows you to connect to your devices securely. It is a good fit for M2, which is a headless device that you may want to access remotely, and also works when run in user space (as `defaultuser`).


## Prerequisites

Before you start, you need to have Nix installed and Home Manager activated. If you haven't installed Nix yet, follow the [installation guide](install.md) first and activate Home Manager as described in the [Home Manager guide](home-manager.md).


## Register machine

To use Tailscale, you need to register your machine for your Tailnet.

Because `tailscale` command does not support custom socket path, the registration must be done with the default socket path.

This means that you enter super user mode

```console
devel-su
```

and create the default socket path before continuing with the registration.

```console
mkdir -p /var/run/tailscale
chown defaultuser:defaultuser /var/run/tailscale/
```

Then you can return to `defaultuser` mode.

```console
exit
```

Now you may manually start the tailscale daemon with user space networking, but with the default socket path

```console
nix-shell -p tailscale --run "tailscaled --tun=userspace-networking"
```

and then run the registration in another shell

```console
nix-shell -p tailscale --run "tailscale up"
```

and follow the instructions.

This should already bring your M2 into your Tailnet, until you break the daemon with `Ctrl+C`.


## Register service

To make sure that Tailscale starts automatically when you start your M2, you can create a systemd user service for it in your `home.nix` configuration:

```nix
  systemd.user.services.tailscaled = {
    Unit = {
      Description = "Tailscale client daemon";
      After = [ "network.target" ];
    };
    Service = {
      ExecStart = "${pkgs.tailscale}/bin/tailscaled --tun=userspace-networking --socket ${config.home.homeDirectory}/.local/share/tailscale/tailscaled.sock";
      Restart = "always";
      RestartSec = 5;
    };
    Install = {
      WantedBy = [ "default.target" ] ;
    };
  };
  systemd.user.startServices = true;
```

When ready, run `home-manager switch` to apply the changes and `systemctl --user start tailscaled` to start the service for the first time. Later it will be automatically started e.g. after a system reboot.

This will start the Tailscale daemon with user space networking and the custom socket path when you start your M2.
