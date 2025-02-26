# Venho Ai / Jolla Mind2 FAQ

## Introduction

This file is meant to collect useful information for Venho Ai / Jolla Mind2 users to help them troubleshoot and resolve known issues.
All the commands below are expected to be run with admin privileges (`devel-su`) and `venho-ada-env` environment settings.

## Find out what containers are running

Venho AI system uses containerd [nerdctl](https://github.com/containerd/nerdctl) as its container management tool. To see what is running, use the following command:

`nerdctl container ls`

## See logs for each container

Containers directly associated with the Venho app are usually found within the `venho.ai/venho-ada/` image location, although the application might use other containers running as well.
At the time of writing this FAQs the known containers for Venho applications are:

- gateway
- opaque
- keygen
- quadstore
- llm
- pytorch-transformers
- mailbox-provider
- storage
- processor
- venho-ada-frontend-httpd-1
- ollama
- ipfs
- redis
- qdrant

In order to see the logs nerdctl can be used running the following command:

```shell
# Switch to root
devel-su
# Switch to venho-ada-env enviorment
venho-ada-env

# See logs for all containers:
nerdctl compose logs -f
# See logs for a specific container (e.g., processor):
nerdctl compose logs -f processor
```

## Start, Stop and restart the Service

Venho Ada service is run by default at system start-up, in order to start stop and restart the service `systemctl` can be used as follows

```shell
systemctl stop venho-ada.service
systemctl start venho-ada.service
systemctl restart venho-ada.service
```

## Manually update the venho ada software version running or change version

The version of the software installed is stored within the RELEASE_TAG variable in /var/lib/venho-ada/local.env file.
The user can change this variable if required using the following procedure (before updating it is recommended to stop the service running and stop the containers) :

```shell
# Stop Service and containers
devel-su
venho-ada-env
systemctl stop venho-ada.service
nerdctl compose down
# exit the venho-ada-env environment
exit
# Update version tag
echo "RELEASE_TAG=0.1.10" > /var/lib/venho-ada/local.env
# Update containers and start
venho-ada-env
nerdctl compose pull
systemctl start venho-ada.service
```

## Clean unused images

In the case that the system doesn't seem to update images correctly, might be worth performing a pruning of the dangling unused images using the command below:

```shell
nerdctl system prune -af
```

Followed by:

```shell
compose create --force-recreate
```

## Clean Userdata and Accounts

If you have some issues, especially when updates break during the alpha release schedule. These commands remove your user and associated data from the system allowing you to recreate a fresh new user.
Take care not to remove the `ollama` or `pytorch-transformers` directories, as these contain LLM models which are quite big and you probably don't want to reinstall them.

```shell
devel-su
systemctl stop venho-ada.service
venho-ada-env
nerdctl compose stop
rm -rf /home/venho-data/qdrant
rm -rf /home/venho-data/quadstore
rm -rf /home/venho-data/valkey
nerdctl compose create --force-recreate
reboot
```

## Adding models

You can independently add models to be used through the Venho interface such as the conversation user experience. When adding models, keep in mind the hardware requirements of the models to ensure you are choosing ones which will best compliment your Mind2:

```shell
devel-su
venho-ada-env
nerdctl exec -it ollama ollama pull some-model # For example, gemma2:2b
nerdctl compose restart llm-router
exit
```

Then wait a few minutes and refresh your webpage, the model should now be available in the model picker in the Conversation experience.

## Re-Install Venho Platform from scratch

The below instructions completely wipe all your data (in Venho) as well as re-install the entire Venho platform to factory image state.

```shell
devel-su
systemctl stop venho-ada.service
venho-ada-env
nerdctl compose down
nerdctl system prune -af
rm -rf /home/venho-data
rm /var/lib/venho-ada/preload-done
reboot
```

Once device has restarted and you have regained SSH access, you may monitor install status with:

```shell
journalctl logs -f
```

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

## How to Fix the KEYGEN_FAIL error when trying to login

The Services were not properlly started. So you will have to start them manually again.

Switch to root

```shell
devel-su
```

Enter the venho-ada-env enviorment

```shell
venho-ada-env
```

Stop the venho-ada service and the containers

```shell
systemctl stop venho-ada.service
nerdctl compose down
```

Start the venho-ada service again

```shell
systemctl start venho-ada.service
```

You will have to wait untill all containers are running again. You can check if all are running again via:

```shell
nerdctl compose ps -a
```

After you are done you can exit the venho-ada-env enviorment again

```shell
exit
```
## How to Fix Venho.ai Container Registry Auth Issue
You run into an authentication issue pulling new container images from the `Venho.ai` (registry.jolla.com) registry. 
An error message shown on the root shell looks e.g. like the following (pulling release tag 0.3.0):

```shell
(venho-ada)[root@Mind2 defaultuser]# nerdctl compose pull
INFO[0000] Pulling image venho.ai/venho-ada/frontend-httpd:0.3.0
venho.ai/venho-ada/frontend-httpd:0.3.0: resolving      |--------------------------------------|
venho.ai/venho-ada/frontend-httpd:0.3.0: resolving      |--------------------------------------|
elapsed: 25.3s                           total:   0.0 B (0.0 B/s)
FATA[0025] failed to resolve reference "venho.ai/venho-ada/frontend-httpd:0.3.0": pull access denied, repository does not exist or may require authorization: authorization failed: no basic auth credentials
FATA[0025] error while pulling image venho.ai/venho-ada/frontend-httpd:0.3.0: exit status 1
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
