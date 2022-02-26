# daloradius-freeradius-container
 <h2>A containerized Daloradius + Freeradius</h2fre>
 
 </p>
    <h3>Docker image for Daloradius based on Ubuntu 20.04 LTS
   <p> Services includes:
    <p> - freeradius 3, 
    <p> - Apache
    <p> - php
    <p> - MariaDB-client
   <p> You need a separate container with MariaDB service.
   <p> DaloRadius' Credentials:
    <p> User: administrator <p>Password: radius
 <font type=bold>################################################</font><p>
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

standard value: Europe/Berlin - see List of tz time zones

