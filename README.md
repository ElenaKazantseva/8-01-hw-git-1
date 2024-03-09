# Домашнее задание к занятию "`GitLab`" - `Казанцева Елена`
---

### Задание 1

**Что нужно сделать:**

1. Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в [этом репозитории](https://github.com/netology-code/sdvps-materials/tree/main/gitlab).   
2. Создайте новый проект и пустой репозиторий в нём.
3. Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

Решение.
Настройки раннера в проекте:

[ссылка на мое решение](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(2).jpg)

[ссылка на мое решение](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(3).jpg)

[ссылка на мое решение](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/1%20(1).jpg)

[ссылка на мое решение](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_71%20(1).jpg)


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


Получилось скачать репозиторий с го-файлами на ВМ 
 `[image: golang:1.19.1](https://github.com/netology-code/sdvps-materials)` ,
и затем запушить его и docker-ci.yml в мой удаленный репозиторий gitlab1  

 [ссылка на мое решение](https://github.com/ElenaKazantseva/homeworks/blob/hw-gitlab-1/img/Screenshot_71%20(8).jpg)


 

 
