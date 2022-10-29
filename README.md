# Jarkom-Modul-2-E04-2022

- Muhammad Afif Dwi Ardiansyah 5025201212
- Muhammad Rafif Fadhil Naufal 5025201
- M Labib Alfaraby 5025201083

### List

1. [Soal 1](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#1)
2. [Soal 2](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#2)
3. [Soal 3](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#3)
4. [Soal 4](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#4)
5. [Soal 5](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#5)
6. [Soal 6](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#6)
7. [Soal 7](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#7)
8. [Soal 8](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#8)
9. [Soal 9](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#9)
10. [Soal 10](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#10)
11. [Soal 11](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#11)
12. [Soal 12](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#12)
13. [Soal 13](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#13)
14. [Soal 14](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#14)
15. [Soal 15](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#15)
16. [Soal 16](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#16)
17. [Soal 17](https://github.com/labibs30/Jarkom-Modul-2-E04-2022.git#17)

## 1

> Membuat topologi seperti gambar dibawah : 

![image](https://user-images.githubusercontent.com/96496752/198814697-9e430bfd-8920-42d2-931f-34480d68133d.png)

dengan setiap node ubuntu dapat terhubung ke internet, untuk itu pada node `Ostania` perlu dilakukan konfigurasi seperti berikut : 
```shell
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.194.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.194.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.194.3.1
	netmask 255.255.255.0
```
Kemudian, untuk setiap node menggunakan konfigurasi seperti berikut : 

`WISE`
```shell
auto eth0
iface eth0 inet static
	address 192.194.3.2
	netmask 255.255.255.0
	gateway 192.194.3.1
```
`SSS`

```shell
auto eth0
iface eth0 inet static
	address 192.194.1.2
	netmask 255.255.255.0
	gateway 192.194.1.1
```
`Garden`

```shell
auto eth0
iface eth0 inet static
	address 192.194.1.3
	netmask 255.255.255.0
	gateway 192.194.1.1
```
`Berlint`
```shell
auto eth0
iface eth0 inet static
	address 192.194.2.2
	netmask 255.255.255.0
	gateway 192.194.2.1
```

`Eden`
```shell
auto eth0
iface eth0 inet static
	address 192.194.2.3
	netmask 255.255.255.0
	gateway 192.194.2.1
```

Kemudian, meletakkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.194.0.0/16` pada `/root/.bashrc` pada **Ostania**. Terakhir, tambahkan `echo nameserver 192.168.122.1` untuk setiap node pada `/root/.bashrc`.

## 2
> Membuat webiste utama dengan menggunakan `wise.E04.com` dengan alias `www.wise.E04.com` pada folder wise. Untuk melakukan hal tersebut, digunakan perintah seperti berikut : 
1. `WISE`

```shell
apt-get update

apt-get install bind9 -y

echo 'zone "wise.E04.com" {
    type master;
    file "/etc/bind/wise/wise.E04.com";
};' > /etc/bind/named.conf.local


mkdir /etc/bind/wise

cp /etc/bind/db.local /etc/bind/wise/wise.E04.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E04.com. root.wise.E04.com. (
                     2022102401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      wise.E04.com.
@       IN      A       192.194.3.2     ; IP Wise
www     IN      CNAME   wise.E04.com.
@       IN      AAAA    ::1' > /etc/bind/wise/wise.E04.com

service bind9 restart
```

2. `SSS`
```shell
echo nameserver 192.194.3.2 > /etc/resolv.conf
```
Untuk melakukan pengecekan apakah konfigurasi yang telah dibuat berhasil, maka kita melakukan check pada client `SSS` atau `Garden` menggunakan perintah :
`ping wise.E04.com` atau `www.wise.E04.com`

![image](https://user-images.githubusercontent.com/96496752/198818419-aa78d08d-afd6-4f94-b436-4a3809c36fc8.png)

![image](https://user-images.githubusercontent.com/96496752/198818465-cfe5d52e-2736-4832-a18f-a849205bdcc1.png)

## 3
> Membuat subdomain `eden.wise.yyy.com` dengan alias `www.eden.wise.yyy.com` yang diatur DNS-nya di **WISE** dan mengarah ke **Eden**

Pada `WISE` kita cukup menambahkan `eden`

```shell
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E04.com. root.wise.E04.com. (
                     2022102401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      wise.E04.com.
@       IN      A       192.194.3.2     ; IP Wise
www     IN      CNAME   wise.E04.com.
eden    IN      A       192.194.2.3
www.eden     IN      CNAME   eden.wise.E04.com.
@       IN      AAAA    ::1' > /etc/bind/wise/wise.E04.com

service bind9 restart
```
Selanjutnya, menjalankan `ping eden.wise.yyy.com` dan `ping www.eden.wise.yyy.com` kita batasi untuk 5 ping menggunakan `-c 5`. Berikut hasilnya : 

![image](https://user-images.githubusercontent.com/96496752/198831311-848bc2b9-0fb1-4f3d-9205-2a8e60659f65.png)

## 4

> Membuat juga reverse domain untuk domain utama.

Kita tambahkan konfigurasi **in-addr.arpa** pada file ` named.conf.local`, seperti berikut
Pada `WISE`

```shell
echo 'zone "wise.E04.com" {
        type master;
        file "/etc/bind/wise/wise.E04.com";
};

zone "3.194.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/3.194.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/wise/3.194.192.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.E04.com. root.wise.E04.com. (
                     2022102401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.194.192.in-addr.arpa.    IN      NS      wise.E04.com.
2                IN      PTR     wise.E04.com.' > /etc/bind/wise/3.194.192.in-addr.arpa

service bind9 restart
```
pada `SSS`
```shell
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install dnsutils

echo nameserver 192.194.3.2 > /etc/resolv.conf

host -t PTR 192.194.3.2
```
Untuk test bisa menggunakan `host -t PTR 192.194.3.2` pada client, berikut hasil yang diperoleh : 

![image](https://user-images.githubusercontent.com/96496752/198832131-c8ed524d-e44e-4c09-9cdf-a8442a7bd245.png)

## 5

> Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

pada `WISE`
```shell
echo 'zone "wise.E04.com" {
        type master;
      notify yes;
          also-notify { 192.194.2.2; }; // Masukan IP Berlint tanpa tanda petik
        allow-transfer { 192.194.2.2; }; // Masukan IP Berlint tanpa tanda petik
        file "/etc/bind/wise/wise.E04.com";
};

zone "3.194.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/3.194.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

service bind9 restart
```
pada `Berlint`
```shell
apt-get update

apt-get install bind9 -y

echo 'zone "wise.E04.com" {
    type slave;
    masters { 192.194.3.2; }; // Masukan IP Wise tanpa tanda petik
    file "/var/lib/bind/wise.E04.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
`WISE` untuk test nomor 5
```shell
service bind9 stop
```
`SSS ` untuk test nomor 5
```shell
echo 'nameserver 192.194.3.2
nameserver 192.194.2.2' > /etc/resolv.conf

ping wise.E04.com -c 5
```
Setelah dijalankan, maka akan diperoleh : 
![image](https://user-images.githubusercontent.com/96496752/198834430-b953cd61-0fec-44ef-a9d2-ebb087606253.png)

## 6

> Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

Pada soal ini melakukan delegasi domain.

**Enieslobby /etc/bind/kaizoku/franky.e01.com.**
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.e01.com. root.franky.e01.com. (
                     2021100401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.e01.com.
@       IN      A       192.200.2.2
www     IN      CNAME   franky.e01.com.
super   IN      A       192.200.2.4
www.super IN    CNAME   super.franky.e01.com.
mecha   IN      NS      ns1
```
Kemudian, melakukan pengeditan pada named.conf.options dnnsec, dan menambahkan allow-query{any;};

**Water7 /etc/bind/named.conf.local**
```shell
// SLAVE
zone "franky.e01.com" {
    type slave;
    masters { 192.200.2.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/franky.e01.com";
};

// DELEGASI
zone "mecha.franky.e01.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.e01.com"
};
```
Edit named.conf.options dnnsec dan menambahkan allow-query{any;};. Selain itu Delegasi juga ditambahkan.

**Water7 /etc/bind/sunnygo/mecha.franky.e01.com**
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mecha.franky.e01.com. root.mecha.franky.e01.com. (
                              2021100401                ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mecha.franky.e01.com.
@       IN      A       192.200.2.4
www     IN      CNAME   mecha.franky.e01.com.
```
Menambahkan Aliasnya.

`ping mecha.franky.e01.com`
`ping www.mecha.franky.e01.com  `

![soal6](https://user-images.githubusercontent.com/65794806/139521039-0ec050f5-7fa7-4b9b-838d-35994e37ca79.png)

## 7

> Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Franky dengan nama general.mecha.frank.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.

Membuat sub-domain kemudian menambahkan `alias general.mecha.franky.e01.com.`.

**Water7 /etc/bind/sunnygo/mecha.franky.e01.com**
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mecha.franky.e01.com. root.mecha.franky.e01.com. (
                              2021100401                ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mecha.franky.e01.com.
@       IN      A       192.200.2.4
www     IN      CNAME   mecha.franky.e01.com.
general IN      A       192.200.2.4
www.general IN  CNAME   general.mecha.franky.e01.com.
```

`ping general.mecha.franky.e01.com`
`ping www.general.mecha.franky.e01.com`

![soal7](https://user-images.githubusercontent.com/65794806/139521280-b16ba373-2efb-4c3e-ac08-8f05f8d9594a.png)

## 8

> Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com

Karena menggunakan web server, maka Config EniesLobby terlebih dahulu diarahkan ke Skypie.

**EniesLobby modul1/webserver/franky.e01.com**
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.e01.com. root.franky.e01.com. (
                        4               ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.e01.com.
@       IN      A       192.200.2.4
www     IN      CNAME   franky.e01.com.
super   IN      A       192.200.2.4
www.super IN    CNAME   super.franky.e01.com.
mecha   IN      NS      ns1
```

**EniesLobby modul1/webserver/2.200.192.in-addr-arpa**
```shell
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.e01.com. root.franky.e01.com. (
                        2               ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.200.192.in-addr.arpa. IN      NS      franky.e01.com.
2                       IN      PTR     franky.e01.com.
```

Install apache2 & php di water7 dan memasukkan dokumen htmlnya.

**Skypie modul1/franky.e01.com**
```bash
<VirtualHost *:80>
  ...
	ServerName franky.e01.com
  ServerAlias www.franky.e01.com

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/franky.e01.com
  
  ...

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

  ...
</VirtualHost>

```
`lynx http://www.franky.e01.com`

![soal8](https://user-images.githubusercontent.com/65794806/139521462-0b028254-894d-4d1b-9735-a03bf5e30126.png)

## 9

> Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.

Membuat alias dari home yang akan mengarah ke index.php/home

**Skypie modul1/franky.e01.com**
```bash
<VirtualHost *:80>
        ...
        ServerName franky.e01.com
        ServerAlias www.franky.e01.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/franky.e01.com

        Alias "/home" "/var/www/franky.e01.com/index.php/home"

        ...

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ...
</VirtualHost>

```
`lynx http://franky.e01.com/home`

![soal9](https://user-images.githubusercontent.com/65794806/139521813-929fd2fe-c606-4c3c-84ad-8fbd43353eb9.png)

## 10

> Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.

**Skypie /etc/apache2/sites-available/super.franky.e01.com.conf**
```bash
<VirtualHost *:80>
        ...
        ServerName super.franky.e01.com
        ServerAlias www.super.franky.e01.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.e01.com

        ...

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ...
</VirtualHost>
```

cek lynx ke super.franky.e01.com
`lynx http://www.super.franky.e01.com`

![soal10](https://user-images.githubusercontent.com/65794806/139521940-61d24205-55e8-401f-983c-950d0b4161ae.png)

Bentuknya berupa directory listing karena tidak ada default htmlnya.

## 11

> Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

Selanjutnya merupakan directory listing untuk public, dengan menambah directory dan option +indexes.

**Skypie super.franky.e01.com.conf**
```bash
<VirtualHost *:80>
        ServerName super.franky.e01.com
        ServerAlias www.super.franky.e01.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.e01.com

	<Directory /var/www/super.franky.e01.com>
                Options +Indexes
        </Directory>

	<Directory /var/www/super.franky.e01.com/error>
                Options -Indexes
        </Directory>

        <Directory /var/www/super.franky.e01.com/public>
                Options +Indexes
        </Directory>

        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

error dimatikan indexnya karena yang dibutuhkan pada public

`lynx http://www.super.franky.e01.com/public`

