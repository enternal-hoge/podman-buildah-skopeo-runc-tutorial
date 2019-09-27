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

```
# buildah images
REPOSITORY                 TAG      IMAGE ID       CREATED       SIZE
docker.io/library/alpine   latest   961769676411   5 weeks ago   5.85 MB

# buildah containers
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
```

```
# container=$(buildah from fedora)
Getting image source signatures
Copying blob 5a915a173fbc done
Copying config e9ed59d2ba done
Writing manifest to image destination
Storing signatures

# echo $container
fedora-working-container

# buildah containers
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
17a37c64bcc4     *     e9ed59d2baf7 docker.io/library/fedora:latest  fedora-working-container

# podman images
REPOSITORY                 TAG      IMAGE ID       CREATED       SIZE
docker.io/library/fedora   latest   e9ed59d2baf7   4 weeks ago   254 MB
docker.io/library/alpine   latest   961769676411   5 weeks ago   5.85 MB

# buildah images
REPOSITORY                 TAG      IMAGE ID       CREATED       SIZE
docker.io/library/fedora   latest   e9ed59d2baf7   4 weeks ago   254 MB
docker.io/library/alpine   latest   961769676411   5 weeks ago   5.85 MB

# buildah inspect fedora-working-container
```

```
# buildah run $container bash
[root@17a37c64bcc4 /]# exit
# buildah run $container java
ERRO[0000] container_linux.go:346: starting container process caused "exec: \"java\": executable file not found in $PATH"
container_linux.go:346: starting container process caused "exec: \"java\": executable file not found in $PATH"
error running container: error creating container for [java]: : exit status 1
error while running runtime: exit status 1

# buildah run $container -- dnf -y install java

# buildah run $container java -version
openjdk version "1.8.0_222"
OpenJDK Runtime Environment (build 1.8.0_222-b10)
OpenJDK 64-Bit Server VM (build 25.222-b10, mixed mode)

# buildah containers
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
17a37c64bcc4     *     e9ed59d2baf7 docker.io/library/fedora:latest  fedora-working-container
```

Perhaps, to operate images, docker is neccesarry.

Docker Install on Ubuntu 18.04LTS.
```
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add
# apt-key fingerprint 0EBFCD88
# add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# apt-get update
# apt-get install docker-ce docker-ce-cli containerd.io
# systemctl list-unit-files --type=service | grep docker
```

tutorial restart
```
# buildah containers
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
17a37c64bcc4     *     e9ed59d2baf7 docker.io/library/fedora:latest  fedora-working-container
089d918d1594     *                  scratch                          working-container

# buildah images
REPOSITORY                 TAG      IMAGE ID       CREATED       SIZE
docker.io/library/fedora   latest   e9ed59d2baf7   4 weeks ago   254 MB
docker.io/library/alpine   latest   961769676411   5 weeks ago   5.85 MB

# buildah run $newcontainer bash
ERRO[0000] container_linux.go:346: starting container process caused "exec: \"bash\": executable file not found in $PATH"
container_linux.go:346: starting container process caused "exec: \"bash\": executable file not found in $PATH"
error running container: error creating container for [bash]: : exit status 1
error while running runtime: exit status 1

# scratchmnt=$(buildah mount $newcontainer)

# echo $scratchmnt
/var/lib/containers/storage/overlay/6a89f84baf5b003d56a2b1af52d808833c50a82be972156bd33b16202cb101cc/merged
```

Using Buildah with container registries
```
# registry=$(buildah from registry)
# echo $registry
registry-working-container

# buildah containers
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
17a37c64bcc4     *     e9ed59d2baf7 docker.io/library/fedora:latest  fedora-working-container
089d918d1594     *                  scratch                          working-container
b1ccd41a4696     *     f32a97de94e1 docker.io/library/registry:latest registry-working-container

# buildah images
REPOSITORY                   TAG      IMAGE ID       CREATED        SIZE
docker.io/library/fedora     latest   e9ed59d2baf7   4 weeks ago    254 MB
docker.io/library/alpine     latest   961769676411   5 weeks ago    5.85 MB
docker.io/library/registry   latest   f32a97de94e1   6 months ago   26.4 MB

# podman ps -a
CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  PORTS  NAMES

root@podman:~# buildah run b1ccd41a4696
command must be specified
root@podman:~#
```

buildah on Ubuntu does'nt work 'buildah run XXX'

execute podman
```
# podman images
REPOSITORY                   TAG      IMAGE ID       CREATED        SIZE
docker.io/library/fedora     latest   e9ed59d2baf7   4 weeks ago    254 MB
docker.io/library/alpine     latest   961769676411   5 weeks ago    5.85 MB
docker.io/library/registry   latest   f32a97de94e1   6 months ago   26.4 MB

# podman run f32a97de94e1
time="2019-09-27T07:29:40.335602436Z" level=warning msg="No HTTP secret provided - generated random secret. This may cause problems with uploads if multiple registries are behind a load-balancer. To provide a shared secret, fill in http.secret in the configuration file or set the REGISTRY_HTTP_SECRET environment variable." go.version=go1.11.2 instance.id=df128ea0-9367-4739-9767-ed1b2c83e11c service=registry version=v2.7.1
time="2019-09-27T07:29:40.335721543Z" level=info msg="redis not configured" go.version=go1.11.2 instance.id=df128ea0-9367-4739-9767-ed1b2c83e11c service=registry version=v2.7.1
time="2019-09-27T07:29:40.336648102Z" level=info msg="Starting upload purge in 9m0s" go.version=go1.11.2 instance.id=df128ea0-9367-4739-9767-ed1b2c83e11c service=registry version=v2.7.1
time="2019-09-27T07:29:40.347356375Z" level=info msg="using inmemory blob descriptor cache" go.version=go1.11.2 instance.id=df128ea0-9367-4739-9767-ed1b2c83e11c service=registry version=v2.7.1
time="2019-09-27T07:29:40.349414505Z" level=info msg="listening on [::]:5000" go.version=go1.11.2 instance.id=df128ea0-9367-4739-9767-ed1b2c83e11c service=registry version=v2.7.1
```

To Be...

## Tutorial - Scopeo

## Tutorial - runc

