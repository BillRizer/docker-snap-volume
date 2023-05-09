# docker-snap-volume
This tool creates and restores snapshot volumes on docker, when creating a snapshot a .tar is generated that can be restored to the volume

# motivation:
I was making a script that patched a gigantic database, and needed to restore it to the current state in the fastest way (importing a dump took over 7hrs). so I found a snapshot tool for docker simpler.


get script
````bash 
sudo curl -SL https://raw.githubusercontent.com/BillRizer/docker-snap-volume/main/docker-snap-volume -o /usr/local/bin/docker-snap-volume
````
change permission
````bash 
sudo chmod +x /usr/local/bin/docker-snap-volume
````

How to use
````
docker-snap-volume -n my-volume-name -f /absolute/path/volume-snapshot.tar --create
````
````
docker-snap-volume -n my-volume-name -f /absolute/path/volume-snapshot.tar --restore
````
