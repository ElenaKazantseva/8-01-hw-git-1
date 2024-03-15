# Домашнее задание к занятию "`Ansible.Часть 2`" - `Казанцева Елена`

---

### Задание 1

**Выполните действия, приложите файлы с плейбуками и вывод выполнения.**

Напишите три плейбука. При написании рекомендуем использовать текстовый редактор с подсветкой синтаксиса YAML.

Плейбуки должны: 

1. Скачать какой-либо архив, создать папку для распаковки и распаковать скаченный архив. Например, можете использовать [официальный сайт](https://kafka.apache.org/downloads) и зеркало Apache Kafka. При этом можно скачать как исходный код, так и бинарные файлы, запакованные в архив — в нашем задании не принципиально.
2. Установить пакет tuned из стандартного репозитория вашей ОС. Запустить его, как демон — конфигурационный файл systemd появится автоматически при установке. Добавить tuned в автозагрузку.
3. Изменить приветствие системы (motd) при входе на любое другое. Пожалуйста, в этом задании используйте переменную для задания приветствия. Переменную можно задавать любым удобным способом.


Решение:

Файл inventory для всех плейбуков:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/2%20(4).jpg)



1. Скачать какой-либо архив, создать папку для распаковки и распаковать скаченный архив. При этом можно скачать как исходный код, так и бинарные файлы, запакованные в архив — в нашем задании не принципиально.


Playbook1.yml:

```yaml
- hosts: test_servers
  gather_facts: yes
  become:
    true
  become_method:
    sudo
  become_user:
    root
  remote_user:
    cubic
  tasks:
    - name: Download golang archive
      get_url:
        url: https://dl.google.com/go/go1.19.1.linux-amd64.tar.gz
        dest: /home/cubic
      register: bin_files

    - name: Creates directory
      ansible.builtin.file:
        path: /home/cubic/golang1
        state: directory

    - name: UNZIPPING the files
      unarchive:
        src: /home/cubic/go1.19.1.linux-amd64.tar.gz
        dest: /home/cubic/golang1
        copy: no
      with_items:
      - "{{ bin_files.stdout }}"
```


![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/1%20(2).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/1%20(3).jpg)



2. Установить пакет tuned из стандартного репозитория вашей ОС. Запустить его, как демон — конфигурационный файл systemd появится автоматически при установке. Добавить tuned в автозагрузку.

playbook2.yml

```yaml
- hosts: test_servers
  become:
    true
  become_method:
    sudo
  become_user:
    root
  remote_user:
    cubic
  roles:
   - tuned
```

/etc/ansible/roles/tuned/tasks/main.yml

```yaml
- name: Update and upgrade apt packages
  become: true
  apt:
    update_cache: yes
    cache_valid_time: 86400

- name: Install tuned
  apt:
    name=tuned
    state=present
  notify:
    - tuned systemd
```


/etc/ansible/roles/tuned/handlers/main.yml

```yaml
- name: tuned systemd
  systemd:
    name: tuned
    enabled: yes
    state: started
```

Ход выполнения:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/2%20(5).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/2%20(6).jpg)



3. Изменить приветствие системы (motd) при входе на любое другое. Пожалуйста, в этом задании используйте переменную для задания приветствия. Переменную можно задавать любым удобным способом.

playbook3.yml

```yaml
- hosts: test_servers
  gather_facts: yes
  become:
    true
  become_method:
    sudo
  become_user:
    root
  remote_user:
    cubic
  vars:
    hello1: printf " Hello from Ansible! "
  tasks:
    - name: Add a line to a file
      ansible.builtin.lineinfile:
        path: /etc/update-motd.d/00-header
        line: " {{ hello1 }} "
```


Ход выполнения:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/3.jpg)

---

### Задание 2

**Выполните действия, приложите файлы с модифицированным плейбуком и вывод выполнения.** 

Модифицируйте плейбук из пункта 3, задания 1. В качестве приветствия он должен установить IP-адрес и hostname управляемого хоста, пожелание хорошего дня системному администратору. 

Решение:

playbook3_1.yml

```yaml
- hosts: test_servers
  gather_facts: yes
  become:
    true
  become_method:
    sudo
  become_user:
    root
  remote_user:
    cubic
  tasks:
    - name: find out ip of a host
      shell: dig +short myip.opendns.com @resolver1.opendns.com
      register: ippub

    - set_fact:
        ippub={{ ippub.stdout }}

    - name: Insert/Update ssh-motd
      ansible.builtin.blockinfile:
        path: /etc/update-motd.d/00-header
        backup: yes
        insertafter: " Hello from Ansible! "
        block: |
          printf " Welcome to {{ inventory_hostname }} , {{ ippub }} !
          Have a good day! "

```

Ход выполнения:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/3_1.jpg)

---

### Задание 3

