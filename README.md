# Ansible_nginx
Deploy by Vagrant + Ansible (Playbook/Role) Nginx webserver on 8080 port

Подготовлен стенд на Vagrant с одним сервером. На этом сервере используя Ansible разворачивается nginx со следующими условиями:
- Используется модуль yum (RedHat Linux)
- Конфигурационные файлы берутся из шаблона jinja2 с перемененными (templates)
- После установки nginx должен быть в режиме enabled в systemd
- Используется notify для старта nginx после установки
- Сайт слушает на нестандартном порту - 8080 (сделано через переменные в Ansible)

Для запуска требуется установленный Ansible на хосте. 

Директории:
1. Nginx через Ansible Playbook
2. Nginx через Ansible Role


### Usefull command:

Отладка/Подробности выполнения команд (min -v; max -vvvv)
```
ansible nginx -m command -a "ls /var/log" -vv
```

Вывод всех переменных:
```
ansible all -m setup  
```

Копирование файла на сервер (-b с правами sudo)
```
ansible nginx -m copy -a "src=hi.txt dest=/home/vagrant" 
ansible nginx -m copy -a "src=hi.txt dest=/home mode=777" -b 
```

Удаление файла с сервера:
```
ansible nginx -m file -a "path=/home/vagrant/hi.txt state=absent"
```

Скачать из Интернета:
```
ansible nginx -m get_url -a "url=___ dest=/home"
```

Установка/удаление apache web server:
```
ansible nginx -m yum -a "name=httpd state=latest" -b
ansible nginx -m yum -a "name=httpd state=removed" -b
```

Добавление в автозапуск + загрузка при запуске:
```
ansible nginx -m service -a "name=httpd state=started enabled=yes" -b
```

### Playbook'и:
```
ansible-playbook nginx.yml
```

#### Block with conditions:
```
--- 
 - name: Install Apache
   hosts: all
   become: true
   
   tasks: 
    - block:   # ====Block for REDHAT=====
        
        - name: Install Apache Web Server
          yum: name=httpd state=latest
         
        - name: Start Web Server service for Redhat
          service: name=httpd state=started enabled=yes
          
      when: ansible_os_family == "RedHat"
  
```

#### Loops:
Установка нескольких пакетов: 
```
---
 - name: Install some packages
   yum: name={{ item }} state=installed
   with_items:
       - python3
       - tree
       - vim

```

### Roles
Создание каталога с ролями:
```
ansible-galaxy init deploy_nginx

deploy_nginx
    ├── defaults
    │   └── main.yml
    ├── files
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── README.md
    ├── tasks
    │   └── main.yml
    ├── templates
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main.yml

```
Про пути до файлов: 
Copy всегда берутся из директории Files.
Templates из templates.
