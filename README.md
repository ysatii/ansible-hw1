# Домашнее задание к занятию 2 «Работа с Playbook» Мельник Юрий Александрович 

## Подготовка к выполнению

1. Установите Ansible версии 2.10 или выше.
2. Создайте свой публичный репозиторий на GitHub с произвольным именем.
3. Скачайте Playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.




# Основная часть

## Задания 1.
 Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте значение, которое имеет факт `some_fact` для указанного хоста при выполнении playbook.

## Решение 1.
листинг inventory/test.yml
```
---
  inside:
    hosts:
      localhost:
        ansible_connection: local
```

команда для запуска 
```sh
ansible-playbook ./site.yml -i inventory/test.yml
```  


 ![рис 1](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble1.jpg)  

 *some_fact: 12* в  файле     "playbook/group_vars/all/examp.yml"

## Задания 2.
 Найдите файл с переменными (group_vars), в котором задаётся найденное в первом пункте значение, и поменяйте его на `all default fact`.


## Решение 2.
листинг "playbook/group_vars/all/examp.yml"
```
 some_fact: all default fact
```

команда для запуска 
```sh
ansible-playbook ./site.yml -i inventory/test.yml
```  

 ![рис 2](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble2.jpg)  


## Задание 3. 
Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

## Решение 3.
Запустить контейнер с CentOS 7
```
docker run -dit --name centos7 pycontribs/centos:7 /bin/bash
```

Запустить контейнер с Ubuntu
```
docker run -dit --name ubuntu pycontribs/ubuntu:latest /bin/bash
```

проверить работоспособность контейнеров
```
docker ps -a
```
 ![рис 3](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble3.jpg)  

## Задание 4.
 Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.

## Решение 4.
 команда для запуска 
```sh
ansible-playbook ./site.yml -i inventory/prod.yml
```  

листинг playbook/prod.yml
```
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local
```  

 ![рис 4](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble4.jpg)  


## Задание 5.
Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились значения: для `deb` — `deb default fact`, для `el` — `el default fact`.

## Решение 5.
Команда 
```
cat group_vars/{deb,el}/*
```

ответ 
```
---
some_fact: "deb default fact"
---
some_fact: "el default fact"
```

## Задание 6.
Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

## Решение 6. 
 команда для запуска 
```sh
ansible-playbook ./site.yml -i inventory/prod.yml
```  
 ![рис 5](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble5.jpg)  

## Задание 7. 
При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.

## Решение 7.

```
ansible-vault encrypt group_vars/deb/examp.yml
```

```
ansible-vault encrypt group_vars/el/examp.yml
```

```
cat group_vars/{deb,el}/*
```

 ![рис 6](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble6.jpg)  
 ![рис 7](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble7.jpg)  
 ![рис 8](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble8.jpg)  

## Задание 8. 
 Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.

## Решение 8.
команда для запуска 
```sh
ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pas
```  

опция для запроса пароля --ask-vault-pas

## Задание 9.
 Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.

## Решение 9.
команда для вывода списка плагинов
```
ansible-doc -t connection -l 
```
 ![рис 9](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble9.jpg)  

вывод информации по модулю local
```
ansible-doc -t connection local
```
 
 ![рис 10](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble10.jpg) 


## Задание 10.
 В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.

## Решение 10.

  ![рис 1](https://github.com/ysatii/ansible-hw1/blob/main/img/img_ansble11.jpg) 

11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь, что факты `some_fact` для каждого из хостов определены из верных `group_vars`.
12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.
13. Предоставьте скриншоты результатов запуска команд.

## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот вариант](https://hub.docker.com/r/pycontribs/fedora).
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в ваш личный репозиторий.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
