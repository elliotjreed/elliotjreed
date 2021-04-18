# Remove all Docker containers, images, networks and volumes

If you are like me and play around a lot with Docker, you will probably end up with a lot of containers, networks, volumes, and images in your system which you don't need. Here's a quick guide on how to remove these.

By using `xargs --no-run-if-empty` we can prevent Docker trying to stop containers or remove containers, images, networks, or volumes if there are none to remove.

## Individually


### Stop all Docker containers and remove / delete them

```bash
docker ps -a -q | xargs --no-run-if-empty docker stop
docker ps -a -q | xargs --no-run-if-empty docker rm
```

### Remove / delete all Docker networks

```bash
docker network ls -q | xargs --no-run-if-empty docker network rm
```


### Remove / delete all Docker volumes

```bash
docker volume ls -q | xargs --no-run-if-empty docker volume rm
```


### Remove / delete all Docker images

```bash
docker images -q -a | xargs --no-run-if-empty docker rmi
```


## Bash function to delete all Docker containers, volumes, networks, and images at once

This function will ask for confirmation before deleting the containers, volumes, and networks, and will ask again for the images (as you may want to keep these for development and not have to re-download them).

You could add this to your .bashrc file (eg. `nano ~/.bashrc`) so you always have it to use. Remember after adding it to the .bashrc file to source it to load in your changes:

```bash
. ~/.bashrc
```

or

```bash
source ~/.bashrc
```

Here's the Bash function:

```bash
rmdocker() {
	read -p $'\e[31mAre you sure you want to delete ALL Docker containers, volumes, and networks? [y/n]\e[0m\n' -n 1 -r
	echo -e "\n"
	if [[ $REPLY =~ ^[Yy]$ ]]
	then
		# Stop all containers
		docker ps -a -q | xargs --no-run-if-empty docker stop
		# Delete all containers
		docker ps -a -q | xargs --no-run-if-empty docker rm
		# Delete all networks
		docker network ls -q | xargs --no-run-if-empty docker network rm
		# Delete all volumes
		docker volume ls -q | xargs --no-run-if-empty docker volume rm
		read -p $'\e[31mAre you sure you want to delete ALL Docker images as well? [y/n]\e[0m\n' -n 1 -r
		echo -e "\n"
		if [[ $REPLY =~ ^[Yy]$ ]]
		then
			# Delete all images
			docker images -q -a | xargs --no-run-if-empty docker rmi
		fi
	fi
}
```
