# docker-images-to-another-location


* Docker files to move
```
root@inspiron-15-3552:/var/lib/docker# ls -la
```
```
drwx--x--x  4 root root 4096 Dec 31  2023 buildkit
drwx--x---  5 root root 4096 Aug 17 11:15 containers
-rw-------  1 root root   36 Dec 31  2023 engine-id
drwx------  3 root root 4096 Dec 31  2023 image
drwxr-x---  3 root root 4096 Dec 31  2023 network
drwx--x--- 41 root root 4096 Aug 18 19:42 overlay2
drwx------  4 root root 4096 Dec 31  2023 plugins
drwx------  2 root root 4096 Aug 18 19:42 runtimes
drwx------  2 root root 4096 Dec 31  2023 swarm
drwx------  2 root root 4096 Aug 18 19:42 tmp
drwx-----x 25 root root 4096 Aug 18 19:42 volumes
```

> if we do not want to move anything and start from fresh:
```
docker system prune -a
```


## Steps to move Docker to keep images etc in another place: 
```
sudo systemctl stop docker
```

Make your directory and provide proper permisions as above. In my case:
```
sudo mkdir /opt/docker-data
then
sudo mv /var/lib/docker/ /path/to/new/docker/
or
sudo cp -r /var/lib/docker/. /opt/docker-data
```

Edit /etc/docker/daemon.json (if it doesn't exist, create it). In recent version (>=17.03):
```
{
  "data-root": "/new/path/to/docker-data"
}
```
Start Docker
```
sudo systemctl start docker
```

To confirm that Docker was reconfigured:
```
docker info|grep "Docker Root Dir"
```
Finally, clean old docker data directory
```
sudo rm -r /var/lib/docker
```
