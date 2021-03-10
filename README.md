# Ansible_nginx
Deploy by Ansible Nginx webserver on 8080 port



Update 10.03.21
Собрать автозапуск



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
