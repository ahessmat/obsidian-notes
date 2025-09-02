https://github.com/groda/big_data/blob/master/docker_for_beginners.md

First we update the Kali base image

```bash
sudo apt update
sudo apt upgrade
```

Installing docker
```bash
sudo apt install docker-cli
sudo apt install docker.io -y
sudo usermod -aG docker $USER
```

Logout and log back in to set the `kali` user as being a part of the docker group.

Check docker is installed with
```bash
docker -v
sudo systemctl status docker
```

# Images

The main entities in Docker are _containers_. A container is a piece of software that emulates a complete machine with some pre-installed libraries and code. Containers allow to distribute software applications together with the whole environment they need for running. By packaging an application in a container we can run it on any computer without the need for any special configuration, thus making the application seamlessly portable.

A Docker image (sometimes also called _container image_) is a file that's essentially a snapshot of a container. Images are created with the `build` command, and they'll produce a container when started with `run`.

**Put plainly: many containers may be instantiated from the same single Docker image**

The above training utilizes **Alpine Linux** for its base image, which is a minimal linux image.

First, we pull the image
```bash
docker pull alpine
```
