# daloradius-freeradius-container
 <h2>A containerized Daloradius + Freeradius</h2fre>
 
 </p>
    <h3>Docker image for Daloradius based on Ubuntu 20.04 LTS
   <p> Services includes:
    <p> - freeradius 3, 
    <p> - Apache
    <p> - php
    <p> - MariaDB-client (You need a separate container with MariaDB service. See the docker-compose below)
   <p> DaloRadius' Credentials:
    <p> User: administrator <p>Password: radius
 <p><span font-style:"bolded">################################################</span><p>
 Environment variables
MYSQL_USER

standard value: radius
MYSQL_PASSWORD

standard value: dalodbpass
MYSQL_HOST

standard value: localhost
MYSQL_PORT

standard value: 3306
MYSQL_DATABASE

standard value: radius
TZ

standard value: America/Sao_Paulo - <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones"> See the list of timezones</a>
<hr size="100" width="100%" color="red">


DALO_VERSION

used for version control
standard value: America/Sao_Paulo - <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones"> See the list of timezones</a>
<hr size="100" width="100%" color="red"> 
 Docker-compose example

If you are using armhf you have to change the MariaDB image. I have provided an example below as a comment.
 
 
 version: "3"
services:
  radius:
    image: frauhottelmann/daloradius-docker:tag #you need to change the tag to your arch and the desired version
    container_name: radius
    restart: always
    depends_on:
      - "radius-mysql" 
    ports:
      - '1812:1812/udp'
      - '1813:1813/udp'
      - '80:80'
    environment:
      - MYSQL_HOST=radius-mysql
      - MYSQL_PORT=3306
      - MYSQL_DATABASE=radius
      - MYSQL_USER=radius
      - MYSQL_PASSWORD=dalodbpass
  radius-mysql:
    image: mariadb:10.3 # use image: linuxserver/mariadb:arm32v7-110.3.18mariabionic-ls37 for RaspberryPi
    container_name: radius-mysql
    restart: always
    environment:
      - MYSQL_DATABASE=radius
      - MYSQL_USER=radius
      - MYSQL_PASSWORD=dalodbpass
      - MYSQL_ROOT_PASSWORD=dalorootpass
    volumes:
      - "./radius-mysql:/var/lib/mysql"
