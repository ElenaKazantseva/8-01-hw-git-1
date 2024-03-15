# Домашнее задание к занятию "`GitLab`" - `Казанцева Елена`
---

### Задание 1

**Что нужно сделать:**

1. Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в [этом репозитории](https://github.com/netology-code/sdvps-materials/tree/main/gitlab).   
2. Создайте новый проект и пустой репозиторий в нём.
3. Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.


Решение.
Этапы регистрации раннера:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(2).jpg)

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(3).jpg)

Статус раннера:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(1).jpg)

Настройки раннера:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_71%20(1).jpg)


---

### Задание 2

**Что нужно сделать:**

1. Запушьте [репозиторий](https://github.com/netology-code/sdvps-materials/tree/main/gitlab) на GitLab, изменив origin. Это изучалось на занятии по Git.
2. Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте: 
   
 * файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне; 
 * скриншоты с успешно собранными сборками.
 
Решение.

код файла gitlab-ci.yml для проекта:

```
stages:
  - test
  - build

test:
  stage: test
  image: golang:1.19.1
  script: 
   - go test .

build:
  stage: build
  image: docker:latest
  script:
   - docker build .
```

  `image: golang:1.19.1` - нужно было изменить, потому что до установки раннера была установлена другая версия

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_71%20(8).jpg)


Получилось скачать [репозиторий с го-файлами](https://github.com/netology-code/sdvps-materials) на ВМ,
и затем запушить его и docker-ci.yml в мой удаленный репозиторий `gitlab1` 


![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_71%20(7).jpg)


Но сборки не получились. Ни одно из заданий, описанных в docker-ci.yml (test, build), не было выполнено:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(6).jpg)

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(7).jpg)


Гитлаб видит раннер:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(4).jpg)


Настройки раннера:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(3).jpg)

в докере:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(10).jpg)

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(12).jpg)

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(13).jpg)

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(14).jpg)

Логи докер-контейнера:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(9).jpg)

Контейнер запускался и регистрировался через команды из дополнительных материалов к домашнему заданию:

Установка и регистрация:

```
docker run -ti --rm --name gitlab-runner \
     --network host \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest register
```

Запуск:

```
docker run -d --name gitlab-runner --restart always \
     --network host \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest
```


Настройки /etc/hosts:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(5).jpg)

Настройки gitlab.rb:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(1).jpg)

Вывод команды gitlab-ctl status

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_runner%20(8).jpg)


В логах почему-то стоит неактуальный ip-адрес (158.160.127.85), хотя во всех настройках он исправлен на актуальный (158.160.124.244) и сервисы перезапущены. 

Домашнее задание выполняла на ВМ в Яндекс Облаке, сервис останавливался, поэтому ip-адрес менялся автоматически при каждом запуске. 

Но даже когда адрес в логах и на ВМ совпадал, раннер все равно не мог выполнить задачи.

Никак не получается найти решение, в учебном чате тоже не разобрались. Догадка - что-то не так с настройками самого раннера, но что именно, не понимаю.

Пожалуйста, помогите
 





---

### Обновление


Большое спасибо, что указали на проблему! 

Регистрация раннера проходила без указания строчки extra_hosts: ни в командах, ни в конфигурационном файле этого значения не было.

На одном из форумов у кого-то сработал вариант, когда они extra_hosts все-таки прописали. Я тоже попробовала, но мне не помогло.

Сейчас, после того как Вы сказали, что вероятнее всего является причиной, было проще сформулировать запрос и ответ нашелся быстро:
[set network_mode with network name in the config.toml](https://stackoverflow.com/questions/50325932/gitlab-runner-docker-could-not-resolve-host)

Я прописала значение в config.toml:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/конфи.jpg)

И test job завершился успешно:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/тест%201.jpg)


Вывод консоли при выполнении:

