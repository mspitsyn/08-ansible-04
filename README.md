# Домашнее задание к занятию "Работа с roles"

### Предварительно необходимо настроить три вирутальные машины с OC ubuntu-2004-lts.
 В проекте это сделано с помощью terraform. 

## Что делает playbook:

Playbook разворачивает на заданных в файле `inventory/prod.yml` хостах c использованием 'roles' в ansible-plybook следующие приложения:
- сlickhouse-client
- clickhouse-server
- clickhouse-common
- vector
- nginx
- git
- lighthouse

Сначала на отдельную виртуальную машину устанавливается и конфигурируется web-сервер Nginx, т.к. он нужен для работы Lighthouse. Далее на ту же машину скачивается Lighthouse, создается его файл web-конфигурации по указанному шаблону и перезапускается web-сервер Nginx.

На следующую виртуальную машину скачивается дистрибутив clickhouse-server и сlickhouse-client по указанному пути с указанными именами файлов. Устанавливается clickhouse-server и сlickhouse-client, создается база данных, запускается сервис Clickhouse. 

На последнюю виртуальную машину выполняется установка дистрибутива Vector, путь к источнику установки указан в файле vars.yml. Создается сервис приложения по файлу из шаблона. После выполнения действий запускается Vector.

## Запуск проекта 
Для стягивания `roles` перейти в каталог `playbook/` и выполнить команду:
```
ansible-galaxy install -r requirements.yml --roles-path="roles/"
```   
В файле `inventory/prod.yml` необходимо указать IP-адреса машин на который будут разворачиваться сервисы.  
```yaml
clickhouse:
  hosts:
    clickhouse:
      ansible_host: "<your IP address>"
vector:
  hosts:
    vector:
      ansible_host: "<your IP address>"
lighthouse:
  hosts:
    lighthouse:
      ansible_host: "<your IP address>" 
```
Для запуска playbook выполнить команду: 
 ``` 
ansible-playbook -i inventory/prod.yml site.yml 
```
