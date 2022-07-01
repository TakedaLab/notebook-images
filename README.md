# notebook-images

This repository contains notebook images for KubeFlow.

Available distributions are as follows:
 - `ghcr.io/takedalab/notebook-images:<version>-jupyterlab-ubuntu-20.04`: CPU only
 - `ghcr.io/takedalab/notebook-images:<version>-jupyterlab-nvidia-cuda-11.1.1-cudnn8-devel-ubuntu20.04`: GPU supported


## How to build images?

You can build container images as follows:

```shell
cd docker
export BASE_IMAGE=<Specify base image (e.g., ubuntu:20.04)>
export IMAGE_TAG=<Specify image tag (default: latest)>
export PLAYBOOK=<"base" or "jupyterlab">
export PUSH_IMAGE=false
./build.sh

```


## How to customize images?

If you want to customize images by running shell commands while building images, 
please follow the steps below:

### 1. Create a new role

```shell
cd roles
cp -R template my_role

```

### 2. Customize the role

Edit `roles/my_role/tasks/main.yml` and add any shell commands you want to run


### 3. Add a new playbook

Save the following contents as `playbook/my_playbook.yaml`.

```yaml
- name: Import jupyterlab
  import_playbook: jupyterlab.yaml

- hosts: localhost
  connection: local
  roles:
    - role: takedalab.notebook_images.my_role

```


### 4. Build images

```shell
cd docker
PLAYBOOK=my_playbook ./build.sh

```