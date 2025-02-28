# How to install Nix on M2

[Nix](https://nixos.org/nix/) has an easy-to-use command line install script, but it relies on the availability of a few specific command line tools (`curl` and `xz`) and the ability to create the `/nix` directory using `sudo`. Therefore, you must take a few extra steps to get started.


## State directory

First, you must create the `/nix` state directory, which will contain everything downloaded and managed by Nix.

But you don't want to take the chance that `/nix` will ever take all free space from the system partition. That's why you create a directory under the home directory instead.

```console
mkdir /home/defaultuser/Nix
```

and only then go to super user

```console
devel-su
```

and bind mount the directory from the home directory into `/nix`.

```console
mkdir /nix
mount --bind /home/defaultuser/Nix /nix
```

At any point, all the space taken by Nix could be released (uninstalled) by just removing the state directory.


## Mount on boot

While the state directory could be bind mounted manually after every boot, it is possible to configure a systemd job to do it for you. Just enter super user mode:

```console
devel-su
```

and create the systemd unit

```console
cat << EOF > /etc/systemd/system/bind-mount-nix.service
[Unit]
Description=Bind mount /home/defaultuser/Nix to /nix
After=local-fs.target
Requires=local-fs.target

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/mount --bind /home/defaultuser/Nix /nix

[Install]
WantedBy=multi-user.target
EOF
```

and enable it.

```console
systemctl daemon-reload
systemctl enable bind-mount-nix.service
systemctl start bind-mount-nix.service
```

Now `/nix` should be (bind) mounted on every boot.


## Bootstrap

Next, you are able to do the required bootstrapping for the final installation. As a super user

```console
devel-su
```

start a minimal container with `/nix` mounted, install the required dependencies `curl` and `xz`, and run the installer for a single user "rootless" Nix installation:

```console
nerdctl run --rm -ti -v /nix:/nix debian:stable
apt-get update
apt-get install curl xz-utils
sh <(curl -L https://nixos.org/nix/install) --no-daemon
```

This initial installation may report some errors, but don't worry. Since it is being run in a container, it can only write to the mounted `/nix` directory.

Once you have exited the container, you may remove the image:

```console
nerdctl rmi debian:stable-slim
```
and change the ownership of the installed files to `defaultuser`.

```console
chown -R defaultuser:defaultuser /nix
```

Remember to exit the super user mode before continuing with the final installation. The rest of the installation should be done as `defaultuser`.


```console
exit
```


## Install

Now you can finally run the installation as `defaultuser`, and going super user is no longer needed for Nix:

```console
/nix/store/*-nix-*/bin/nix shell nixpkgs#curl nixpkgs#xz --extra-experimental-features nix-command --extra-experimental-features flakes
sh <(curl -L https://nixos.org/nix/install) --no-daemon
```

It's good to know that the installation creates the following symlinks:

``` console
/home/defaultuser/.nix-channels/
/home/defaultuser/.nix-defexpr/
/home/defaultuser/.nix-profile/
```


## Activate

Installed Nix must be activated by sourcing its activation script in the shell with

```console
. "$HOME/.nix-profile/etc/profile.d/nix.sh"
```

This can be automated by creating `/home/defaultuser/.profile` (or adding to an existing file) with the activation command:

```console
cat << EOF > /home/defaultuser/.profile
if [ -e "$HOME/.nix-profile/etc/profile.d/nix.sh" ]; then
  . "$HOME/.nix-profile/etc/profile.d/nix.sh"
fi
EOF
```

Now Nix should be activated on every new shell on M2. As a safeguard, the activation is skipped if the bind mount of `/nix` fails for any reason.


## Additional resources

* [Nix manual on package management](https://nix.dev/manual/nix/2.24/package-management/)
* [Home Manager for more than just package management](https://nix-community.github.io/home-manager/index.xhtml#sec-install-standalone)