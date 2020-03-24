# CNA350

Real World Project

First Step you have install commands in your VM

sudo apt install docker.io sudo apt install docker-compose sudo apt mariadb-client

#sudo apt install docker

#sudo apt install docker-compose

#sudo apt install mariadb-server

Fork maxscale repository

git clone the maxscale repo

git clone https://github.com/gustanik/CNA350

git the right directory or cd into maxscale

cd CNA350 cd maxscale

docker-compose up -d --build

#output Starting maxscale_master2_1 ... Starting maxscale_master_1 ... Starting maxscale_master2_1 Starting maxscale_master2_1
... done Starting maxscale_slave2_1 ... Starting maxscale_master_1 ... done Starting maxscale_slave1_1 ... Starting
maxscale_slave2_1 ... done Starting maxscale_maxscale_1 ... Starting maxscale_maxscale_1 ... done phpmyadmin is up-to-date

docker-compose ps

  Name                   Command            State            Ports      
--------------------------------------------------------------------------------
     
maxscale_master2_1 docker-entrypoint.sh Up 0.0.0.0:4003->3306/tcp mysql ...
maxscale_master_1 docker-entrypoint.sh Up 0.0.0.0:4001->3306/tcp mysql ...
maxscale_maxscale_1 docker-entrypoint.sh Up 0.0.0.0:3306->3306/tcp, maxsc ... 0.0.0.0:4006->4006/tcp, 0.0.0.0:4007-
>4007/tcp, 0.0.0.0:8989->8989/tcp maxscale_slave1_1 docker-entrypoint.sh Up 0.0.0.0:4002->3306/tcp mysql ...
maxscale_slave2_1 docker-entrypoint.sh Up 0.0.0.0:4004->3306/tcp mysql ...
phpmyadmin /docker-entrypoint.sh Up 0.0.0.0:8080->80/tcp
apac ...

making sure all the server runing properly

docker-compose exec maxscale maxctrl list servers

─────────┬───────────┬──────┬─────────────┬─────────────
────┬───────────┐ │ Server │ Address │ Port │ Connections │ State │ GTID │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ server1 │ master │ 3306 │ 0 │ Master, Running │ 0-3000-32 │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ server2 │ slave1 │ 3306 │ 0 │ Slave, Running │ 0-3000-32 │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ server3 │ master2 │ 3306 │ 0 │ Master, Running │ 0-3002-31 │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ server4 │ slave2 │ 3306 │ 0 │ Slave, Running │ 0-3002-31 │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ Shard-A │ 127.0.0.1 │ 4006 │ 0 │ Running │ │
├─────────┼───────────┼──────┼─────────────┼────────────
─────┼───────────┤ │ Shard-B │ 127.0.0.1 │ 4007 │ 0 │ Running │ │
└─────────┴───────────┴──────┴─────────────┴─────────────────┴───────────┘

you have to make sure all the databases exis is in subdirectory

/CNA350/maxscale/sql/shard-A/master$ mysql -umaxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "show databases" +--------------
------+ | Database | +--------------------+ | mysql | | information_schema | | performance_schema | | zipcodes_one | |
zipcodes_two | +--------------------+

access the master shard database zipcode one

:~/CNA350/maxscale/sql/shard-A/master$ mysql -umaxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM
zipcodes_two.zipcodes_two LIMIT 10;"+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City | State | LocationType | Coord_Lat | Coord_Long | Location | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| 42040 | STANDARD | FARMINGTON | KY | PRIMARY | 36.67 | -88.53 | NA-US-KY-FARMINGTON | FALSE | 465 | 896 | 11562973 |
| 41524 | STANDARD | FEDSCREEK | KY | PRIMARY | 37.4 | -82.24 | NA-US-KY-FEDSCREEK | FALSE | | | |
| 42533 | STANDARD | FERGUSON | KY | PRIMARY | 37.06 | -84.59 | NA-US-KY-FERGUSON | FALSE | 429 | 761 | 9555412 |
| 40022 | STANDARD | FINCHVILLE | KY | PRIMARY | 38.15 | -85.31 | NA-US-KY-FINCHVILLE | FALSE | 437 | 839 | 19909942 |
| 40023 | STANDARD | FISHERVILLE | KY | PRIMARY | 38.16 | -85.42 | NA-US-KY-FISHERVILLE | FALSE | 1884 | 3733 | 113020684 |
| 41743 | PO BOX | FISTY | KY | PRIMARY | 37.33 | -83.1 | NA-US-KY-FISTY | FALSE | | | |
| 41219 | STANDARD | FLATGAP | KY | PRIMARY | 37.93 | -82.88 | NA-US-KY-FLATGAP | FALSE | 708 | 1397 | 20395667 |
| 40935 | STANDARD | FLAT LICK | KY | PRIMARY | 36.82 | -83.76 | NA-US-KY-FLAT LICK | FALSE | 752 | 1477 | 14267237 |
| 40997 | STANDARD | WALKER | KY | PRIMARY | 36.88 | -83.71 | NA-US-KY-WALKER | FALSE | | | |
| 41139 | STANDARD | FLATWOODS | KY | PRIMARY | 38.51 | -82.72 | NA-US-KY-FLATWOODS | FALSE | 3692 | 6748 | 121902277 |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+


To access a master shard database from zipcode two:

/CNA350/maxscale$ mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_two.zipcodes_two LIMIT 10;"

It gives me the output:

+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City | State | LocationType | Coord_Lat | Coord_Long | Location | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| 42040 | STANDARD | FARMINGTON | KY | PRIMARY | 36.67 | -88.53 | NA-US-KY-FARMINGTON | FALSE | 465 | 896 | 11562973 |
| 41524 | STANDARD | FEDSCREEK | KY | PRIMARY | 37.4 | -82.24 | NA-US-KY-FEDSCREEK | FALSE | | | |
| 42533 | STANDARD | FERGUSON | KY | PRIMARY | 37.06 | -84.59 | NA-US-KY-FERGUSON | FALSE | 429 | 761 | 9555412 |
| 40022 | STANDARD | FINCHVILLE | KY | PRIMARY | 38.15 | -85.31 | NA-US-KY-FINCHVILLE | FALSE | 437 | 839 | 19909942 |
| 40023 | STANDARD | FISHERVILLE | KY | PRIMARY | 38.16 | -85.42 | NA-US-KY-FISHERVILLE | FALSE | 1884 | 3733 | 113020684 |
| 41743 | PO BOX | FISTY | KY | PRIMARY | 37.33 | -83.1 | NA-US-KY-FISTY | FALSE | | | |
| 41219 | STANDARD | FLATGAP | KY | PRIMARY | 37.93 | -82.88 | NA-US-KY-FLATGAP | FALSE | 708 | 1397 | 20395667 |
| 40935 | STANDARD | FLAT LICK | KY | PRIMARY | 36.82 | -83.76 | NA-US-KY-FLAT LICK | FALSE | 752 | 1477 | 14267237 |
| 40997 | STANDARD | WALKER | KY | PRIMARY | 36.88 | -83.71 | NA-US-KY-WALKER | FALSE | | | |
| 41139 | STANDARD | FLATWOODS | KY | PRIMARY | 38.51 | -82.72 | NA-US-KY-FLATWOODS | FALSE | 3692 | 6748 | 121902277 |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+

"And thats it, it's all done"

"ABDUl and GABE help me"
