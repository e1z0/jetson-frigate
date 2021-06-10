# jetson-frigate
Frigate on Jetson Nano research


# Install

## Enable nvidia container runtime by default
You need use nvidia-container-runtime as explained in docs: "It is also the only way to have GPU access during docker build".
```
sudo apt-get install nvidia-container-runtime
```
Edit/create the **/etc/docker/daemon.json** with content:
```
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
         } 
    },
    "default-runtime": "nvidia" 
}
```

Restart docker daemon:

```
sudo systemctl restart docker
```
Build your image (now GPU available during build):
```
docker build -t my_image_name:latest .
```

## Build ffmpeg and frigate

First you need to build ffmpeg with jetson nano support patches
```
make jetson_ffmpeg
```

Now you can build frigate itself
```
make jetson_frigate
```

After that you will have **frigate:latest** docker image.

# Running
```
docker run -d \
 --runtime nvidia \
 --gpus all \
 --name frigate \
 --restart unless-stopped \
 --privileged \
 --shm-size=1024m \
 -p 5000:5000 \
 -v /path/to/config:/config:ro \
 -v /etc/localtime:/etc/localtime:ro \
 -v /media/storage:/media/frigate \
 --device /dev/bus/usb:/dev/bus/usb \
 -e FRIGATE_RTSP_PASSWORD='pass' \
 frigate:latest
```
