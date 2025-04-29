# Setting up Protonmail Bridge on the Jolla Mind2

This guide will walk you through setting up Protonmail Bridge on the Jolla Mind2. Specifically, we will use the [shenxn/protonmail-bridge-docker](https://github.com/shenxn/protonmail-bridge-docker) image and running it with nerdctl / nerdctl compose, similar to how Venho.AI's environment runs on top of the Mind2.

This ensures we can more easily update the software, isolate its operations to the container, and if desired lock it down to limit networking. While the later is possible, it is not _currently_ covered in this guide and it will default to using the bridge network, therefore similar to the Venho UI your bridge **will be exposed to the network**. Be smart and be safe.

This guide will assume you have enabled developer mode on your Mind2. If you have not, see [this](https://docs.sailfishos.org/Support/Help_Articles/Enabling_Developer_Mode/) Sailfish OS guide. If you are accessing it remotely, be sure you have enabled remote connection and set a secure password for device access. Developer mode at the very least is required during setup, remote connection is optional (if you are not using the Terminal from the Mind2) and can be safely turned off after this setup.

## Preparation

We will be stuff any configuration into a dedicated directory for our service, so let's set that up now.

```
devel-su
mkdir -p /var/lib/venho-user/protonmail-bridge
```

## Files

### Compose file

Our `compose.yml` file is responsible for knowing what image to pull from an image registry (default is Docker Hub), which ports to expose, default volumes to create, and more. This `compose.yml` is used by `nerdctl` which in turn is used by our `systemd` service unit.

The source `docker-compose.yml` can be found [here](https://github.com/shenxn/protonmail-bridge-docker/blob/master/docker-compose.yml). However, we will need to tweak ours to pull down ARM builds (which are available with the `build` tag)

Pull this file down via the following command:

```
curl https://raw.githubusercontent.com/venhoai/jolla-mind2/refs/heads/main/docs/protonmail-bridge/compose.yml -o /var/lib/venho-user/protonmail-bridge/compose.yml
```

### Env

We will now pull down a basic env file that will be used to indicate where our configuration and compose file are.

```
curl https://raw.githubusercontent.com/venhoai/jolla-mind2/refs/heads/main/docs/protonmail-bridge/00-default.env -o /var/lib/venho-user/protonmail-bridge/00-default.env
```

### systemd Service Unit

**Important:** Do **not** enable this systemd unit until the end of the guide (we have a separate section on it).

```
curl https://raw.githubusercontent.com/venhoai/jolla-mind2/refs/heads/main/docs/protonmail-bridge/protonmail-bridge.service -o /etc/systemd/system/protonmail-bridge.service
```

## First Run

While still in `devel-su`, run the following to do a first run of Protonmail Bridge. Per the [upstream docs](https://github.com/shenxn/protonmail-bridge-docker?tab=readme-ov-file#initialization):

> Wait for the bridge to startup, then you will see a prompt appear for [Proton Mail Bridge interactive shell](https://proton.me/support/bridge-cli-guide). Use the `login` command and follow the instructions to add your account into the bridge. Then use `info` to see the configuration information (username and password). After that, use `exit` to exit the bridge. You may need `CTRL+C` to exit the docker entirely.

Furthermore, you may want to let it fully sync before stopping the container, just to be safe.

```
nerdctl compose --env-file /var/lib/venho-user/protonmail-bridge/00-default.env --file /var/lib/venho-user/protonmail-bridge/compose.yml run protonmail-bridge init
```

Once the init is done, type `info` and copy the settings somewhere (you'll need these for adding it to Venho)

## Enable service startup

Now, let's enable and start the systemd service:

```
systemctl enable --now protonmail-bridge.service
```

You can validate the container is running successfully with: `systemctl status protonmail-bridge`

## Exiting devel-su

Type `exit` to leave su mode.

## Adding to Venho

Go to the Messaging Center and add an account.

- For E-Mail Address, Username and Password (both IMAP and SMTP) put the `username` defined from the `info` of ProtonMail Bridge.
- For Hostname (both IMAP and SMTP): `localhost.localdomain`
- IMAP Port: `1143`
- Password (both IMAP and SMTP): `password` defined from `info` of ProtonMail Bridge
- SMTP Port: `1025`
