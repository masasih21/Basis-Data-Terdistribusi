# Wordpress dengan MySQL Multi-Node, ProxySQL sebagai Load Balancer, dan Redis Cluster untuk Cache

## A.	Model Arsitektur
Sistem ini terdiri dari sebuah NDB Manager, 2 buah Data Node, 2 buah MySQL API Node, sebuah ProxySQL sebagai Load Balancer, serta sebuah Redis Master dan 2 buah Redis Slave.
Berikut adalah pembagian IP beserta hostname yang digunakan:

| IP            | Nama                              | hostname  |
|---------------|-----------------------------------|-----------|
| 192.168.33.10	|	NDB Manager, Redis Master	        | manager   |
| 192.168.33.11	|	Data Node, API Node, Redis Slave  | data1     |
| 192.168.33.12	|	Data Node, API Node, Redis Slave  | data2     |
| 192.168.33.13	|	ProxySQL				                  | proxy     |

## B.	Implementasi
### 1. Konfigurasi MySQL Multi-Node dan ProxySQL
Ikuti langkah pada tutorial berikut [tugas 1](https://github.com/masasih21/Basis-Data-Terdistribusi/tree/master/tugas%201)

### 2. Konfigurasi Wordpress dkk
Ikuti langkah pada tutorial berikut [tugas ETS](https://github.com/masasih21/Basis-Data-Terdistribusi/tree/master/tugas%20ETS)

### 3. Konfigurasi Redis
Ikuti langkah pada tutorial berikut [tugas 6](https://github.com/masasih21/Basis-Data-Terdistribusi/tree/master/tugas%206)

### 4. Konfigurasi Redis Cache pada Wordpress
Yang harus dilakukan pertama kali adalah menginstall plugin  ```Redis Object Cache```
```
Plugins > Add New > Redis Object Cache > Install Now > Activate
```

Buka ```wp-config.php``` pada proxy
```
$ sudo nano wp-config.php
```
Tambahkan baris berikut
```
define( 'WP_REDIS_CLIENT', 'predis' );
define( 'WP_REDIS_SENTINEL', 'mymaster' );
define( 'WP_REDIS_SERVERS', [
    'tcp://192.168.33.10:26379?database=15&alias=master',
    'tcp://192.168.33.11:26379?database=15&alias=slave1',
    'tcp://192.168.33.12:26379?database=15&alias=slave2',
] );

define('WP_CACHE_KEY_SALT', 'example.com');
define('WP_CACHE', true);
```
Keterangan:
- ```WP_CACHE_KEY_SALT``` mengatur awalan untuk semua kunci cache.
- ```WP_CACHE``` membuat cache persistent plugin

### 5.

## Referensi
https://wordpress.org/plugins/redis-cache/
https://www.cloudways.com/blog/install-redis-cache-wordpress/
https://www.digitalocean.com/community/tutorials/how-to-configure-redis-caching-to-speed-up-wordpress-on-ubuntu-14-04
https://blog.serverdensity.com/monitor-redis/
