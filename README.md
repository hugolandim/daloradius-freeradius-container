# daloradius-freeradius-container
 <h2>A containerized Daloradius + Freeradius</h2>
 
 </p>
    <h3>Docker image for Daloradius based on Ubuntu 20.04 LTS</h3>
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


 
used for version control
standard value: America/Sao_Paulo - <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones"> See the list of timezones</a>
<hr size="100" width="100%" color="red"> 
 
<h2>DALO_VERSION V1.3</h2>


<hr size="100" width="100%" color="red"> 
 Docker-compose example

If you are using armhf you have to change the MariaDB image.
 
```yaml
version: "3"
services:
  radius:
    image: hugolandim/daloradius-freeradius-container:latest #you need to change the tag to your arch and the desired version
    container_name: radius
    restart: always
    depends_on:
      - "radiusmysql" 
    ports:
      - '1812:1812/udp'
      - '1813:1813/udp'
      - '80:80'
    environment:
      - MYSQL_HOST=radiusmysql
      - MYSQL_PORT=3306
      - MYSQL_DATABASE=radius
      - MYSQL_USER=radius
      - MYSQL_PASSWORD=rddbpass
    networks:
      - radius
    privileged: 'true'
 
  radiusmysql:
    image: mariadb:10.5
    container_name: radiusmysql
    restart: always
    environment:
      - MYSQL_DATABASE=radius
      - MYSQL_USER=radius
      - MYSQL_PASSWORD=rdbdpass
      - MYSQL_ROOT_PASSWORD=dalorootpass
    volumes:
      - ".
    networks:
     - radius
    privileged: 'true'
 
networks:
  radius:
    driver: bridge
 ```
 ---
 <h2>Inside the container <strong>radius</strong></h2>
 
 Edit the file vim /etc/freeradius/3.0/mods-config/sql/main/mysql/setup.sql like an example below:
 ---
 
 ```sql
 
 # -*- text -*-
##
## admin.sql -- MySQL commands for creating the RADIUS user.
##
##      WARNING: You should change 'localhost' and 'radpass'
##               to something else.  Also update raddb/mods-available/sql
##               with the new RADIUS password.
##
##      $Id: f0453e179a6721c5675f6d72ad30a97e1ccb48fa $

#
#  Create default administrator for RADIUS
#
CREATE USER 'radius'@'radiusmysql';
SET PASSWORD FOR 'radius'@'radiusmysql' = PASSWORD('rddbpass');

# The server can read any table in SQL
GRANT SELECT ON radius.* TO 'rd'@'radiusmysql';

# The server can write to the accounting and post-auth logging table.
#
#  i.e.
GRANT ALL on radius.radacct TO 'radius'@'radiusmysql';
GRANT ALL on radius.radpostauth TO 'radius'@'radiusmysql';
~                                                                                                                                                                                  
~                                                                                                                                                                                  
~                                                                                                                                                                                  
~                                                                                                                                                                                  
~                                                                                                                                                                                  
-- INSERT --  
 ```
 ---
 <p>Commands to configure MariaDB Server
<p>sudo mysql_secure_installation

<p>SET PASSWORD FOR 'radius' = PASSWORD('rddbpass');
<p>CREATE USER 'radius';
<p>GRANT SELECT ON radius.* TO 'radius';
<p>GRANT ALL PRIVILEGES ON *.* TO 'radius'@172.27.0.3 IDENTIFIED BY 'rddbpass';
<p>GRANT ALL on radius.radacct TO 'radius';
<p>GRANT ALL on radius.radpostauth TO 'radius';
<p>flush privileges;
<p> teste do acesso remoto: SELECT User, Host FROM mysql.user WHERE Host <> 'localhost';
+--------+------------+
| User   | Host       |
+--------+------------+
| radius | %          |
| root   | %          |
| radius | 172.27.0.3 |
+--------+------------+
3 rows in set (0.003 sec)
