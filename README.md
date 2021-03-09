# Ansible_nginx
Deploy by Ansible Nginx webserver on 8080 port



### Usefull command:

Отладка/Подробности выполнения команд (min -v; max -vvv)
```
ansible nginx -m command -a "ls /var/log" -vv
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



