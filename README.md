# Jarkom-Modul-2-IT11-2023

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Dyas Amorita Radhwa Nashirah (5027211009)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Midyanisa Yuniar (5027211025)

[Resources Modul 2](https://drive.google.com/drive/folders/15Wr1eTQqn_vZzqkTXEAKF7tgULsxkxfe?usp=sharing)

&nbsp;&nbsp;1. [Soal 1](#soal-1-topologi)

&nbsp;&nbsp;2. [Soal 2](#soal-2)

&nbsp;&nbsp;3. [Soal 3](#soal-3)

&nbsp;&nbsp;4. [Soal 4](#soal-4)

&nbsp;&nbsp;5. [Soal 5](#soal-5)

&nbsp;&nbsp;6. [Soal 6](#soal-6)

&nbsp;&nbsp;7. [Soal 7](#soal-7)

&nbsp;&nbsp;8. [Soal 8](#soal-8)

&nbsp;&nbsp;9. [Soal 9](#soal-9)

&nbsp;&nbsp;10. [Soal 10](#soal-10)

&nbsp;&nbsp;11. [Soal 11](#soal-11)

&nbsp;&nbsp;12. [Soal 12](#soal-12)

&nbsp;&nbsp;13. [Soal 13](#soal-13)

&nbsp;&nbsp;14. [Soal 14](#soal-14)

&nbsp;&nbsp;15. [Soal 15](#soal-15)

&nbsp;&nbsp;16. [Soal 16](#soal-16)

&nbsp;&nbsp;16. [Soal 16](#soal-16)

&nbsp;&nbsp;17. [Soal 17](#soal-17)

&nbsp;&nbsp;18. [Soal 18](#soal-18)

&nbsp;&nbsp;19. [Soal 19](#soal-19)

&nbsp;&nbsp;20. [Soal 20](#soal-20)

## Soal 1 Topologi

Soal :
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian [sebagai berikut](https://docs.google.com/spreadsheets/d/1OqwQblR_mXurPI4gEGqUe7v0LSr1yJViGVEzpMEm2e8/edit?usp=sharing). Folder topologi dapat diakses pada [drive berikut](https://drive.google.com/drive/folders/1Ij9J1HdIW4yyPEoDqU1kAwTn_iIxg3gk?usp=sharing).

Network configuration :

**Pandudewanata**

```
# config for eth0
auto eth0
iface eth0 inet dhcp

# config for eth1
auto eth1
iface eth1 inet static
	address 10.69.1.1
	netmask 255.255.255.0

# config for eth2
auto eth2
iface eth2 inet static
	address 10.69.2.1
	netmask 255.255.255.0

# config for eth3
auto eth3
iface eth3 inet static
	address 10.69.3.1
	netmask 255.255.255.0
```

**Yudhistira: DNS Master**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.1.2
	netmask 255.255.255.0
	gateway 10.69.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Nakula: Client**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.1.3
	netmask 255.255.255.0
	gateway 10.69.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Werkudara: DNS Slave**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.2.2
	netmask 255.255.255.0
	gateway 10.69.2.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Sadewa: Client**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.2.3
	netmask 255.255.255.0
	gateway 10.69.2.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Prabukusuma: Web Server**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.3.2
	netmask 255.255.255.0
	gateway 10.69.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Abimanyu: Web Server**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.3.3
	netmask 255.255.255.0
	gateway 10.69.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Wisanggeni: Web Server**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.3.4
	netmask 255.255.255.0
	gateway 10.69.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**Arjuna: Load Balancer**

```
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.69.3.5
	netmask 255.255.255.0
	gateway 10.69.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

## Soal 2 

Soal :

Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Lakukan telnet pada node Yudhistira dan jalankan perintah berikut

```
apt-get update
apt-get install bind9 dnsutils -y
```

Menambahkan konfigurasi dibawah pada **/etc/bind/named.conf.local**

```
zone "arjuna.it11.com" {
    type master;
    file "/etc/bind/zones/arjuna.it11.com.zone";
};
```

Membuat folder zones dan memasukkan konfigurasi ke dalam file **/etc/bind/zones/arjuna.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.it11.com. root.arjuna.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.it11.com.
; DNS Records
@           IN      A       10.69.3.5 ; IP Utama
www         IN      CNAME   arjuna.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

Pengecekan dapat dilakukan dengan 2 cara, yaitu:

1. Dengan nslookup

![nslookup arjuna.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/242d3b89-7201-4b60-90f5-8f922b685e41)

2. Lakukan ping pada nodes client ( Nakula / Sadewa )

Tambahkan IP Yudhistira pada /etc/resolv.conf

```
nameserver 10.69.1.2
```

Lakukan ping pada nodes client

![ping arjuna.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/d94dafce-0e5f-4da5-8979-cc95910b46bd)

## Soal 3 

Soal :

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Menambahkan konfigurasi dibawah pada **/etc/bind/named.conf.local**

```
zone "abimanyu.it11.com" {
    type master;
    file "/etc/bind/zones/abimanyu.it11.com.zone";
};
```

Membuat folder zones dan memasukkan konfigurasi ke dalam file **/etc/bind/zones/abimanyu.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.it11.com. root.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@             IN      NS      abimanyu.it11.com.
@             IN      A       10.69.3.3 ; IP abimanyu
www           IN      CNAME   abimanyu.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

Pengecekan dapat dilakukan dengan 2 cara, yaitu:

1. Dengan nslookup

![nslookup abimanyu.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/973109c3-ac27-443c-b1ee-9c08efea3cb1)

2. Lakukan ping pada nodes client ( Nakula / Sadewa )

Tambahkan IP Yudhistira pada /etc/resolv.conf

```
nameserver 10.69.1.2
```

Lakukan ping pada nodes client

![ping abimanyu.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/ca3d2726-6784-401d-befa-ddc1e18c9c2e)

## Soal 4

Soal :

Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Modifikasi file **/etc/bind/zones/abimanyu.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.it11.com. root.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@             IN      NS      abimanyu.it11.com.
@             IN      A       10.69.3.3 ; IP abimanyu
www           IN      CNAME   abimanyu.it11.com.
parikesit     IN      A       10.69.3.3 ; IP abimanyu
www.parikesit IN      CNAME   parikesit.abimanyu.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

Pengecekan dapat dilakukan dengan 2 cara, yaitu:

1. Dengan nslookup

![nslookup parikesit.abimanyu.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/3efc2ce7-3eeb-4024-ae52-0bb09d52bda8)

2. Lakukan ping pada nodes client ( Nakula / Sadewa )

Tambahkan IP Yudhistira pada /etc/resolv.conf

```
nameserver 10.69.1.2
```

Lakukan ping pada nodes client

![ping parikesit.abimanyu.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/237d251d-7c8b-4204-8d9e-7ce13b4f0b54)

## Soal 5

Soal :

Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Menambahkan konfigurasi dibawah pada **/etc/bind/named.conf.local**

```
zone "3.69.10.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/3.69.10.in-addr.arpa";
};
```

Membuat folder zones dan memasukkan konfigurasi ke dalam file **/etc/bind/zones/3.69.10.in-addr.arpa**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.it11.com. root.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
; PTR Records
3.69.10.in-addr.arpa.    IN      NS      abimanyu.it11.com.
3                        IN      PTR     abimanyu.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

Pengecekan dapat dilakukan dengan menjalankan perintah

```
host -t PTR 10.69.3.3
```

![ping parikesit.abimanyu.it11.com](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/a50bd247-d863-4739-8775-fbd1a4a53813)

## Soal 6

Soal :

Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Modifikasi konfigurasi pada **/etc/bind/named.conf.local** pada Yudhistira

```
zone "arjuna.it11.com" {
    type master;
    notify yes;
    also-notify { 10.69.2.2; };
    allow-transfer { 10.69.2.2; };
    file "/etc/bind/zones/arjuna.it11.com.zone";
};

zone "abimanyu.it11.com" {
    type master;
    notify yes;
    also-notify { 10.69.2.2; };
    allow-transfer { 10.69.2.2; };
    file "/etc/bind/zones/abimanyu.it11.com.zone";
};
```

Kemudian lakukan konfigurasi pada Werkudara, pada file **/etc/bind/named.conf.local**

```
zone "abimanyu.it11.com" {
    type slave;
    masters { 10.69.1.2; }; # IP Yudhistira
    file "/etc/bind/zones/slave.abimanyu.it11.com.zone";
};

zone "arjuna.it11.com" {
    type slave;
    masters { 10.69.1.2; }; # IP Yudhistira
    file "/etc/bind/zones/slave.arjuna.it11.com.zone";
};
```

Membuat folder zones dan memasukkan konfigurasi ke dalam file **/etc/bind/zones/slave.abimanyu.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.it11.com. root.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.it11.com.
; DNS Records
@           IN      A       10.69.3.3
www         IN      CNAME   abimanyu.it11.com.
parikesit   IN      A       10.69.3.3
```

Memasukkan konfigurasi ke dalam file **/etc/bind/zones/slave.arjuna.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.it11.com. root.arjuna.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.it11.com.
; DNS Records
@           IN      A       10.69.3.5 ; IP Utama
www         IN      CNAME   arjuna.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

Pengecekan dapat dilakukan dengan memasukkan IP Werkudara ke client pada file **/etc/resolv.conf**, seperti dibawah

```
#IP Yudhistira
#nameserver 10.69.1.2
#IP Werkudara
nameserver 10.69.2.2
#nameserver 192.168.122.1
```

![ping arjuna](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/9b16a968-7a89-4d8a-a14c-30d6d7775c5e)

![ping abimanyu](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/bb2c9d9d-6ae4-4e69-bfcc-c62b9b0fefd8)

## Soal 7

Soal :

Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

Mengubah file **/etc/bind/zones/abimanyu.it11.com.zone** pada Yudhistira

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.it11.com. root.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@             IN      NS      abimanyu.it11.com.
@             IN      A       10.69.3.3 ; IP abimanyu
www           IN      CNAME   abimanyu.it11.com.
parikesit     IN      A       10.69.3.3 ; IP abimanyu
www.parikesit IN      CNAME   parikesit.abimanyu.it11.com.
ns1           IN      A       10.69.2.2 ; IP Werkudara
baratayuda    IN      NS      ns1
```

Kemudian tambahkan konfigurasi pada Werkudara, pada file **/etc/bind/named.conf.local**

```
zone "baratayuda.abimanyu.it11.com" {
    type master;
    file "/etc/bind/zones/baratayuda.abimanyu.it11.com.zone";
};
```

Membuat folder zones dan memasukkan konfigurasi ke dalam file **/etc/bind/zones/baratayuda.abimanyu.it11.com.zone**

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.it11.com. root.baratayuda.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.it11.com.
; DNS Records
@           IN      A       10.69.3.3 
www         IN      CNAME   baratayuda.abimanyu.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

![ping baratayuda](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/0474e228-161d-4f6b-acb8-4c7541ef2fb3)

## Soal 8

Soal :

Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Menambahkan konfigurasi pada **/etc/bind/named.conf.local**

```
zone "rjp.baratayuda.abimanyu.it11.com" {
    type master;
    file "/etc/bind/zones/rjp.baratayuda.abimanyu.it11.com.zone";
};
```

Membuat file **/etc/bind/zones/rjp.baratayuda.abimanyu.it11.com.zone** pada Werkudara

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rjp.baratayuda.abimanyu.it11.com. root.rjp.baratayuda.abimanyu.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rjp.baratayuda.abimanyu.it11.com.

; DNS Records
@           IN      A       10.69.3.3
www         IN      CNAME   rjp.baratayuda.abimanyu.it11.com.
```

Kemudian, restart bind dengan command

```
service bind9 restart
```

![ping baratayuda](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/5355643d-eb98-4f0d-845f-01ad0aa414f0)

## Soal 9

Soal :

Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Pada nodes **Prabukusuma**

Membuat file **/var/www/jarkom/index.php**

```
<?php
echo "Hello World from prabukusuma";
?>
```

Membuat konfigurasi **/etc/nginx/sites-available/jarkom**

```
server {
    listen 8001;
    root /var/www/jarkom;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}
```

Kemudian jalankan command

```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart

service php7.2-fpm start

service php7.2-fpm status
```

Dapat diakses dengan cara

```
lynx http://10.69.3.2:8001
```

![prabukusuma](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/398652c3-a12a-4c26-945b-a9d4f5212305)

Pada nodes **Wisanggeni**

Membuat file **/var/www/jarkom/index.php**

```
<?php
echo "Hello World from wisanggeni";
?>
```

Membuat konfigurasi **/etc/nginx/sites-available/jarkom**

```
server {
    listen 8003;
    root /var/www/jarkom;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}
```

Kemudian jalankan command

```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart

service php7.2-fpm start

service php7.2-fpm status
```

Dapat diakses dengan cara

```
lynx http://10.69.3.4:8003
```

![wisanggeni](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/cb563344-1cb2-4650-9dee-5b95232c5494)

Pada nodes **Abimanyu**

Membuat file **/var/www/jarkom/index.php**

```
<?php
echo "Hello World from abimanyu";
?>
```

Membuat konfigurasi **/etc/nginx/sites-available/jarkom**

```
server {
    listen 8002;
    root /var/www/jarkom;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
    }
    location ~ /\.ht {
        deny all;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}
``` 

Kemudian jalankan command

```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart

service php7.2-fpm start

service php7.2-fpm status
```

Dapat diakses dengan cara

```
lynx http://10.69.3.3:8002
```

![abimanyu](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/f68b9abb-a9da-4aff-b991-585c7b213721)

## Soal 10

Soal :

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh

    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

Membuat konfigurasi pada node Arjuna untuk load balancer pada file **/etc/nginx/sites-available/load-balancer**

```
upstream webserver {
    server 10.69.3.2:8001; # IP Prabukusuma
    server 10.69.3.3:8002; # IP Abimanyu
    server 10.69.3.4:8003; # IP Wisanggeni
}
server {
    listen 80;
    server_name arjuna.it11.com www.arjuna.it11.com;
    location / {
        proxy_pass http://webserver;
    }
}
```

Mejalankan command

```
ln -s /etc/nginx/sites-available/load-balancer /etc/nginx/sites-enabled/load-balancer

service nginx restart
```

Dapat diakses dengan cara

```
lynx http://www.arjuna.it11.com
```

![upstream arjuna prabukusuma](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/398652c3-a12a-4c26-945b-a9d4f5212305)

![upstream arjuna abimanyu](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/f68b9abb-a9da-4aff-b991-585c7b213721)

![upstream arjuna wisanggeni](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/88996914/cb563344-1cb2-4650-9dee-5b95232c5494)

## Soal 11

Soal :

Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

Membuat konfigurasi pada **/etc/apache2/sites-available/abimanyu-it11.conf**
```
<VirtualHost *:80>
    ServerName abimanyu.it11.com
    ServerAlias www.abimanyu.it11.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu-it11

    <Directory /var/www/abimanyu-it11>
        Options +Indexes
    </Directory>

    RewriteEngine On

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

membuat perintah untuk download kebutuhan website dari google drive serta meletakkannya pada direktori **var/www/**
```
cd /var/www

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' -O abimanyu

unzip abimanyu -d abimanyu-it11

rm abimanyu

mv abimanyu-it11/abimanyu.yyy.com/* abimanyu-it11

rmdir abimanyu-it11/abimanyu.yyy.com
```

untuk mengecek apakah konfigurasinya berhasil, pergi ke terminal client **Sadewa / Nakula** dan cek menggunakan perintah ``lynx http://www.abimanyu.it11.com``
maka akan muncul tampilan halaman home abimanyu

![tampilan_abimanyu](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/0bfc2302-54eb-479e-80f9-06b9be54ef26)

## Soal 12

Soal :

Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

Untuk melkukan konfigurasi, ditambahkan kode berikut pada **/etc/apache2/sites-available/abimanyu-it11.conf**
```
<Directory /var/www/abimanyu-it11>
        Options +Indexes
    </Directory>

    Alias /home /var/www/abimanyu-it11/index.php/home
```
kemudian menjalankan perintah untuk menjalankan konfigurasi

```
cd /etc/apache2/sites-available/

a2enmod rewrite
a2ensite abimanyu-it11.conf
service apache2 reload
service apache2 start
service apache2 status
```
Untuk melakukan pengecekan maka pergi ke terminal client **Sadewa / Nakula** dan cek menggunakan perintah ``lynx http://www.abimanyu.it11.com/home``
maka akan muncul tampilan halaman home abimanyu sama seperti soal no 11


![image](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/56fb6733-d56a-414f-ab56-f65770cbb275)


## Soal 13

Soal :

Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

masukkan konfigurasi untuk website pada **/etc/apache2/sites-available/parikesit-abimanyu-it11.conf**
```
<VirtualHost *:80>
    ServerName parikesit.abimanyu.it11.com
    ServerAlias www.parikesit.abimanyu.it11.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/parikesit-abimanyu-it11

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
kemudian jalankan code untuk melakukan download kebutuhan website dari drive yang telah disediakan

```
cd /var/www

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS' -O parikesit.zip

unzip parikesit.zip -d parikesit-abimanyu-it11

rm parikesit.zip

mv parikesit-abimanyu-it11/parikesit.abimanyu.yyy.com/* parikesit-abimanyu-it11

rm -rf parikesit-abimanyu-it11/parikesit.abimanyu.yyy.com
```

Untuk melakukan pengecekan maka pergi ke terminal client **Sadewa / Nakula** dan cek menggunakan perintah ``lynx http://www.parikesit.abimanyu.it11.com``
maka akan ditampilkan halaman website


![web_parikesit](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/ca956ea9-6f14-4509-b2fd-51fa2e9adf3f)


## Soal 14

Soal :

Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

untuk melakukan ini, maka menambahkan kode pada **/etc/apache2/sites-available/parikesit-abimanyu-it11.conf** untuk menyalakan directory listing pada folder /public, dan mematikan directory listing pada /secret

```
<Directory /var/www/parikesit-abimanyu-it11/public>
   Options +Indexes
</Directory>

<Directory /var/www/parikesit-abimanyu-it11/secret>
   Options -Indexes
</Directory>
```
maka ketika mengakses ```lynx http://www.parikesit.abimanyu.it11.com/public``` pada client akan ditampilkan halaman berikut

![parikesit_public](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/88b5b5a6-3107-49d3-9bed-d419562000f3)

sedangkan ketika mengakses ```lynx http://www.parikesit.abimanyu.it11.com/secret``` pada client akan ditampilkan halaman berikut

![secret](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/8bc856fb-d28d-49fc-bba9-41e4e1881666)


## Soal 15

Soal :

Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

untuk melakukan hal ini maka tambahkan kode berikut pada **/etc/apache2/sites-available/parikesit-abimanyu-it11.conf**

```
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```

maka ketika mengakses ```lynx http://www.parikesit.abimanyu.it11.com/secret``` pada client akan ditampilkan halaman berikut

![secrt](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/bb2bbcc2-1af2-4a9b-8444-0677063e6e46)

sedangkan ketika mengakses ```lynx http://www.parikesit.abimanyu.it11.com``` dengan tambahan string acak/halaman yang tidak tersedia, contohnya ```lynx http://www.parikesit.abimanyu.it11.com/acak``` maka pada client akan ditampilkan eror 404 diikuti dengan halaman berikut

![acak](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/9c018fb4-93fa-44cf-a3b5-a83232879d65)


## Soal 16

Soal :

Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

untuk melakukan hal ini maka tambahkan kode berikut pada **/etc/apache2/sites-available/parikesit-abimanyu-it11.conf**

```
Alias /js /var/www/parikesit-abimanyu-it11/public/js

RewriteEngine On
```

dan jalankan perintah berikut pada terminal
```
cd /etc/apache2/sites-available/

a2enmod rewrite
a2ensite parikesit-abimanyu-it11.conf
service apache2 reload
service apache2 start
service apache2 status
```
maka ketika mengakses suatu halaman cukum menuliskan perintah ```lynx http://www.parikesit.abimanyu.it11.com/js``` pada client dan akan ditampilkan halaman berikut

![/js](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/d587c07e-4a45-4a0e-8a37-298cbc1b27f4)

## Soal 17

Soal :

Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

Masukkan konfigurasi untuk website pada **/etc/apache2/sites-available/rjp-baratayuda-abimanyu-it11.conf**

```
<VirtualHost *:14000 *:14400>
    ServerName rjp.baratayuda.abimanyu.it11.com
    ServerAlias www.rjp.baratayuda.abimanyu.it11.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/rjp-baratayuda-abimanyu

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
kode ```<VirtualHost *:14000 *:14400>``` berfungsi untuk mengkonigurasikan dari port mana saja website tersebut dapat diakses, sesuai dengan perintah pada soal maka dikonfigurasikan untuk port 14000 dan 14400

tambahkan juga kode berikut pada **/etc/apache2/ports.conf** untuk melakukan konfigurasi port
```
Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

yang terjadi adalah ketika kita mengakses website dengan perintah ```lynx http://www.rjp.baratayuda.abimanyu.it11.com``` saja, maka akan ditampilkan halaman berikut:

![no_access](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/97fa6f20-7452-4b53-9111-28b88d81cdd6)

untuk mengaksesnya maka perlu ada tambahan port seperti ```lynx http://www.rjp.baratayuda.abimanyu.it11.com:14000``` atau ```lynx http://www.rjp.baratayuda.abimanyu.it11.com:14400```
maka tampilannya akan seperti ini

![baratayuda_success](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/5e4edf78-13d3-4849-a496-4bbc78edd3ed)


## Soal 18

Soal :

Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

Untuk melakukan hal ini tambahkan konfigurasi untuk website pada **/etc/apache2/sites-available/rjp-baratayuda-abimanyu-it11.conf**

```
 <Directory "/var/www/rjp-baratayuda-abimanyu">
        Options +FollowSymLinks -Multiviews
        AllowOverride All
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </Directory>
```
Kemudian jalankan perintah berikut pada terminal untuk melakukan konfigurasi autentikasi user
```
cd /etc/apache2/sites-available/

htpasswd -c /etc/apache2/.htpasswd Wayang
```
dan jalankan perintah berikut untuk menerapkan konfigurasi

```
a2ensite rjp-baratayuda-abimanyu-it11.conf
service apache2 reload
service apache2 start
service apache2 status
```

masukkan password yang sudah ditentukan yaitu baratayudait11 ketika diminta oleh terminal saat menjalankan kode

![image](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/6ca8601f-7d47-46fe-abd7-b32153c4fab9)

maka ketika kita mengakses ```lynx http://www.rjp.baratayuda.abimanyu.it11.com:14000``` atau ```lynx http://www.rjp.baratayuda.abimanyu.it11.com:14400``` akan ditunjukkan halaman autentifikasi user seperti berikut:

![user](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/6bcecba8-15f9-438f-8be5-f0b13f148760)

dan permintaan password


![password](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/dee0023f-44b7-4817-b486-70d426a03c2b)


jika sudah maka akan ditampilkan 


![baratayuda_success](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/5e4edf78-13d3-4849-a496-4bbc78edd3ed)

## Soal 19

Soal :

Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

Tambahkan kode beikut pada **/etc/apache2/sites-available/abimanyu-it11.conf**
```
ServerAlias 10.69.3.3
```
sehingga saat mengakses ```lynx http://10.69.3.3``` akan ditampilkan halaman home


![terminal](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/263f4a83-a885-45ab-b6f5-1744c7719d4e)




![home_ip_abimanyu](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/5447d043-1e59-4a9b-93d6-7991b10d7ef1)


## Soal 20

Soal :

Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
Tambahkan kode beikut pada **/etc/apache2/sites-available/parikesit-abimanyu-it11.conf**
```
# Tambahkan aturan RewriteCond untuk mencocokkan permintaan yang mengandung "abimanyu"
    RewriteCond %{REQUEST_URI} abimanyu [NC]

    # Terapkan aturan RewriteRule untuk mengarahkan permintaan ke /public/images/abimanyu.png
    RewriteRule (.*) /public/images/abimanyu.png [L]

```
maka ketika kita mengakses website dengan tambahan string yang mengandung kata abimanyu seperti ```lynx http://www.parikesit.abimanyu.it11.com/abimanyu``` atau bahkan huruf acak yang mengandung kata abimanyu seperti ```lynx http://www.parikesit.abimanyu.it11.com/dkfkwabimanyu```

![terminal_parikesit](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/53673162-96b3-46c5-a380-ce747f4b4071)


maka akan ditampilkan tampilan yang meminta persetujuan untuk mendownload file png dimana file tersebut merupakan file abimanyu.png


![izin_download](https://github.com/Yuniarrr/Jarkom-Modul-2-IT11-2023/assets/107184933/584fdb02-8279-4490-9857-d80fb8da1125)

