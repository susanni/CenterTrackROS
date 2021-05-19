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
docker run -v /home/swarm/unreal_ssd/CenterTrack:/home/CenterTrackModels -it $CENTERTRACK_IMAGE_ID bash

# To start the container again:
export CENTERTRACK_CONTAINER_ID=$(docker ps -a | grep $CENTERTRACK_IMAGE_ID | awk '{ print $1 }')
docker start $CENTERTRACK_CONTAINER_ID
docker exec -it $CENTERTRACK_CONTAINER_ID /bin/bash

# Inside Docker container, run:
sourceros
```
