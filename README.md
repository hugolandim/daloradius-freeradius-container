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
 <p><font type=bold>################################################</font><p>
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

standard value: America/Sao_Paulo - see List of tz time zones <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones"> See the list of timezones</a>

