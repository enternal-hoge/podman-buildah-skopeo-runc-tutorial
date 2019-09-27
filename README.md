# podman-buildah-skopeo-runc-tutorial

this contents is following tools tutorial.

podman
buildah
skopeo
runc

# Prerquirement

after executed to install.

https://github.com/enternal-hoge/podman-buildah-skopeo-runc-installation-on-ubuntu

# Tutorial

## Tutorial - Podman

```
# podman info
host:
  BuildahVersion: 1.10.1
  Conmon:
    package: 'conmon: /usr/libexec/podman/conmon'
    path: /usr/libexec/podman/conmon
    version: 'conmon version 2.0.0, commit: unknown'
  Distribution:
    distribution: ubuntu
    version: "18.04"
  MemFree: 6108446720
  MemTotal: 7065591808
  OCIRuntime:
    package: 'containerd.io: /usr/bin/runc'
    path: /usr/bin/runc
    version: |-
      runc version 1.0.0-rc8
      commit: 425e105d5a03fabd737a126ad93d62a9eeede87f
      spec: 1.0.1-dev
  SwapFree: 2147483648
  SwapTotal: 2147483648
  arch: amd64
  cpus: 4
  eventlogger: journald
  hostname: LAPTOP-CC0S1UVB
  kernel: 4.19.67-microsoft-standard
  os: linux
  rootless: false
  uptime: 31m 39.79s
registries:
  blocked: null
  insecure: null
  search: null
store:
  ConfigFile: /etc/containers/storage.conf
  ContainerStore:
    number: 0
  GraphDriverName: overlay
  GraphOptions: null
  GraphRoot: /var/lib/containers/storage
  GraphStatus:
    Backing Filesystem: extfs
    Native Overlay Diff: "true"
    Supports d_type: "true"
    Using metacopy: "false"
  ImageStore:
    number: 0
  RunRoot: /var/run/containers/storage
  VolumePath: /var/lib/containers/storage/volumes
```

```
# curl https://raw.githubusercontent.com/projectatomic/registries/master/registries.fedora -o /etc/containers/registries.conf
# curl https://raw.githubusercontent.com/containers/skopeo/master/default-policy.json -o /etc/containers/policy.json
# podman pull alpine
Trying to pull docker.io/library/alpine...
Getting image source signatures
Copying blob 9d48c3bd43c5 done
Copying config 9617696764 done
Writing manifest to image destination
Storing signatures
961769676411f082461f9ef46626dd7a2d1e2b2a38e6a44364bcbecf51e66dd4

# podman images
REPOSITORY                 TAG      IMAGE ID       CREATED       SIZE
docker.io/library/alpine   latest   961769676411   3 weeks ago   5.85 MB
```

Podman’s local repository
```
# ls /var/lib/containers
```

```
# podman run -it docker.io/library/alpine /bin/sh
/ # exit
```

```
# podman ps -a
CONTAINER ID  IMAGE                            COMMAND  CREATED         STATUS                    PORTS  NAMES
da984081082c  docker.io/library/alpine:latest  /bin/sh  11 seconds ago  Exited (0) 8 seconds ago         infallible_babbage
root@podman:~# podman rm da984081082c
da984081082cb5b89995bcdac516ecfda73a917091d2d9f2670b3ba090134f42

# podman rm da984081082c
da984081082cb5b89995bcdac516ecfda73a917091d2d9f2670b3ba090134f42
```

## Tutorial - Buildah

## Tutorial - Scopeo

## Tutorial - runc

