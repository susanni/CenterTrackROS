# CenterTrackROS
ROS implementation of https://github.com/xingyizhou/CenterTrack in Docker

### Using Docker Image
```bash
# Build Docker image.
# Inside CenterTrackROS directory which contains Dockerfile:
docker build -t center-track-ros .
export CENTERTRACK_IMAGE_ID=$(docker images --filter=reference=center-track-ros --format "{{.ID}}")

# Mount directory that contains CenterTrack models as a volume.
# i.e. My models are located at /home/swarm/unreal_ssd/CenterTrack
docker run -v /home/swarm/unreal_ssd/CenterTrack:/home/CenterTrackModels \
           -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
           --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" \
           -it $CENTERTRACK_IMAGE_ID bash
# Inside Docker container:
exit

export CENTERTRACK_CONTAINER_ID=$(docker ps -a | grep $CENTERTRACK_IMAGE_ID | awk '{ print $1 }')
# Permit docker container to connect to X windows display.
xhost +local:`docker inspect --format='{{ .Config.Hostname }}' $CENTERTRACK_CONTAINER_ID`

# Start the container again:
docker start $CENTERTRACK_CONTAINER_ID
docker exec -it $CENTERTRACK_CONTAINER_ID /bin/bash
# This will leave the Docker container continuously running until it is killed.

# Inside Docker container, run:
sourceros
```
NOTES:
* WARNING: xhost [not the safest](http://wiki.ros.org/docker/Tutorials/GUI#The_simple_way).
  * To remove permissions after done using display: <code>xhost -local:\`docker inspect --format='{{ .Config.Hostname }}' $CENTERTRACK_CONTAINER_ID\`</code>
