# Wordpress dengan MySQL Multi-Node, ProxySQL sebagai Load Balancer, dan Redis Cluster untuk Cache

## A.	Model Arsitektur
Sistem ini terdiri dari sebuah NDB Manager, 2 buah Data Node, 2 buah MySQL API Node, sebuah ProxySQL sebagai Load Balancer, serta master dan 2 slave redis.
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

### 4.
### 5.

## Referensi
https://wordpress.org/plugins/redis-cache/
https://www.cloudways.com/blog/install-redis-cache-wordpress/
https://www.digitalocean.com/community/tutorials/how-to-configure-redis-caching-to-speed-up-wordpress-on-ubuntu-14-04
https://blog.serverdensity.com/monitor-redis/
