docker-snap-volume
The docker-snap-volume tool allows you to create and restore snapshot volumes in Docker. When a snapshot is created, a .tar file is generated, which can be restored to the volume as needed.

Motivation
While developing a script to patch a large database, the need arose to quickly restore the database to its current state. The traditional method of importing a dump took over 7 hours. Thus, a Docker snapshot tool was developed to simplify and expedite this process.

Installation
To download the script and set it up on your system, follow the steps below:

Download the script:

bash
Copy code
sudo curl -SL https://raw.githubusercontent.com/BillRizer/docker-snap-volume/main/docker-snap-volume -o /usr/local/bin/docker-snap-volume
Change permissions:

bash
Copy code
sudo chmod +x /usr/local/bin/docker-snap-volume
How to Use
Create a Snapshot
To create a snapshot of a volume, use the command below:

bash
Copy code
docker-snap-volume -n volume-name -f /absolute/path/to/volume-snapshot.tar --create
-n: The name of the Docker volume to be snapshotted.
-f: The absolute path to the .tar file where the snapshot will be saved.
Restore a Snapshot
To restore a volume from a snapshot, use the following command:

bash
Copy code
docker-snap-volume -n volume-name -f /absolute/path/to/volume-snapshot.tar --restore
-n: The name of the Docker volume to be restored.
-f: The absolute path to the .tar file of the snapshot to be restored.
Feel free to contribute to this project or open issues for improvements and fixes.
