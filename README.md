# Домашнее задание к занятию "`Что такое DevOps. СI/СD`" - `Казанцева Елена`

### Задание 1

**Что нужно сделать:**

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
2. Установите на машину с jenkins [golang](https://golang.org/doc/install).
3. Используя свой аккаунт на GitHub, сделайте себе форк [репозитория](https://github.com/netology-code/sdvps-materials.git). В этом же репозитории находится [дополнительный материал для выполнения ДЗ](https://github.com/netology-code/sdvps-materials/blob/main/CICD/8.2-hw.md).
4. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта ```go test .``` и  ```docker build .```.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

Результат сборки (консоль):
```
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/my_freestyle1
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/my_freestyle1/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url http://github.com/ElenaKazantseva/sdvps-8-02.git # timeout=10
Fetching upstream changes from http://github.com/ElenaKazantseva/sdvps-8-02.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
 > git fetch --tags --force --progress -- http://github.com/ElenaKazantseva/sdvps-8-02.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 223dbc3f489784448004e020f2ef224f17a7b06d (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 223dbc3f489784448004e020f2ef224f17a7b06d # timeout=10
Commit message: "Update README.md"
First time build. Skipping changelog.
[my_freestyle1] $ /bin/sh -xe /tmp/jenkins17216798040096271600.sh
+ /usr/lib/go-1.21/bin/go test .
ok  	github.com/netology-code/sdvps-materials	0.001s
[my_freestyle1] $ /bin/sh -xe /tmp/jenkins5271345336840450066.sh
+ docker build . -t ubuntu-bionic:8082/hello-world:v4
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  245.8kB

Step 1/8 : FROM golang:1.16 AS builder
1.16: Pulling from library/golang
e4d61adff207: Pulling fs layer
4ff1945c672b: Pulling fs layer
ff5b10aec998: Pulling fs layer
12de8c754e45: Pulling fs layer
8c86ff77a317: Pulling fs layer
0395a1c478ba: Pulling fs layer
245345d44ed8: Pulling fs layer
0395a1c478ba: Waiting
12de8c754e45: Waiting
245345d44ed8: Waiting
ff5b10aec998: Verifying Checksum
ff5b10aec998: Download complete
4ff1945c672b: Verifying Checksum
4ff1945c672b: Download complete
e4d61adff207: Verifying Checksum
e4d61adff207: Download complete
12de8c754e45: Verifying Checksum
12de8c754e45: Download complete
245345d44ed8: Verifying Checksum
245345d44ed8: Download complete
e4d61adff207: Pull complete
8c86ff77a317: Verifying Checksum
8c86ff77a317: Download complete
4ff1945c672b: Pull complete
ff5b10aec998: Pull complete
0395a1c478ba: Verifying Checksum
0395a1c478ba: Download complete
12de8c754e45: Pull complete
8c86ff77a317: Pull complete
0395a1c478ba: Pull complete
245345d44ed8: Pull complete
Digest: sha256:5f6a4662de3efc6d6bb812d02e9de3d8698eea16b8eb7281f03e6f3e8383018e
Status: Downloaded newer image for golang:1.16
 ---> 972d8c0bc0fc
Step 2/8 : WORKDIR $GOPATH/src/github.com/netology-code/sdvps-materials
 ---> Running in a5a60e419652
Removing intermediate container a5a60e419652
 ---> 07947b262148
Step 3/8 : COPY . ./
 ---> e62dc5441da1
Step 4/8 : RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o /app .
 ---> Running in 531988b446eb
Removing intermediate container 531988b446eb
 ---> e1ddf91c9ca3
Step 5/8 : FROM alpine:latest
latest: Pulling from library/alpine
4abcf2066143: Pulling fs layer
4abcf2066143: Verifying Checksum
4abcf2066143: Download complete
4abcf2066143: Pull complete
Digest: sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
Status: Downloaded newer image for alpine:latest
 ---> 05455a08881e
Step 6/8 : RUN apk -U add ca-certificates
 ---> Running in 1aea6fc13bec
fetch https://dl-cdn.alpinelinux.org/alpine/v3.19/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.19/community/x86_64/APKINDEX.tar.gz
(1/1) Installing ca-certificates (20230506-r0)
Executing busybox-1.36.1-r15.trigger
Executing ca-certificates-20230506-r0.trigger
OK: 8 MiB in 16 packages
Removing intermediate container 1aea6fc13bec
 ---> 758307ef8b40
Step 7/8 : COPY --from=builder /app /app
 ---> b68ef1a05872
Step 8/8 : CMD ["/app"]
 ---> Running in e4b2ad72a48b
Removing intermediate container e4b2ad72a48b
 ---> e48e34c486c8
Successfully built e48e34c486c8
Successfully tagged ubuntu-bionic:8082/hello-world:v4
Finished: SUCCESS
```

Результат сборки (скриншот):
![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/sdvps-8-02-CICD/img/Screenshot_2.jpg)

![Screenshot_1](https://github.com/ElenaKazantseva/homeworks/blob/sdvps-8-02-CICD/img/Screenshot_1.jpg)


---

### Задание 2

**Что нужно сделать:**

1. Создайте новый проект pipeline.
2. Перепишите сборку из задания 1 на declarative в виде кода.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.

---

В качестве ответа добавьте ссылку на этот коммит в ваш md-файл с решением:

[ссылка на мое решение, Second commit](https://github.com/ElenaKazantseva/hw-git-1/commit/f239802bc3c0733b4070aa47cce4611033454235)

---

### Задание 3

**Что нужно сделать:**

1. Установите на машину Nexus.
2. Создайте raw-hosted репозиторий.
3. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
4. Загрузите файл в репозиторий с помощью jenkins.

В качестве ответа пришлите скриншоты с настройками проекта и результатами выполнения сборки.


В качестве ответа прикрепите ссылку на граф коммитов https://github.com/ваш-логин/ваш-репозиторий/network в ваш md-файл с решением.

Ваш граф комитов должен выглядеть аналогично скриншоту:   

![скрин для Git](https://github.com/netology-code/sdvps-homeworks/assets/77622076/e73589cf-7e97-40e5-ac01-d1d55376f1b9)

[ссылка на мое решение, graph](https://github.com/ElenaKazantseva/hw-git-1/network)
