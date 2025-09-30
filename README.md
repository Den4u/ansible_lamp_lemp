## Ansible установка LA(E)MP стека.<br />
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Apache](https://img.shields.io/badge/apache-%23D42029.svg?style=for-the-badge&logo=apache&logoColor=white)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) 
![MySQL](https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white)
![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)
<br>
### Содержание:
* [Описание](#описание)
* [Стек](#стек)
* [Требования](#требования)
* [Совместимость версий](#совместимость-версий)
* [Структура репозитория](#структура-репозитория)
* [Как запустить сценарий](#как-запустить-сценарий)
* [Проверка](#проверка) 

### Описание:
LA(E)MP - Linux, Apache(Nginx), MySQL, PHP

Репозиторий содержит готовые сценарии (lamp.yml и lemp.yml) для автоматической установки LAMP/LEMP стека на удаленный сервер Ubuntu. Включает базовую настройку UFW и Fail2ban для обеспечения безопасности сервера.

### Стек:
- Операционная система: Ubuntu (20.04, 22.04, 24.04 LTS)
- Веб-сервер: Apache2 / Nginx
- База данных: MySQL
- Язык: PHP
- Безопасность: UFW, Fail2ban


### Требования:
- Ubuntu 20.04/22.04/24.04 LTS
- Ansible (протестировано на Ansible 2.16)
- Python (протестировано на Python 3.8/3.12)
- SSH-доступ: Для пользователя, от имени которого запускается Ansible, должен быть настроен SSH-доступ к целевому хосту. Рекомендуется использовать SSH-ключи.


### Совместимость версий:
Соответствие версий Ubuntu:PHP по умолчанию:
- Ubuntu 20.04 LTS: PHP 7.4
- Ubuntu 22.04 LTS: PHP 8.1
- Ubuntu 24.04 LTS: PHP 8.3 <br>
Необходимо указать версию PHP, определив переменную ver_php в group_vars.


### Структура репозитория:

```
.
├── group_vars/
│   └── my_vm.yml     # Переменные для группы my_vm
├── inventory/
│   └── hosts         # Файл инвентаря с описанием хостов
├── roles/            # Роли
│   ├── apache/       
│   ├── fail2ban/     
│   ├── mysql/        
│   ├── nginx/        
│   ├── php/          
│   └── ufw/         
├── ansible.cfg       # Конфигурация Ansible
├── lamp.yml          # Плейбук для установки LAMP
├── lemp.yml          # Плейбук для установки LEMP
├── README.md         # Описание (этот файл)
└── secrets.yml       # Зашифрованный файл с секретами (создается пользователем)
```

### Как запустить сценарий:

1. Клонируйте репозиторий:
```
git clone git@github.com:Den4u/ansible_lamp_lemp.git
```

2. Настройте файл инвентаря inventory/hosts:
Укажите IP-адрес или доменное имя вашего удаленного сервера и пользователя SSH.
```
[my_vm]
host_name ansible_host=your_server_ip_or_domain ansible_user=your_ssh_user
```

3. Проверьте доступность сервера:
```
ansible my_vm -m ping
```
4. Создайте зашифрованный файл с секретами в корне репозитория:
```
ansible-vault create secrets.yml
```
Введите надежный пароль для Vault. В файл добавьте:
```
ansible_become_pass: "your_sudo_passwd"
```
Замените your_sudo_passwd на ваш реальный пароль sudo.

5. Настройте переменные в groups_var.

6. Запустите плейбук.<br>
Для LAMP:
```
ansible-playbook lamp.yml --ask-vault-pass
```
 Для LEMP:
```
ansible-playbook lemp.yml --ask-vault-pass
```


### Проверка:

- Откройте веб-браузер и перейдите по адресу вашего сервера: http://your_server_ip_or_domain
- Проверьте статус сервисов mysql, fail2ban:
```
sudo systemctl status mysql
sudo systemctl status fail2ban
``` 


### Автор: [Den4u](https://github.com/Den4u)
