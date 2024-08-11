### Управление образами (images).
Скачать образ с облачного реестра и установить его в локальный реестр: 
```bash
docker pull <image-name>
```
Скачать определенную версию образа: 
```bash
docker pull <image-name>:<image-version>
```
Создать(или сначала скачать, если не существует) контейнер на основе образа.
Флаги: 
**-d** - запустить в фоновом режиме;
**-it** - запустить в интерактивном режиме(сразу войти в контейнер);
**--rm** - контейнер будет автоматически удален при его остановке;
**--name** - задать имя контейнеру;
**-p** - задать порты контейнеру в формате <локальный-порт>:<порт-контейнера>
**-e** - задать переменную окружения
**-v** - смонтировать том в формате <локальный-том>:<том-контейнера>
**--net** - указать имя сети, в которой будет запущен контейнер
```bash
docker run <image-name>
```
Создать контейнер на основе определенной версии образа: 
```bash
docker run <image-name>:<image-version>
```
Посмотреть список скачанных или созданных образов:
```bash
docker images
```
Удалить образ(по ID или названию контейнера): 
```bash
docker rmi <image-id>
```
Удалить все образы и контейнеры, которые не запущены в данный момент
Флаги: 
**-a** - выбрать все сущности;
**--volumes** - с этим флагом будут также удалены все тома
```bash
docker system prune -a --volumes
```
Изменить или задать тег образа:
```bash
docker tag <image-id> <repository>:<version>
```
### Управление контейнерами (containers).
Посмотреть список запущенных контейнеров;
Флаги:
**-a** - отобразить абсолютно все контейнеры, даже те, которые не запущены: 
```bash
docker ps
```
Удалить контейнер (по ID или названию контейнера): 
```bash
docker rm <container-id>
```
Удалить сразу несколько контейнеров: 
```bash
docker rm <container-id-first> <container-id-second>
```
Запустить контейнер, если он еще не запущен:
```bash
docker start <container-name>
```
Остановить контейнер:
```bash
docker stop <container-name>
```
Принудительно остановить запущенный контейнер (убить процесс):
```bash
docker kill <container-name>
```
Посмотреть информацию о контейнере: 
```bash
docker inspect <container-id>
```
Посмотреть потребление ресурсов контейнером: 
```bash
docker stats <container-id>
```
Посмотреть логи контейнера
Флаги:
**-f** - смотреть логи в лайв(live) режиме
```bash
docker logs <container-id>
```
Зайти в контейнер, а также запустить оболочку внутри него
Флаги:
**-it** - запустить в интерактивном режиме
```bash
docker exec -it <container-name> /bin/bash 
```
### Управление томами (volumes).
Посмотреть список смонтированных томов: 
```bash
docker volume ls
```
Смонтировать том на локальную машину: 
```bash
docker run -v <local-volume>?:<container-volume> -d <container-name>
```
Создать новый именной том(volume):
```bash
docker volume create <volume-name>
```
Удалить существующий том(volume):
```bash
docker volume rm <volume-name>
```
### Управление сетями (networks).
##### Типы сетей
1) bridge
2) host
3) none 
4) macvlan

Посмотреть список всех сетей: 
```bash
docker network ls
```
Создать новую сеть
Флаги:
**-d** - указать тип сети (host, bridge, null);
**--subnet** - указать адрес сети, например 192.168.10.0/24
**--gateway** - указать шлюз сети, например 192.168.10.1
```bash
docker network create <network-name>
```
Запустить контейнер в пределах указанной сети: 
```bash
docker run --net <network-name> <container-name>
```
Посмотреть информацию о сети: 
```bash
docker network inspect <network-name>
```
Удалить сеть(и): 
```bash
docker network rm <network-name>, ...
```
Подключить запущенный контейнер к существующей сети:
```bash
docker network connect <network-name> <container-name>
```
Отключить запущенный контейнер от указанной сети:
```bash
docker network disconnect <network-id> <container-name>
```
### Описание и запуск Dockerfile.
Запустить(сбилдить) образ по докер файлу.
Флаги:
**-t** - задать тег, например myimage:v1
```bash
docker build .
```
#### Пример файла Dockerfile
```dockerfile
# На базе какого образа построить сервис  
FROM ubuntu:20.04  
# Указать значки, описывающие образ  
LABEL autor=Igorakimy  
LABEL type=beta  
LABEL platform=Github  
  
# Запустить команды во время сборки  
RUN apt-get update && apt-get install nginx -y  
  
# Указать рабочую директорию, в которую будет осуществлен 
# переход после запуска контейнера
WORKDIR /var/www/html/  
  
# Скопировать локальный файл в контейнер  
COPY index.html .  
  
# Указать переменные окружения, которые будут  
# доступны внутри контейнера  
ENV OWNER=Igor  
ENV TYPE=demo  
  
# Выполнить команду, передавая ей параметры при необходимости    
CMD ["nginx", "-g", "daemon off;"]
```
### Описание и работа с docker-compose.
#### Пример docker-compose.yaml файла
```yaml
# Версия docker-compose
version: "3.7"  

# Список сервисов(контейнеров), которые будут созданы
services:  
 # Указать название для сервиса
 server:  
   # Указать образ, на основе которого будет собран контейнер
   image: nginx:stable  
   # Указать конкретное название для контейнера
   container_name: nginx-server
   # Указать, какие тома будут смонтированы из локальной машины
   volumes:  
     - /home/Downloads/docker:/var/www/html
   # Указать переменный окружения
   environment:
     - NGINX_HOST=web.aki.de
     - NGINX_PORT=80
   # Список доступных извне портов
   ports:
     - "80:80"
     - "443:443"
   # Параметр запуска контейнера
   restart: unless-stoped
```