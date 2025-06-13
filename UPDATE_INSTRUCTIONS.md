>[!CAUTION]
> Your device must be connected to a display and show the Sailfish UX for updates to work.
>

# Upgrading to 0.4.6

Starting with Sailfish OS 5.0.0.68 the Venho containers can be updated from the
Sailfish Settings UI.

First update Sailfish OS and then go to Settings > System > Venho ADA. From
there youcan check and install the update.

Note that the update progress indicator is still quite coarse, so it may seem
like it's stuck on operations like downloading the images.


# Upgrading to 0.4.5

>[!IMPORTANT]
> If you're upgrading from a release prior to 0.4.2, you will need to follow steps 1, 2 and 3. In the [Upgrading to 0.4.2](#upgrading-to-042) section
>

1. From a terminal session on the device, stop running containers with the following:
   ```/bin/sh
      devel-su
      systemctl stop venho-ada
   ```

2. Now update the `RELEASE_TAG` that the `venho-ada-env` will use to resolve container images:
   ```/bin/sh
      echo "RELEASE_TAG=0.4.5" > /var/lib/venho-ada/local.env
   ```

3. Once the `RELEASE_TAG` is updated, we can now enter the `venho-ada-env` shell environment:
   ```/bin/sh
      venho-ada-env
      env | grep RELEASE_TAG
   ```

   The above should output: `RELEASE_TAG=0.4.5`

3. From within the `venho-ada-env` environment, pull in the updated containers:
   ```/bin/sh
      nerdctl compose pull
   ```

4. Next we re-create the venho containers to the updated images:
   ```/bin/sh
      nerdctl compose create --force-recreate
   ```

5. Once this is done, we can restart container services and clean up old images.
   ```/bin/sh
      systemctl start venho-ada
   ```

>[!CAUTION]
> On occasion you may find you get unusual errors when attempting to login, or use the system. If this happens please run the following:
>

   ```/bin/sh
      devel-su venho-ada-env
      nerdctl compose restart frontend-httpd
   ```


# Upgrading to 0.4.2

>[!CAUTION]
>
>You will need to create a new Venho AI Local ID Account after updating to this Version. All data from your old Account will be deleted.

1. Update Sailfish OS to 5.0.0.67 -- This **MUST** be done through the Sailfish UX currently.
2. As root clear the old venho-data
   ```
     devel-su
     systemctl stop venho-ada
     cd /home/venho-data
     rm -r frontend-httpd ipfs ollama qdrant quadstore secrets valkey
   ```
3. Download and extract the new data files in /home/venho-data
   ```
     curl -O https://releases.jolla.com/venho/data/rknn-llm_0.2.0_data.tar
     curl -O https://releases.jolla.com/venho/data/ollama_0.2.0_data.tar
     tar xvf rknn-llm_0.2.0_data.tar
     tar xvf ollama_0.2.0_data.tar
   ```
4. Delete the local.env file if you have one
   ```
      rm /var/lib/venho-ada/local.env
   ```
5. Enter the venho-ada-env and update the containers
   ```
     venho-ada-env
     /usr/libexec/venho-ada/setup
     nerdctl compose down
     nerdctl compose pull
     exit
   ```
6. Restart the venho-ada service
   ```
     systemctl start venho-ada
   ```
