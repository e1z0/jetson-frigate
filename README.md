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
