# inventory  
target ansible_host=192.168.1.191 ansible_user=user1 ansible_ssh_pass=<ваш_пароль> ansible_become_pass=<ваш_пароль>
target2 ansible_host=192.168.1.126 ansible_user=user1 ansible_ssh_pass=<ваш_пароль> ansible_become_pass=<ваш_пароль>

[web]
target
target2


------------
# настройка ansible 
# 1) редактируем /etc/ssh/ssh_config ( тут раскоментировать нужно
# PasswordAuthentication yes
# и можно ,но не желательно ( если вообще есть )
 
### PermitRootLogin yes
### 2) для удобства поменяем  /etc/hostname --> на нужный и также /etc/hosts ---> убираем все кроме hostname (нового) и localhost
### 3) установить на ansible-controller( это hostname узла ) ---> apt install ansible +++ apt install sshpass ---> если будем входить по паролю, а не по  ssh
# 4) создадим папку например (project) ---> полный путь /host/user1/project ---> добавим там файл inventory 
# 5) root@ansible-controller:/home/user1/project# cat inventory 
# target ansible_host=192.168.157.126 ansible_user=user1 ansible_ssh_pass=<ваш_пароль>
# root@ansible-controller:/home/user1/project#   ---> добавили информацию о  узле ( ansible-target ) в inventory 
# 6) проверим конект  ansible target -m ping -i inventory
# 7) добавили 2 узел в inventory --> главное не забыть про finger_prints или же раскоментировать в /etc/ansible/ansible.cfg  #Host_key_cheking = False
# 9)ansible target -m apt -a "name=nginx state=present" -i inventory -b -K
-b повышение привелегий до sudo  
-K ввести пароль
также нужно чтобы user1 был уже в группе sudo
и можно добавить в inventory ansible_become_pass=<ваш_пароль>

10) обязательно использовать become: yes ---> в playbook чтобы зайти под sudo , в каждой tasks, ну понятоное дело что заходим из под юзера
пример запускаroot@ansible-controller:/home/user1/project# ansible-playbook -i inventory playbook.yml -K

- vars мы для работы -- не переопределяем а default переопределям


--------------------------- тест
# Настройка Ansible

## 1. Редактирование конфигурации SSH
Отредактируйте файл `/etc/ssh/ssh_config`:
- Раскомментируйте строку:
  ```plaintext
  PasswordAuthentication yes
Также можно, но не желательно (если вообще есть), раскомментировать:
plaintext

Копировать код
PermitRootLogin yes
2. Настройка имени хоста
Для удобства измените:

/etc/hostname на нужное имя.
/etc/hosts — уберите все, кроме нового имени хоста и localhost.
3. Установка Ansible на контроллере
На узле Ansible-контроллера выполните:

bash

Копировать код
apt install ansible
apt install sshpass  # Если будете входить по паролю, а не по SSH
4. Создание папки для проекта
Создайте папку, например, project:

bash

Копировать код
mkdir -p /home/user1/project
5. Создание файла inventory
Добавьте файл inventory в созданную папку:

bash

Копировать код
cat <<EOL > /home/user1/project/inventory
target ansible_host=192.168.157.126 ansible_user=user1 ansible_ssh_pass=<ваш_пароль>
EOL
Здесь добавлена информация о узле (ansible-target) в inventory.

6. Проверка соединения
Проверьте соединение с узлом:

bash

Копировать код
ansible target -m ping -i inventory
7. Добавление второго узла в inventory
Добавьте второй узел в inventory. Не забудьте про fingerprints или раскомментируйте в /etc/ansible/ansible.cfg:

plaintext

Копировать код
# host_key_checking = False
8. Установка пакета на целевом узле
Установите пакет nginx на целевом узле:

bash

Копировать код
ansible target -m apt -a "name=nginx state=present" -i inventory -b -K
-b — повышение привилегий до sudo.
-K — введите пароль.
Убедитесь, что user1 уже в группе sudo. Также можно добавить в inventory:

plaintext

Копировать код
ansible_become_pass=<ваш_пароль>
9. Использование become в playbook
Обязательно используйте become: yes в playbook, чтобы зайти под sudo в каждой задаче. Пример запуска:

bash

Копировать код
ansible-playbook -i inventory playbook.yml -K
10. Переменные
Используйте vars для работы — не переопределяйте, а defaults переопределяйте.

Code

Копировать код

Сохраните этот текст в файл с расширением `.md`, и он будет готов к использованию.
