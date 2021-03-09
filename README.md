# Ansible_nginx
Deploy by Ansible Nginx webserver on 8080 port



### Usefull command:

Копирование файла на сервер (-b с правами sudo)
```
ansible nginx -m copy -a "src=hi.txt dest=/home/vagrant" 
ansible nginx -m copy -a "src=hi.txt dest=/home mode=777" -b 
```

Удаление файла с сервера:
```
ansible nginx -m file -a "path=/home/vagrant/hi.txt state=absent"
```
