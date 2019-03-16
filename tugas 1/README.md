# Implementasi MySQL Multi-Node dan ProxySQL sebagai Load Balancer

### A.	Model Arsitektur
Sistem ini terdiri dari sebuah NDB Manager, 3 buah Data Node, 2 buah MySQL API Node, dan sebuah ProxySQL sebagai Load Balancer.
Berikut adalah pembagian IP beserta hostname yang digunakan:
* 192.168.33.10		NDB Manager dan API Node		->	manager
* 192.168.33.11		Data Node dan API Node		->	clusterdb1
* 192.168.33.12		Data Node				->	clusterdb2
* 192.168.33.13		Data Node				->	clusterdb3
* 192.168.33.14		ProxySQL				->	proxy

### B.	Implementasi
##### 1.	Konfigurasi Awal
Konfigurasi awal dilakukan pada Vagrantfile.
Menjalankannya :
```
$ cd (direktori)
$ vagrant up
$ vagrant ssh (nama server)
```

##### 2.	Instalasi dan Konfigurasi Cluster Manager
Masuk pada manager (192.168.33.10) dan men download package berikut :
```
$ cd ~
$ wget https://dev.mysql.com/get/Downloads/MySQL-Cluster-7.6/mysql-cluster-community-management-server_7.6.6-1ubuntu18.04_amd64.deb
```
Menginstall ndb_mgdb :
```
$ sudo dpkg -i mysql-cluster-community-management-server_7.6.6-1ubuntu18.04_amd64.deb
```
Membuat direktori /var/lib/mysql-cluster
```
$ sudo mkdir /var/lib/mysql-cluster
```
Mengedit isi file confg.ini
```
$ sudo nano /var/lib/mysql-cluster/config.ini
```
Berikut isi config ini :
---
Menjalankan mdb_mgmd
```
$ sudo ndb_mgmd -f /var/lib/mysql-cluster/config.ini
```
Mematikan server yang masih berjalan
```
$ sudo pkill -f ndb_mgmd
```
Mengedit ndb_mgmd.service
```
$ sudo nano /etc/systemd/system/ndb_mgmd.service
```
Berikut isinya :
---
Mereload sistem
```
$ sudo systemctl daemon-reload
$ sudo systemctl enable ndb_mgmd
```
Menjalankan service
```
$ sudo systemctl start ndb_mgmd
```
Mengecek status
```
$ sudo systemctl status ndb_mgmd
```
Hasil :
---

Memberikan akses untuk koneksi dari luar
```
$ sudo ufw allow from 192.168.33.10
$ sudo ufw allow from 192.168.33.11
$ sudo ufw allow from 192.168.33.12
$ sudo ufw allow from 192.168.33.13
$ sudo ufw allow from 192.168.33.14
```

##### 3.	Instalasi dan Konfigurasi Data Node
Masuk pada data node / clusterdb (192.168.33.11, 192.168.33.12, 192.168.33.13) dan men download package berikut :
```
$ cd ~
$ wget https://dev.mysql.com/get/Downloads/MySQL-Cluster-7.6/mysql-cluster-community-data-node_7.6.6-1ubuntu18.04_amd64.deb
```
Menginstall dependency :
```
$ sudo apt update
$ sudo apt install libclass-methodmaker-perl
```
Menginstall data node binary :
```
$ sudo dpkg -i mysql-cluster-community-data-node_7.6.6-1ubuntu18.04_amd64.deb
```
Mengedit isi /etc/my.cnf
```
$ sudo nano /etc/my.cnf
```
Berikut isi my.cnf :
---
Membuat direktori :
```
$ sudo mkdir -p /usr/local/mysql/data
```
Memulai data node
```
$ sudo ndbd
```
Hasil :
---
Pada clusterdb1
---
Pada clusterdb2
---
Pada clusterdb3
---
Memberikan akses untuk koneksi dari luar
```
$ sudo ufw allow from 192.168.33.10
$ sudo ufw allow from 192.168.33.11
$ sudo ufw allow from 192.168.33.12
$ sudo ufw allow from 192.168.33.13
$ sudo ufw allow from 192.168.33.14
```
Mematikan proses mdbd masih berjalan
```
$ sudo pkill -f ndbd
```
Mengedit ndbd.service
```
$ sudo nano /etc/systemd/system/ndbd.service
```
Berikut isinya :
---
Mereload sistem
```
$ sudo systemctl daemon-reload
$ sudo systemctl enable ndbd
```
Menjalankan service
```
$ sudo systemctl start ndbd
```
Mengecek status
```
$ sudo systemctl status ndbd
```
Hasil :
---
Pada clusterdb1
---
Pada clusterdb2
---
Pada clusterdb3
---

##### 4.	Konfigurasi MySQL Server dan Client
Masuk pada API node (192.168.33.10, 192.168.33.11) dan mendownload package berikut :
```
$ cd ~
$ wget https://dev.mysql.com/get/Downloads/MySQL-Cluster-7.6/mysql-cluster_7.6.6-1ubuntu18.04_amd64.deb
```
Mengekstrak arsip
```
$ mkdir install
$ tar -xvf mysql-cluster_7.6.6-1ubuntu18.04_amd64.deb-bundle.tar -C install/
```
Menginstall dependency :
```
$ cd install
$ sudo apt update
$ sudo apt install libaiol libmecab2
$ sudo dpkg -i mysql-common_7.6.6-1ubuntu18.04_amd64.deb
$ sudo dpkg -i mysql-cluster-community-client_7.6.6-1ubuntu18.04_amd64.deb
$ sudo dpkg -i mysql-client_7.6.6-1ubuntu18.04_amd64.deb
$ sudo dpkg -i mysql-cluster-community-server_7.6.6-1ubuntu18.04_amd64.deb
$ sudo dpkg -i mysql-server_7.6.6-1ubuntu18.04_amd64.deb
```
Mengedit isi /etc/mysql/my.cnf
```
$ sudo nano /etc/mysql/my.cnf
```
Tambahan isi my.cnf :
---
Merestart sistem
```
$ sudo systemctl restart mysql
$ sudo systemctl enable mysql
```

