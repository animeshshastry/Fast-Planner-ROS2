USER_UID := $(shell id -u)
USER_GID := $(shell id -g)

build:
	docker build \
	--build-arg USER_UID=$(USER_UID) \
	--build-arg USER_GID=$(USER_GID) \
	-f Dockerfile . -t fast --rm

run:
	docker run -itd --rm --net=host --pid host --ipc host --gpus all \
    --env=\"NVIDIA_DRIVER_CAPABILITIES=all\" --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
	-v /dev:/dev \
	-v ./../:/home/workspace/ \
	--name fast fast

start:
	docker start fast

stop:
	docker stop fast

kill:
	docker kill fast

attach:
	docker exec -it fast bash

commit:
	docker commit fast fast2
	docker kill fast
	docker tag fast2 fast
