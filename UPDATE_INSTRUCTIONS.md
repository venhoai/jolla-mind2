## Upgrading to 0.4.2

1. Update Sailfish OS to 5.0.0.67
2. As root clear the old venho-data
        devel-su
        systemctl stop venho-ada
        cd /home/venho-data
        rm -r frontend-httpd ipfs ollama qdrant quadstore secrets valkey
3. Download and extract the new data files in /home/venho-data
        curl -O https://releases.jolla.com/venho/data/rknn-llm_0.2.0_data.tar
        curl -O https://releases.jolla.com/venho/data/ollama_0.2.0_data.tar
        tar xvf rknn-llm_0.2.0_data.tar
        tar xvf ollama_0.2.0_data.tar
4. Enter the venho-ada-env and update the containers
        venho-ada-env
        /usr/libexec/venho-ada/setup
        nerdctl compose down
        nerdctl compose pull
        exit
5. Restart the venho-ada service
        systemctl start venho-ada (edited) 