##### 5.	Verifikasi cluster
Masuk pada API node (192.168.33.10, 192.168.33.11) .

Menjalankan :
```
$ mysql -u root –p
```
Masukkan password : admin

Mengecek status :
```
mysql> SHOW ENGINE NDB STATUS \G
```
Hasil :
---
Mengecek data dan service node yang terhubung
```
$ ndb_mgm
ndb_mgm> SHOW
```
Hasil :
---

##### 6.	Instalasi ProxySQL
Masuk pada proxy (192.168.33.14) , pada direktori /tmp download package berikut :
```
$ cd /tmp
$ curl –OL https://github.com/sysown/proxysql/releases/download/ v1.4.4/proxysql_1.4.4-ubuntu16_amd64.deb
```
Menginstall package :
```
$ sudo dpkg -i proxysql_*
$ rm proxysql_*
$ sudo apt-get update
$ sudo apt-get install mysql-client
```
Menjalankan proxysql
```
$ sudo systemctl start proxysql
```
Mengecek status
```
$ systemctl status proxysql
```
Hasil :
---

##### 7.	Setting password untuk ProxySQL Administrator
Pada proxy, jalankan :

```
$ mysql -u admin -p -h 127.0.0.1 -P 6032 --prompt='ProxySQLAdmin> '

ProxySQLAdmin> UPDATE global_variables SET variable_value= 'admin:password' WHERE variable_name='admin-admin_credentials';
ProxySQLAdmin> LOAD ADMIN VARIABLES TO RUNTIME;
ProxySQLAdmin> SAVE ADMIN VARIABLES TO DISK;
```

##### 8.	Konfigurasi Monitoring di MySQL
Pada salah satu MySQL node, jalankan :
```
$ curl -OL https://gist.github.com/lefred/77ddbde301c72535381ae7af9f968322/raw/5e40b03333a3c148b78aa348fd2cd5b5dbb36e4d/addition_to_sys.sql
$ mysql -u root -p < addition_to_sys.sql
```
Masuk pada MySQL
```
$ mysql -u root –p
```
Membuat user monitor
```
mysql> CREATE USER 'monitor'@'%' IDENTIFIED BY 'monitorpassword';
mysql> GRANT SELECT on sys.* to 'monitor'@'%';
mysql> FLUSH PRIVILEGES;
```

##### 9.	Konfigurasi Monitoring di ProxySQL
Pada proxy, jalankan :
```
ProxySQLAdmin> UPDATE global_variables SET variable_value='monitor' WHERE variable_name='mysql-monitor_username';
ProxySQLAdmin> LOAD MYSQL VARIABLES TO RUNTIME;
ProxySQLAdmin> SAVE MYSQL VARIABLES TO DISK;
```

##### 10.	Menambahkan node MySQL ke server Proxy
Pada proxy, membuat baris baru dengan variable dan value pada mysql_group replication_hostgroups
```
ProxySQLAdmin> INSERT INTO mysql_group_replication_hostgroups (writer_hostgroup, backup_writer_hostgroup, reader_hostgroup, offline_hostgroup, active, max_writers, writer_is_also_reader, max_transactions_behind) VALUES (2, 4, 3, 1, 1, 3, 1, 100);
```
Menambahkan MySQL server (service node)
```
ProxySQLAdmin> INSERT INTO mysql_servers(hostgroup_id, hostname, port) VALUES (2, '192.168.33.10', 3306);
ProxySQLAdmin> INSERT INTO mysql_servers(hostgroup_id, hostname, port) VALUES (2, '192.168.33.11', 3306);
ProxySQLAdmin> LOAD MYSQL VARIABLES TO RUNTIME;
ProxySQLAdmin> SAVE MYSQL VARIABLES TO DISK;
```
Mengecek hostgroup
```
ProxySQLAdmin> SELECT hostgroup_id, hostname, status FROM runtime_mysql_servers;
```
Hasil :
---

##### 11.	Membuat user MySQL
```
mysql> CREATE USER 'bdt'@'%' IDENTIFIED BY 'bdt';
mysql> GRANT ALL PRIVILEGES on test.* to 'bdt'@'%';
mysql> FLUSH PRIVILEGES
mysql> EXIT;
```

##### 12.	Membuat user ProxySQL
```
ProxySQLAdmin> INSERT INTO mysql_users(username, password, default_hostgroup) VALUES ('bdt', 'bdt', 2);
ProxySQLAdmin> LOAD ADMIN VARIABLES TO RUNTIME;
ProxySQLAdmin> SAVE ADMIN VARIABLES TO DISK;
```

### C.	Menyimpan Data pada MySQL Cluster
Masuk pada mysql dengan
```
$ mysql –u root –p
```
Membuat database sakila
```
mysql> create database sakila;
```
Keluar dari mysql dan import schema dan data sakila
```
sudo mysql –u root –p sakila < /vagrant/sakila-mv-schema.sql
sudo mysql –u root –p sakila < /vagrant/sakila-mv-data.sql
```
Malihat daftar database
```
mysql> show databases;
```
Hasil :
---

Melihat daftar table pada database sakila
```
mysql> use sakila;
mysql> show tables;
```
Hasil :
---

### D.	Tes Koneksi Keluar
```
select @@hostname
```
Hasil :
---