# Development Workspace

Vagrant development workspace

## Requirements

|Requirements|Recommend versions|
|:---:|:---:|
|vagrant|2.0.1 or later|
|vagrant-vbguest|latest|
|virtualbox|5.2 or later|

## Specs

|Spec|Value|
|:---:|:---:|
|IP Address|192.168.128.3|
|Hostname|develop.localdomain|

## How to use

### Vagrant

Run these commands in the root directory of this project.

- ```vagrant up``` startup and build workspace
- ```vagrant ssh``` login workspace
- ```vagrant halt``` shutdown workspace
- ```vagrant destroy``` destroy workspace(for rebuild)

### Compose

Login to workspace and run these commands.

- ```comopse up``` startup and build compose service(s)
- ```compose reload``` restart compose service(s)
- ```compose destroy``` shutdown compose service(s)
- ```compose build``` rebuild compose service(s)
- ```compose volume``` list volume(s)
- ```compose volume rm [volume_name(s)]``` remove volume(s)

### Import a project

* Edit ```Vagrantfile``` to mount project as synced folder of vagrant.

```ruby
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox", mount_options: ["ttl=0"]
  # add project folder around line 56
  config.vm.synced_folder "../(project folder name)", "/(project folder name)", type: "virtualbox", owner: "vagrant", group: "vagrant", mount_options: ["ttl=0"]
```

* Replace environment variables in ```.dockerrc```.

```shell script
export COMPOSE_PROJECT_NAME="(project folder name)"
export COMPOSE_FILE="/(project folder name)/docker-compose.yml"
export PATH="/(project folder name):$PATH"
```

* Change file status for avoid to reflection by git.

```shell script
git update-index --skip-worktree Vagrantfile
git update-index --skip-worktree .dockerrc
```

* Terminate old project services, if running it.

```shell script
vagrant ssh           
# login vagrant environment
compose destroy
exit
# logout vagrant environment
vagrant reload
```
