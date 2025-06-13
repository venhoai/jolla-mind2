>[!NOTE]
>
>Follow the [netbird docs](https://docs.netbird.io/how-to/getting-started) to setup/create an Account, if you don't already have one.

# Set up netbird for remote access on Jolla Mind 2
## Open the Terminal on the Mind 2 or ssh into it
```/bin/sh
    devel-su
```
to switch to root
## Run this one liner to install the netbird client
```/bin/sh
    curl -fsSL https://pkgs.netbird.io/install.sh | sh
```
## Activate the client
``` /bin/sh
    netbird up
```
to start the netbird client a URL will be shown in the Terminal/SSH session , looking like this `https://login.netbird.io/activate?user_code=XXXX-XXXX`open it in the Webbrowser and activate the Device.

## That's it
Now you can access the Mind 2 via `[HOSTNAME].netbird.cloud on any device running the netbird client.