**Выполните действия, приложите архив с ролью и вывод выполнения.**

Ознакомьтесь со статьёй [«Ansible - это вам не bash»](https://habr.com/ru/post/494738/), сделайте соответствующие выводы и не используйте модули **shell** или **command** при выполнении задания.

Создайте плейбук, который будет включать в себя одну, созданную вами роль. Роль должна:

1. Установить веб-сервер Apache на управляемые хосты.
2. Сконфигурировать файл index.html c выводом характеристик каждого компьютера как веб-страницу по умолчанию для Apache. Необходимо включить CPU, RAM, величину первого HDD, IP-адрес.
Используйте [Ansible facts](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_vars_facts.html) и [jinja2-template](https://linuxways.net/centos/how-to-use-the-jinja2-template-in-ansible/). Необходимо реализовать handler: перезапуск Apache только в случае изменения файла конфигурации Apache.
4. Открыть порт 80, если необходимо, запустить сервер и добавить его в автозагрузку.
5. Сделать проверку доступности веб-сайта (ответ 200, модуль uri).

В качестве решения:
- предоставьте плейбук, использующий роль;
- разместите архив созданной роли у себя на Google диске и приложите ссылку на роль в своём решении;
- предоставьте скриншоты выполнения плейбука;
- предоставьте скриншот браузера, отображающего сконфигурированный index.html в качестве сайта.



Решение:

/etc/ansible/inventory/playbook4.yml

```yaml
- hosts: test_servers
  become:
    true
  become_method:
    sudo
  become_user:
    root
  remote_user:
    cubic
  roles:
   - apache2
```


/etc/ansible/roles/apache2/tasks/main.yml

```yaml
- name: Update and upgrade apt packages
  become: true
  apt:
    upgrade: 'yes'
    update_cache: yes
    cache_valid_time: 3400

- name: Install Apache2
  apt:
    name=apache2
    state=latest
  notify:
    - apache2 systemd

- name: Create index.html using Jinja2
  template:
    src: index.j2
    dest: /var/www/html/index.html

- name: set index.html as the default page
  ansible.builtin.lineinfile:
    path: /etc/apache2/apache2.conf
    regexp: '^DirectoryIndex '
    insertafter: '^#DirectoryIndex '
    line: DirectoryIndex index.html
  notify:
    - apache2 restart

- name: Allow all access to tcp port 80
  ufw:
    rule: allow
    port: '80'
    proto: tcp

- name: find out ip of a host
  shell: dig +short myip.opendns.com @resolver1.opendns.com
  register: ippub

- set_fact:
    ippub={{ ippub.stdout }}

- name: Check that you can connect to a page and it returns a status 200
  ansible.builtin.uri:
    url: http://{{ ippub }}

```


/etc/ansible/roles/apache2/handlers/main.yml

```yaml
- name: apache2 systemd
  systemd:
    name: apache2
    enabled: yes
    state: started

- name: apache2 restart
  service:
    name=apache2
    state=restarted

```


/etc/ansible/roles/apache2/templates/index.j2

```yaml
Welcome!

System information:

Hostname: {{ hostvars['cat1']['inventory_hostname'] }}

IP Address: {{ hostvars['cat1']['ansible_facts']['eth0']['ipv4']['address'] }}

CPU: {{ hostvars['cat1']['ansible_processor_vcpus'] }}

RAM: {{ hostvars['cat1']['ansible_memory_mb'] }}

HDD : {{ hostvars['cat1']['ansible_facts']['devices']['vda']['size'] }}



Hostname: {{ hostvars['dog2']['inventory_hostname'] }}

IP Address: {{ hostvars['dog2']['ansible_facts']['eth0']['ipv4']['address'] }}

CPU: {{ hostvars['dog2']['ansible_processor_vcpus'] }}

RAM: {{ hostvars['dog2']['ansible_memory_mb'] }}

HDD : {{ hostvars['dog2']['ansible_facts']['devices']['vda']['size'] }}

```


Ход выполнения:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(1).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(2).jpg)

Скриншоты сайта index.html с обоих хостов:

Да, вышло не очень красиво, но зато все получилось.

Здесь указаны внутренние ip адреса, так как была взята переменная от ansible_facts `{{ hostvars['cat1']['ansible_facts']['eth0']['ipv4']['address'] }}`, как это было необходимо в задании. Ниже прилагаю подтверждение, что данные внутренние ip адреса действительно принадлежат хостам:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(3).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(6).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(5).jpg)

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/4%20(4).jpg)


Файл inventory для всех плейбуков:

![скрин для Git](https://github.com/ElenaKazantseva/homeworks/blob/hw-ansible-2/img/2%20(4).jpg)

Файлы .yml представила сразу здесь в виде кода, потому что мне кажется, так гораздо удобнее и быстрее сразу посмотреть и прочитать.
