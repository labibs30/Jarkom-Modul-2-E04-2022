# Jarkom-Modul-2-E04-2022

- Muhammad Afif Dwi Ardiansyah 5025201212
- Muhammad Rafif Fadhil Naufal 5025201273
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

## Soal 1

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

## Soal 2
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

## Soal 3
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

## Soal 4

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

## Soal 5

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

## Soal 6

> Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation


pada `Wise`
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
eden    IN      A       192.194.2.3     ; IP Eden
www.eden     IN      CNAME   eden.wise.E04.com.
ns1     IN      A       192.194.2.3     ; IP Eden
operation       IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/wise/wise.E04.com

```shell
echo 'options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "wise.E04.com" {
        type master;
        file "/etc/bind/wise/wise.E04.com";
        allow-transfer { 192.194.2.2; }; // Masukan IP Eden tanpa tanda petik
};

zone "3.194.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/3.194.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

service bind9 restart
```

Pada `Berlint`

```shell
echo 'options {
        directory "/var/cache/bind";

        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "wise.E04.com" {
        type slave;
        masters { 192.194.3.2; }; // Masukan IP Wise tanpa tanda petik
        file "/var/lib/bind/wise.E04.com";
};

zone "operation.wise.E04.com"{
        type master;
        file "/etc/bind/operation/operation.wise.E04.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/operation

cp /etc/bind/db.local /etc/bind/operation/operation.wise.E04.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.E04.com. root.operation.wise.E04.com. (
                     2022102401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      operation.wise.E04.com.
@               IN      A       192.194.2.3       ;ip Eden
www             IN      CNAME   operation.wise.E04.com.' > /etc/bind/operation/operation.wise.E04.com

service bind9 restart
```
Pada `SSS`
```shell
echo 'nameserver 192.194.3.2
nameserver 192.194.2.2
nameserver 192.194.2.3' > /etc/resolv.conf

ping operation.wise.E04.com -c 5
```
Pada `Wise`
![gamber 5 1](https://user-images.githubusercontent.com/81162174/198837277-a6090aa1-7f2a-4786-8116-72b0e761df57.PNG)
Pada `Berlint`
![gambar 63](https://user-images.githubusercontent.com/81162174/198837649-ce4902ba-9b08-41e1-8f3a-0d18659d8836.PNG)
Pada `SSS`

![gambar 5 2](https://user-images.githubusercontent.com/81162174/198837356-ebc614c6-12cb-41c6-ad25-d58499674b6d.PNG)

## Soal 7

> Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden.


Pada `Berlint`

```shell
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     operation.wise.E04.com. root.operation.wise.E04.com. (
                     2022102401         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      operation.wise.E04.com.
@               IN      A       192.194.2.3       ;ip Eden
www	          IN	CNAME	operation.wise.E04.com.
strix	          IN	A	192.194.2.3
www.strix	    IN	A	192.194.2.3' > /etc/bind/operation/operation.wise.E04.com

service bind9 restart

```

Pada `SSS`
```shell
ping strix.operation.wise.E04.com -c 5
```
Pada `Berlint`
![gamber 71](https://user-images.githubusercontent.com/81162174/198837565-e3076624-8b4b-4e26-a09e-bffea1c96fe0.PNG)

Pada `SSS`

![gambar 72](https://user-images.githubusercontent.com/81162174/198837570-78a2bb65-22f0-462c-a7f2-10441f56e8b1.PNG)

## Soal 8

> Diperlukan konfigurasi Webserver `www.wise.E04.com` dengan DocumentRoot pada `/var/www/wise.E04.com`. Untuk itu dilakukan command seperti berikut:
1. Di node `Wise`
```shell
cp /etc/bind/wise/3.194.192.in-addr.arpa /etc/bind/wise/2.194.192.in-addr.arpa

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
2.194.192.in-addr.arpa.       IN      NS      wise.E04.com.
3       IN      PTR      wise.E04.com.' > /etc/bind/wise/2.194.192.in-addr.arpa


echo 'zone "wise.E04.com" {
    type master;
    file "/etc/bind/wise/wise.E04.com";
    allow-transfer { 192.194.2.2; }; // Masukan IP Berlint tanpa tanda petik
};

zone "2.194.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/2.194.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local

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
@       IN      A       192.194.2.3
www     IN      CNAME   wise.E04.com.
eden    IN      A       192.194.2.3
www.eden        IN      CNAME   eden.wise.E04.com.
ns1     IN      A       192.194.2.3
operation       IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/wise/wise.E04.com

service bind9 restart
```
2. Di node `Eden`
```shell
apt-get update

apt-get install apache2

service apache2 start

apt-get install php

apt-get install libapache2-mod-php7.0

apt-get install wget -y

apt-get install unzip -y

cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wise.E04.com.conf

echo '
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/wise.E04.com
        ServerName wise.E04.com
        ServerAlias www.wise.E04.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
' > /etc/apache2/sites-available/wise.E04.com.conf

mkdir /var/www/wise.E04.com

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1S0XhL9ViYN7TyCj2W66BNEXQD2AAAw2e' -O /var/www/wise.zip

unzip /var/www/wise.zip -d /var/www

cp /var/www/wise/* /var/www/wise.E04.com

a2ensite wise.E04.com

service apache2 restart

service apache2 reload
```

3. Di node `SSS`
```shell
apt-get update

apt-get install lynx

lynx wise.E04.com

```
Maka akan muncul seperti ini:
</br></br>
<img width="528" alt="image" src="https://user-images.githubusercontent.com/87472849/198835758-c57234e0-fe90-4c93-8c41-99c0101c3496.png">


## Soal 9
> Setelah itu, Loid juga membutuhkan agar url `www.wise.E04.com/index.php/home` dapat menjadi menjadi `www.wise.E04.com/home`. Untuk itu dilakukan command seperti berikut:
1. Di node `Eden`
```shell
a2enmod rewrite

service apache2 restart

echo "
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule (.*) /index.php/\$1 [L]
" >/var/www/wise.E04.com/.htaccess

echo "
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/wise.E04.com
        ServerName wise.E04.com
        ServerAlias www.wise.E04.com

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        <Directory /var/www/wise.E04.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
</VirtualHost>
" > /etc/apache2/sites-available/wise.E04.com.conf

service apache2 restart
```
2. Di node `SSS`
```shell
lynx www.wise.E04.com/home
```
Maka akan muncul seperti ini:
</br></br>
<img width="516" alt="image" src="https://user-images.githubusercontent.com/87472849/198835937-dd3337bc-92ea-4ca8-b6eb-4f97859e4f4b.png">


## Soal 10
> Setelah itu, pada subdomain `www.eden.wise.E04.com`, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/eden.wise.E04.com`. Untuk itu dilakukan command seperti berikut:
1. Di node `Eden`
```shell
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/eden.wise.E04.com.conf

echo '
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.E04.com
        ServerName eden.wise.E04.com
        ServerAlias www.eden.wise.E04.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
' > /etc/apache2/sites-available/eden.wise.E04.com.conf

mkdir /var/www/eden.wise.E04.com

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV' -O /var/www/eden.wise.zip

unzip /var/www/eden.wise.zip -d /var/www

cp /var/www/eden.wise/* /var/www/eden.wise.E04.com

a2ensite eden.wise.E04.com

service apache2 restart

service apache2 reload
```
2. Di node `SSS`
```shell
lynx www.eden.wise.E04.com
```
Maka akan muncul seperti ini:
</br></br>
<img width="545" alt="image" src="https://user-images.githubusercontent.com/87472849/198836067-c694cb10-cfae-47f9-b81b-04f58ef2a7aa.png">


## Soal 11
> Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja. Untuk itu dilakukan command seperti berikut:
1. Di node `Eden`
```shell
echo "
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/eden.wise.E04.com
        ServerName eden.wise.E04.com
        ServerAlias www.eden.wise.E04.com

        <Directory /var/www/eden.wise.E04.com/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/eden.wise.E04.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
" > /etc/apache2/sites-available/eden.wise.E04.com.conf

service apache2 restart
```
2. Di node `SSS`
```shell
lynx www.eden.wise.E04.com
```
Maka akan muncul seperti ini:
</br></br>
<img width="546" alt="image" src="https://user-images.githubusercontent.com/87472849/198836219-9f682e15-471f-42fa-a0ea-ffce8510fb0f.png">

### Kendala

1. Ketika install lynx kendalanya adalah terkadang mengalami error ketika download atau terdownload namun sangat lama.
2. Terkadang project tiba-tiba connection lost sehingga run project mengulang dari awal

