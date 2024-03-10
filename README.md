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


В логах почему-то стоит неактуальный ip-адрес, хотя во всех настройках он исправлен на актуальный и сервисы перезапущены. 
Никак не получается настроить работу раннера, чтобы он выполнил свои задания.
Пожалуйста, помогите
 
