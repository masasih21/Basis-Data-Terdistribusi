# Implementasi Redis

## Pendahuluan
**Redis** adalah salah satu database NoQSL yang berbasis key-value store. Sistemnya yang in-memory membuat pengambilan data dari Redis menjadi lebih cepat, namun juga persistent bila ingin menyimpan data ke disk. Redis memiliki sejumlah query yang pastinya mudah digunakan untuk menyimpan mulai dari data sederhana hingga data kompleks.

**Keunggulan** Redis :
1. Sangat cepat - Redis bisa melakukan sekitar 110000 SET per detik, sekitar 81000 GET per detik.
2. Mendukung banyak tipe data - Redis secara native mendukung sebagian besar tipe data yang telah diketahui pengembang seperti list, set, sorted, dan hash yang diurutkan.
3. Operasi bersifat atom - Semua operasi Redis bersifat atom, yang memastikan bahwa jika dua klien mengakses secara bersamaan, server Redis akan menerima nilai yang diperbarui.
4. Multi-utility tool - Redis memiliki multi-utility tool dan dapat digunakan dalam sejumlah kasus penggunaan seperti caching, messaging-queues (Redis native mendukung Publish / Subscribe), data singkat dalam aplikasi, seperti web sessions aplikasi, web page hit counts, dll.

Perbedaan dengan database Key-Value store lainnya :
1. Redis adalah jalur evolusi yang berbeda dalam DB-nilai kunci, di mana nilai dapat berisi tipe data yang lebih kompleks, dengan operasi atom yang didefinisikan pada tipe data tersebut.
2. Redis adalah database in-memori tapi tetap pada basis data disk, oleh karena itu merupakan trade off yang berbeda dimana kecepatan tulis dan baca yang sangat tinggi dicapai dengan keterbatasan kumpulan data yang tidak boleh lebih besar dari pada memori.
3. Keuntungan lain dari database memori adalah bahwa representasi memori dari struktur data kompleks jauh lebih sederhana untuk dimanipulasi dibandingkan dengan struktur data yang sama pada disk. Dengan demikian, Redis bisa melakukan banyak hal dengan sedikit kompleksitas internal.

## 1. Arsitektur Server

## 2. Instalasi Redis

## 3. CRUD Data

### a. Create Data
### b. Read Data
### c. Update Data
### d. Delete Data

## 4. Simulasi Fail Over

## Kesimpulan

## Referensi
https://www.kapalomen.com/2017/09/pengenalan-database-redis-keuntungan.html
https://www.codepolitan.com/pdkt-dengan-redis
