# Ubuntu 22.10 LTS (Kinetic Kudu) Ansible Test Image
Ubuntu 22.10 LTS (Kinetic Kudu) Docker container for Ansible playbook and role testing.

# Credits
98% of the work goes to [Jeff Geerling](https://github.com/geerlingguy). I highly recommend checking out his content on YouTube.

All I did was incorporate his Shell Script within the Dockerfile and set the base image to `Ubuntu:2210`.

# How-To Build Image from Dockerfile
* Download the DockerFile onto your machine that has docker installed. 
* Run the following command to build the image:
```
docker build -t docker-ubuntu2210-ansible:latest .
```

# How-To Run the Image
* Once the image has been built, create a container by running the following command:
```
docker run --detach --privileged --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw --cgroupns=host docker-ubuntu2210-ansible:latest
```

# How-To Run the Image in Molecule

NOTE: For individuals using WSL2 (Ubuntu 20.04), you may need to run the following commands on your WSL2 instance to get systemd to work:
```
sudo apt-get update && sudo apt-get install -yqq daemonize dbus-user-session
sudo daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
exec sudo nsenter -t $(pidof systemd) -a su - $LOGNAME
```

* Here's an example of a `molecule.yml` file to test the image with your ansible playbooks/roles:
```
---
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: instance-1-ubuntu2210
    image: docker-ubuntu2210-ansible:latest
    privileged: true
    pre_build_image: true
provisioner:
  name: ansible
  config_options:
    defaults:
      remote_tmp: /tmp
verifier:
  name: ansible
```

* Within the root of your project, run the following:
```
molecule converge
```

# Notes
This image is for TESTING PURPOSES ONLY!!
