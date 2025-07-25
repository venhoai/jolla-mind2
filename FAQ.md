# Venho Ai / Jolla Mind2 FAQ

## Introduction

This file is meant to collect useful information for Venho Ai / Jolla Mind2 users to help them troubleshoot and resolve known issues.

>[!IMPORTANT]
> All the commands below are expected to be run with root privileges (`devel-su`)
>

Venho AI system uses containerd [nerdctl](https://github.com/containerd/nerdctl) as its container management tool,
and starting with SailfishOS 5.0.0.68 additional ComposeD to wrap the common operations and updates.

>[!IMPORTANT]
> When ever you need to issue `nerdctl` commands you should first enter the
> ComposeD managed environment with the `venhoadactl shell` command.
>


## Find out what containers are running

To see what service containers are running, use the following command:

```shell
nerdctl compose ps -a
```

## See logs for each container

In order to see the logs nerdctl can be used running the following command:

```shell
# See logs for all containers:
nerdctl compose logs -f

# To see logs for a specific service add the service name listed in the 
# compose ps output, e.g., processor:
nerdctl compose logs -f processor
```

## Start, Stop and restart the Service

Venho Ada service is run by default at system start-up, in order to start stop and restart the service `venhoadactl` can be used as follows

```shell
venhoadactl stop
venhoadactl start
venhoadactl restart
```

## Get ComposeD logs on venhoadactl command failures

The `venhoadactl` commands like start, stop, and update call the ComposeD service and may occasionally fail.
To get more information on these failures you can check the composed log with

```shell
journalctl -u composed@venhoada
```


## Clean unused images

In the case that the system doesn't seem to update images correctly, might be worth performing a pruning of the dangling unused images using the command below:

```shell
nerdctl system prune -af
```

Followed by:

```shell
nerdctl compose create --force-recreate
```

## Clean Userdata and Accounts

If you have some issues, especially when updates break during the alpha release schedule. These commands remove your user and associated data from the system allowing you to recreate a fresh new user.
Take care not to remove the `ollama`, `pytorch-transformers` or `rknn-llm` directories, as these contain LLM models which are quite big and you probably don't want to reinstall them.

```shell
venhoadactl shell nerdctl compose down
cd /home/venho-data
rm -rf ipfs qdrant quadstore valkey
venhoadactl restart --force
```

## Resetting Your Mind2 Device Using Recovery Mode

If your **Mind2** is not starting up normally or not starting at all, you can try to fix it using **Recovery Mode**.

#### Step 1: Access Recovery Button

To boot into Recovery Mode, you may need to **remove the bottom plate** of your Mind2 device to access the **Recovery** button.

- While **connecting power**, **hold the Recovery button** until the **power button lights up**.
- The Recovery button is the **right-hand side** button (if the power button is facing away from you).
- It is labeled **"RECOVERY"**.

![Recovery mode button location](./assets/images/recovery.png)

#### Step 2: Connect via USB

1. Connect a **USB data cable** from your Mind2 to your **laptop**.
2. Your computer should detect the device as a **USB network interface**.
3. Use a **telnet** client to connect to:

```
telnet 10.42.66.66
```

4. A menu should appear with different recovery options.

![Recovery mode telnet menu](./assets/images/recoveryModeTelnet.png)

#### Step 3: Perform Factory Reset

- Select option **#1**: **“Reset device to factory state.”**
- Wait for the process to complete and **follow any on-screen instructions**.
- ⚠️ **Warning**: This will reset your device to factory settings and **erase all data** on the Mind2.

> **Note**: If your terminal application crashes or disappears during this step, reconnect to Recovery Mode and retry the reset. You may need to **repeat this step several times**.

#### Step 4: Exit Recovery Mode

- Once the reset is complete and you’re back in the menu, select option **#6: "Exit"**.
- Disconnect the USB cable.
- The **Mind2 should now boot up normally**.

## Adding models

You can independently add models to be used through the Venho interface such as the conversation user experience. When adding models, keep in mind the hardware requirements of the models to ensure you are choosing ones which will best compliment your Mind2:

```shell
nerdctl exec -it ollama ollama pull some-model # For example, gemma2:2b
nerdctl compose restart llm-router
```

Then wait a few minutes and refresh your webpage, the model should now be available in the model picker in the Conversation experience.

## Re-Install Venho Platform from scratch

The below instructions completely wipe all your data (in Venho) as well as download the container images and recreate the containers.

```shell
venhoadactl shell nerdctl compose down
nerdctl system prune -af
cd /home/venho-data
rm -rf frontend-httpd ipfs qdrant quadstore secrets valkey
venhoadactl update --force
venhoadactl start
```

Downloading the the images will take a while.


## Turn Mind2 LEDs off

The Mind2 LEDs are quite useful to understand in which state the Software is and what is doing.
You can however turn them off via the following command

```shell
echo 0 > /sys/class/leds/Led/brightness
```

A better way is perhaps to use `mce-tools`.

First install it as root via

```shell
devel-su pkcon install mce-tools
```

Then you can, either as root or simple user :

Turn Off

```shell
mcetool -L
```

Turn On

```shell
mcetool -l
```

## How to Fix Venho.ai Container Registry Auth Issue
You run into an authentication issue pulling new container images from the `Venho.ai` (registry.jolla.com) registry.

An error message in `journalctl -u composed@venhoada` looks like the following:

```
Jun 13 14:35:12 Mind2 composed[17572]: time="2025-06-13T14:35:12+03:00" level=info msg="fetch failed" error="pull access denied, repository does not exist or may require authorization: authorization failed: no basic auth credentials" host=registry.jolla.com
Jun 13 14:35:12 Mind2 composed[17572]: time="2025-06-13T14:35:12+03:00" level=fatal msg="failed to resolve reference \"venho.ai/venho-ada/storage:0.4.6\": pull access denied, repository does not exist or may require authorization: authorization failed: no basic auth credentials"
Jun 13 14:35:12 Mind2 composed[17572]: time="2025-06-13T14:35:12+03:00" level=fatal msg="error while pulling image venho.ai/venho-ada/storage:0.4.6: exit status 1"
Jun 13 14:35:12 Mind2 composed[17572]: INFO: Update [100%]: Failed - Command '('/usr/bin/nerdctl', 'compose', 'pull', '-q')' returned non-zero exit status 1.
```

You can check your stored registry credentials using the `docker-credential-ssu` CLI that will query the SailfishOS keystore:

```shell
(venho-ada)[root@Mind2 venho-ada]# printf "registry.jolla.com" | docker-credential-ssu -d get
[D] unknown:0 - Credentials reguested for "registry.jolla.com"
[D] unknown:0 - Updating store credentials
{
    "error": "Credentials not found"
}
```

An error message will tell you that the credentials are missing.

To fix this issue, please:

- connect your `Mind2` device with a display and mouse
- open the Jolla store application

The Jolla store application will update the registry authentication credentials in the keystore. You can verify this 
by using the previous `docker-credentials-ssu` command to check your success:

```shell
(venho-ada)[root@Mind2 venho-ada]# printf "registry.jolla.com" | docker-credential-ssu -d get
[D] unknown:0 - Credentials reguested for "registry.jolla.com"
[D] unknown:0 - Updating store credentials
{
    "Secret": "xxxxxxxxxxxxxxxxx",
    "ServerURL": "registry.jolla.com",
    "Username": "johndoe"
}
```

It should now be possible to pull the new `Venho.ai` images from the registry.
