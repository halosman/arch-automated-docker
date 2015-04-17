# arch-automated-docker
Run docker containers as services.

## Install
1. Extract tar file
2. run `makepkg`
3. Intall via `sudo pacman -U arch-automated-docker-git.tar.gz`

## Configure
Program works by taking configuration from `automated-docker.conf` and applying them to 
`automated-docker@.service.template` in order to generate a `automated-docker@imageName.service`.

### Modifying automated-docker.conf
`automated-docker.conf` is used to store the command line parameters needed to launch the container. 
Generally this is found in `/etc/automated-docker/automated-docker.conf`.
Example Entry:
```
[arch]
-p 555:555
-v ~/stuff:/home/things
base/archlinux
```

Name of image is within brackets, each additional line should include only one **arg value** pair, with 
the last line being the base image. 

In this example the image name is *arch*, has command line options `-p 555:555 -v` and ` 
~/stuff:/home/things`, and uses image `base/archlinux` in the docker repositories. 

Now that the conf file is setup, now it's time to modify the template file.

### Modifying service template files
arch-automated-docker takes the .template files, adds the command line arguments and container name from 
`automated-docker.conf`, and generates a `.service` file to be used with systemd. By default 
automated-docker will create a `automated-docker@.service.template` file (usually in 
`/etc/automated-docker/`). When start-container reads `automated-docker.conf` it will search for any 
template files with the name `automated-docker@containername.service.template`, if that is not found 
then it will use `automated-docker@.service.template`. Either way, the script will create a file named 
`automated-docker@containername.service` and place it in the systemd service directory 
(`/etc/systemd/system/` by default). This can then be loaded on startup (`sudo systemctl enable 
automated-docker@containername.service`) and started (`sudo systemctl start 
automated-docker@containername.service`).

The default `automated-docker@.service.template` should be fine for most users. Note that the default 
solution takes advantage of Ibukanov in the thread https://github.com/docker/docker/issues/6791.

## Run
Simply issue `start-container` to create service files for each container listed in 
`automated-docker.conf`.

## Quick Start
This quick start was written using defaults and Arch Linux. It shows how to create a basic arch linux 
image that binds port 553 of the host.

1. Install Using Directions above
2. Add the following below the comments in `/etc/automated-docker/automated-docker.conf`
```
[arch]
-p 553:553
base/archlinux
```
3. [Optional] copy the default template file to a new one named `automated-docker@arch.service`
```
cp /etc/automated-docker/automated-docker@.service /etc/automated-docker/automated-docker@arch.service
```
4. Run start-container
`start-container`
5. Start and enable the service to run at bootup
`sudo systemctl start automated-docker@arch.service && sudo systemctl enable 
automated-docker@arch.service`
