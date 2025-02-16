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

In order to see the logs nerdctl can be used running the following command:

```
# See logs for all containers:
nerdctl compose logs -f
# See logs for a specific container (e.g., processor):
nerdctl compose logs -f processor
```

## Start, Stop and restart the Service

Venho Ada service is run by default at system start-up, in order to start stop and restart the service `systemctl` can be used as follows

```
systemctl stop venho-ada.service
systemctl start venho-ada.service
systemctl restart venho-ada.service
```

## Manually update the venho ada software version running or change version

The version of the software installed is stored within the RELEASE_TAG variable in /var/lib/venho-ada/local.env file.  
The user can change this variable if required using the following procedure (before updating it is recommended to stop the service running and stop the containers) :
```
# Stop Service and containers
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

`nerdctl system prune -af`
and then 
`compose create --force-recreate` 

## Re-Install Venho Platform from scratch

The below instructions completely wipe all your data (in Venho) as well as re-install the entire Venho platform to factory image state.

```
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

```
journalctl logs -f
```

## Turn Mind2 LEDs off

The Mind2 LEDs are quite useful to understand in which state the Software is and what is doing.
You can however turn them off via the following command

`echo 0 > /sys/class/leds/Led/brightness`

A better way is perhaps to use `mce-tools`.

First install it as root via

``` shell
devel-su pkcon install mce-tools
```

Then you can, either as root or simple user :

Turn Off
``` shell
mcetool -L
```

Turn On
``` shell
mcetool -l
```
