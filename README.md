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

## Soal 8

> Diperlukan konfigurasi Webserver `www.wise.E04.com` dengan DocumentRoot pada `/var/www/wise.E04.com`. Untuk itu dilakukan command seperti berikut:
1. `Wise`
```
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
2. `Eden`
```
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

3. `SSS`
```
apt-get update

apt-get install lynx

lynx wise.E04.com

```
Maka akan muncul seperti ini:

## Soal 9
> Setelah itu, Loid juga membutuhkan agar url `www.wise.E04.com/index.php/home` dapat menjadi menjadi `www.wise.E04.com/home`. Untuk itu dilakukan command seperti berikut:
1. `Eden`
```
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
2. `SSS`
```
lynx www.wise.E04.com/home
```
Maka akan muncul seperti ini:

## Soal 10
> Setelah itu, pada subdomain `www.eden.wise.E04.com`, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/eden.wise.E04.com`. Untuk itu dilakukan command seperti berikut:
1. `Eden`
```
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
2. `SSS`
```
www.eden.wise.E04.com
```
Maka akan muncul seperti ini:
