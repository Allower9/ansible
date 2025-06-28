Стенд:
target -- подопытный
target2  -- подопыный
ansible-controller на нём ansible 

# inventory  
### target ansible_host=192.168.1.191 ansible_user=user1 ansible_ssh_pass=<ваш_пароль> ansible_become_pass=<ваш_пароль>
### target2 ansible_host=192.168.1.126 ansible_user=user1 ansible_ssh_pass=<ваш_пароль> ansible_become_pass=<ваш_пароль>

[web]
- target
- target2


------------
# настройка ansible в Virtual box 
Перед началом должна уже быть поднята система. Из интерфейсов должен быть сетевой мост или обычный nat, но c двустороннем буфером. Так как я делаю на mac os --> двустороннего дуфера у меня нету :( 
### 1) редактируем /etc/ssh/ssh_config ( тут раскоментировать нужно `PasswordAuthentication yes` )
### и можно `PermitRootLogin yes` ( если вообще есть ) ,но не желательно особенно для прода! ---> это все делаем на vm, чтобы после по паролю подключиться через терминал mac os 
 

### 2) для удобства поменяем  /etc/hostname --> на нужный и также /etc/hosts ---> убираем все кроме hostname (нового) и localhost
### 3) установить на ansible-controller( это hostname узла ) ---> `apt install ansible` +++ `apt install sshpass` ---> если будем входить по паролю, а не по  ssh
###  4) создадим папку например (project) ---> полный путь /host/user1/project ---> добавим там файл inventory 
###  5) root@ansible-controller:/home/user1/project# cat inventory 
###  target ansible_host=192.168.157.126 ansible_user=user1 ansible_ssh_pass=<ваш_пароль>
### root@ansible-controller:/home/user1/project#   ---> добавили информацию о  узле ( ansible-target ) в inventory 
###  6) проверим конект  ansible target -m ping -i inventory
###  7) добавили 2 узел в inventory --> главное не забыть про finger_prints или же раскоментировать в /etc/ansible/ansible.cfg  #Host_key_cheking = False
###  9)ansible target -m apt -a "name=nginx state=present" -i inventory -b -K
` -b повышение привелегий до sudo  
-K ввести пароль
также нужно чтобы user1 был уже в группе sudo
и можно добавить в inventory ansible_become_pass=<ваш_пароль> `

10) обязательно использовать become: yes ---> в playbook чтобы зайти под sudo , в каждой tasks, ну понятоное дело что заходим из под юзера
пример запускаroot@ansible-controller:/home/user1/project# ansible-playbook -i inventory playbook.yml -K

`- vars мы для работы -- не переопределяем а default переопределям `


