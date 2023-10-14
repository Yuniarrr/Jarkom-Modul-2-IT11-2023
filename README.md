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

## Soal 7

Soal :

Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

## Soal 8

Soal :

Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

## Soal 9

Soal :

Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

## Soal 10

Soal :

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh

    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

## Soal 11

Soal :

Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

## Soal 12

Soal :

Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

## Soal 13

Soal :

Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

## Soal 14

Soal :

Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

## Soal 15

Soal :

Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

## Soal 16

Soal :

Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

## Soal 17

Soal :

Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

## Soal 18

Soal :

Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

## Soal 19

Soal :

Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

## Soal 20

Soal :

Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