```
Running with gitlab-runner 16.9.1 (782c6ecb)
  on gitlab X5k3YRLy, system ID: r_q6ONRml89zQO
Preparing the "docker" executor
Using Docker executor with image gliderlabs/herokuish:latest ...
Starting service postgres:9.6.16 ...
Pulling docker image postgres:9.6.16 ...
Using docker image sha256:cb2889ab0680ba518e0dc1bb5a09cbe23f1e5908d9e484daea3d34f5e092ec17 for postgres:9.6.16 with digest postgres@sha256:d6a6badb2b5b22de5e135490a217522cecc2ae90fe35b33290615827569310a1 ...
Waiting for services to be up and running (timeout 30 seconds)...
*** WARNING: Service runner-x5k3yrly-project-1-concurrent-0-0fd36696fbb0be3b-postgres-0 probably didn't start properly.
Health check error:
create service container: Error response from daemon: conflicting options: host type networking can't be used with links. This would result in undefined behavior (services.go:208:0s)
Service container logs:
*********
Pulling docker image gliderlabs/herokuish:latest ...
Using docker image sha256:b8d01302796b2474a2ed0caeaa061778e17e9a1552acc3832fe7c92520734263 for gliderlabs/herokuish:latest with digest gliderlabs/herokuish@sha256:4be2ea25fe43e5552b75706c68a1b313f2b9cf6fd7edcf1d629fbf9ed52522e5 ...
Preparing environment
Running on runner-x5k3yrly-project-1-concurrent-0 via gitlab...
Getting source from Git repository
Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /builds/root/my_project/.git/
Checking out 223dbc3f as detached HEAD (ref is master)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
Using docker image sha256:b8d01302796b2474a2ed0caeaa061778e17e9a1552acc3832fe7c92520734263 for gliderlabs/herokuish:latest with digest gliderlabs/herokuish@sha256:4be2ea25fe43e5552b75706c68a1b313f2b9cf6fd7edcf1d629fbf9ed52522e5 ...
$ if [ -z ${KUBERNETES_PORT+x} ]; then # collapsed multi-line command
$ export DATABASE_URL="postgres://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${DB_HOST}:5432/${POSTGRES_DB}"
$ cp -R . /tmp/app
$ /bin/herokuish buildpack test
-----> Go app detected
-----> Fetching jq... done
-----> Fetching stdlib.sh.v8... done
-----> 
       Detected go modules via go.mod
-----> 
       Detected Module Name: github.com/netology-code/sdvps-materials
-----> 
-----> New Go Version, clearing old cache
-----> Installing go1.16.15
-----> Fetching go1.16.15.linux-amd64.tar.gz... done
-----> Determining packages to install
       
       Detected the following main packages to install:
       		github.com/netology-code/sdvps-materials
       
-----> Running: go install -v -tags heroku github.com/netology-code/sdvps-materials 
github.com/netology-code/sdvps-materials
       
       Installed the following binaries:
       		./bin/sdvps-materials
       
       Created a Procfile with the following entries:
       		web: bin/sdvps-materials
       
       If these entries look incomplete or incorrect please create a Procfile with the required entries.
       See https://devcenter.heroku.com/articles/procfile for more details about Procfiles
       
-----> Copying go tool chain to $GOROOT=$HOME/.heroku/go... done
-----> Running Tests With: go test -race -v -mod=readonly ./... | patter
TAP version 13
ok 1 - TestHello (0.00s)
1..1
-----> 
-----> Running Benchmarks With: go test -v -mod=readonly -run=_ -bench=. -benchmem ./...
PASS
ok  	github.com/netology-code/sdvps-materials	0.002s
-----> 
-----> Standard (Non TAP) test output
=== RUN   TestHello
--- PASS: TestHello (0.00s)
PASS
ok  	github.com/netology-code/sdvps-materials	0.020s
-----> 
-----> Finished
-----> 
Job succeeded
```


Но при выполнении build job появилась уже другая ошибка:

![Screenshot_2](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/билд.jpg)


Вывод консоли при выполнении:

```
Running with gitlab-runner 16.9.1 (782c6ecb)
  on gitlab X5k3YRLy, system ID: r_q6ONRml89zQO
Preparing the "docker" executor
00:04
Using Docker executor with image registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image:v1.37.0 ...
Starting service docker:20.10.12-dind ...
Pulling docker image docker:20.10.12-dind ...
Using docker image sha256:1a42336ff683d7dadd320ea6fe9d93a5b101474346302d23f96c9b4546cb414d for docker:20.10.12-dind with digest docker@sha256:6f2ae4a5fd85ccf85cdd829057a34ace894d25d544e5e4d9f2e7109297fedf8d ...
Waiting for services to be up and running (timeout 30 seconds)...
*** WARNING: Service runner-x5k3yrly-project-1-concurrent-0-d2b115a2f3bca0b2-docker-0 probably didn't start properly.
Health check error:
create service container: Error response from daemon: conflicting options: host type networking can't be used with links. This would result in undefined behavior (services.go:208:0s)
Service container logs:
*********
Pulling docker image registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image:v1.37.0 ...
Using docker image sha256:6cb5ddcdac174d9994d415734d25f234260828e955877e807d25f45c7ae77dc4 for registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image:v1.37.0 with digest registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image@sha256:641092d93f20d421493b21e3007f160c8cfb272518997d9d02164e40f0e1d7d7 ...
Preparing environment
00:01
Running on runner-x5k3yrly-project-1-concurrent-0 via gitlab...
Getting source from Git repository
00:00
Fetching changes with git depth set to 20...
Reinitialized existing Git repository in /builds/root/my_project/.git/
Checking out 223dbc3f as detached HEAD (ref is master)...
Skipping Git submodules setup
Executing "step_script" stage of the job script
00:02
Using docker image sha256:6cb5ddcdac174d9994d415734d25f234260828e955877e807d25f45c7ae77dc4 for registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image:v1.37.0 with digest registry.gitlab.com/gitlab-org/cluster-integration/auto-build-image@sha256:641092d93f20d421493b21e3007f160c8cfb272518997d9d02164e40f0e1d7d7 ...
$ if [[ -z "$CI_COMMIT_TAG" ]]; then # collapsed multi-line command
$ /build/build.sh
Logging in to GitLab Dependency proxy with CI credentials...
Login Succeeded
Building Dockerfile-based application...
WARNING: buildx: git was not found in the system. Current commit information was not captured by the build
ERROR: invalid tag "/master:223dbc3f489784448004e020f2ef224f17a7b06d": invalid reference format
Uploading artifacts for failed job
00:00
Uploading artifacts...
WARNING: gl-auto-build-variables.env: no matching files. Ensure that the artifact path is relative to the working directory (/builds/root/my_project) 
ERROR: No files to upload                          
ERROR: Job failed: exit code 1
```

Что лучше всего было бы сделать?

Переустановить докер на более раннюю версию?

Прописать значение переменной окружения DOCKER_BUILDKIT=0?

Или это проблема с плагином buildx?

Я уже совсем не понимаю, что не так настроено 




