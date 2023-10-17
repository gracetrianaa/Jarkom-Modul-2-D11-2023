# Jarkom-Modul-2-D11-2023

|Nama|NRP|
|----|---|
|Grace|5025211|
|Muhammad Rifqi Fadhilah|5025211228|

- [Soal 1](#soal-1)
- [Soal 2](#soal-2)
- [Soal 3](#soal-3)
- [Soal 4](#soal-4)
- [Soal 5](#soal-5)
- [Soal 6](#soal-6)
- [Soal 7](#soal-7)
- [Soal 8](#soal-8)
- [Soal 9](#soal-9)
- [Soal 10](#soal-10)

## Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

Answer :
### Topologi 
<img width="705" alt="1" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/41b9ae8b-088d-4f36-9561-a0eb590d1f1d">

### Config
- Router

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.27.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.27.2.1
	netmask 255.255.255.0
```
- NakulaClient

```
 auto eth0
 iface eth0 inet static
 	address 10.27.1.2
 	netmask 255.255.255.0
 	gateway 10.27.1.1
```

- SadewaClient

```
 auto eth0
 iface eth0 inet static
 	address 10.27.1.3
 	netmask 255.255.255.0
 	gateway 10.27.1.1
```

- AbimanyuWebServer

```
auto eth0
iface eth0 inet static
	address 10.27.1.4
	netmask 255.255.255.0
	gateway 10.27.1.1
```

- PrabukusumaWebServer

```
auto eth0
iface eth0 inet static
	address 10.27.1.5
	netmask 255.255.255.0
	gateway 10.27.1.1
```

- WisanggeniWebServer

```
auto eth0
iface eth0 inet static
	address 10.27.1.6
	netmask 255.255.255.0
	gateway 10.27.1.1
```

- YudhistiraDNSMaster

```
auto eth0
iface eth0 inet static
	address 10.27.2.2
	netmask 255.255.255.0
	gateway 10.27.2.1
```

- WerkudaraDNSSlave

```
auto eth0
iface eth0 inet static
	address 10.27.2.3
	netmask 255.255.255.0
	gateway 10.27.2.1
```

- ArjunaLoadBalancer

```
auto eth0
iface eth0 inet static
	address 10.27.2.4
	netmask 255.255.255.0
	gateway 10.27.2.1
```

setelah melakukan setup maka kita coba lakukan `ping google` untuk membuktikan apakah setup kita sudah benar atau tidak

<img width="659" alt="2" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/d158d403-b51c-4db3-b1d3-2245d595a487">


## Soal 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Answer :
Masukan script/setup pada node YudhistiraDNSMaster sebagai berikut :
```
echo 'zone "arjuna.d11.com" {
	type master;
	file "/etc/bind/jarkom/arjuna.d11.com";
};' > /etc/bind9/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.d11.com

echo '$TTL	604800
@	IN	SOA	arjuna.d11.com.	root.arjuna.d11.com. (
			2022100601	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@	IN	NS	arjuna.d11.com.
@	IN	A	10.27.2.4	; IP Arjuna
www	IN	CNAME	arjuna.d11.com.
@	IN	AAAA	::1' > /etc/bind/jarkom/arjuna.d11.com

service bind9 restart
```

jangan lupa setup nameserver pada client untuk diarahkan ke `IP YudhistiraDNSMaster` dan untuk membuktikan setup domain arjuna.d11.com sudah benar maka lakukan ping di node client

```
ping arjuna.d11.com -c 5
ping www.arjuna.d11.com -c 5
```

<img width="490" alt="3" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/8e01ce54-1ab1-4a04-8bec-98e0e7fefc18">

## Soal 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Answer :
Tambahkan script/setup pada node YudhistiraDNSMaster sebagai berikut :

```
echo 'zone "abimanyu.d11.com" {
	type master;
	file "/etc/bind/jarkom/abimanyu.d11.com";
};' > /etc/bind9/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.d11.com

echo '$TTL	604800
@	IN	SOA	abimanyu.d11.com.	root.abimanyu.d11.com. (
			2022100601	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@	IN	NS	abimanyu.d11.com.
@	IN	A	10.27.1.4	; IP Abimanyu
www	IN	CNAME	abimanyu.d11.com.
@	IN	AAAA	::1' > /etc/bind/jarkom/abimanyu.d11.com

service bind9 restart
```

untuk membuktikan setup domain abimanyu.d11.com sudah benar maka lakukan ping di node client

```
ping abimanyu.d11.com -c 5
ping www.abimanyu.d11.com -c 5
```

<img width="612" alt="4" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/cf98ddbf-f948-4a73-be3f-df2d129254e4">

## Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Answer :
Tambahkan setup pada node YudhistiraDNSMaster pada `/etc/bind/jarkom/abimanyu.d11.com`

```
echo '$TTL	604800
@	IN	SOA	abimanyu.d11.com.	root.abimanyu.d11.com. (
			2022100601	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@	IN	NS	abimanyu.d11.com.
@	IN	A	10.27.1.4	
www	IN	CNAME	abimanyu.d11.com.
parikesit	IN	A	10.27.1.4	
@	IN	AAAA	::1' > /etc/bind/jarkom/abimanyu.d11.com

service bind9 restart
```

untuk membuktikan setup subdomain parikesit.abimanyu.d11.com sudah benar maka lakukan ping di node client

<img width="587" alt="5" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/49e041ac-b5ab-4f56-bdbc-c0369fd696f4">

## Soal 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Answer :
karena yang ingin di reverse adalah abimanyu, maka kita harus tau `IP Abimanyu` yaitu `10.27.1.4` dan selanjutkan kita setup di node YudhistiraDNSMaster

```
echo 'zone "1.27.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/1.27.10.in-addr.arpa";
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/1.27.10.in-addr.arpa

echo '$TTL	604800
@	IN	SOA	abimanyu.d11.com.	root.abimanyu.d11.com. (
			2022100601	; Serial
			604800		; Refresh
			86400		; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
1.27.10.in-addr.arpa.	IN	NS	abimanyu.d11.com.
4	IN	PTR	abimanyu.d11.com.' > /etc/bind/jarkom/1.27.10.in-addr.arpa

service bind9 restart
```

untuk membuktikan setup reverse abimanyu benar maka cek di client menggunakan syntax

```
host -t PTR 10.27.1.4
```

<img width="383" alt="6" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/da4e12ce-9944-4453-9624-1a9477ffb772">

## Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Answer :
Lakukan tambahan setup di node YudhistiraDNSMaster

```
echo 'zone "arjuna.d11.com" {
    type master;
    notify yes;
    also-notify { 10.27.2.3; }; 
    allow-transfer { 10.27.2.3; };
    file "/etc/bind/jarkom/arjuna.d11.com";
};

zone "abimanyu.d11.com" {
    type master;
    notify yes;
    also-notify { 10.27.2.3; }; 
    allow-transfer { 10.27.2.3; };
    file "/etc/bind/jarkom/abimanyu.d11.com";
};' > /etc/bind/named.conf.local

service bind9 restart
// karena ingin test DNS Slave maka jangan lupa stop bind9 di DNS Master 
service bind9 stop
```

lakukan setup di node WerkudaraDNSSlave

```
echo 'zone "arjuna.d11.com" {
    type slave;
    masters { 10.27.2.2; }; 
    file "/var/lib/bind/arjuna.d11.com";
};

zone "abimanyu.d11.com" {
    type slave;
    masters { 10.27.2.2; }; 
    file "/var/lib/bind/abimanyu.d11.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```

setelah stop bind9 di DNS Master lakukan ping di client untuk membuktikan setup DNS Slave sudah benar

<img width="857" alt="7" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/1dbeb02a-98c1-40fe-aaa9-8f4592ce9967">

<img width="482" alt="8" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/6496d1ac-5920-46d3-8431-a9c1326a6d14">

## Soal 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

Answer :
Lakukan setup di node YudhistiraDNSMaster

```
echo '$TTL      604800
@       IN      SOA     abimanyu.d11.com.       root.abimanyu.d11.com. (
                        2022100601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800  )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.d11.com.
@       IN      A       10.27.1.4       ; IP Abimanyu
www     IN      CNAME   abimanyu.d11.com.
parikesit       IN      A       10.27.1.4 ; IP Abimanyu
abimanyu        IN      A       10.27.1.4 ; IP Abimanyu
www.abimanyu    IN      A       10.27.1.4
ns1     IN      A       10.27.2.3       ; IP Werkudara
baratayuda      IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.d11.com

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no; # conform to RFC1035
        listen-on-v6 { any; };

};' > /etc/bind/named.conf.options

service bind9 restart
```

Lakukan juga setup pada WerkudaraDNSSlave

```
echo 'zone "baratayuda.abimanyu.d11.com" {
    type master;
    file "/etc/bind/baratayuda/baratayuda.abimanyu.d11.com";
};'  > /etc/bind/named.conf.local

mkdir /etc/bind/baratayuda

cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.d11.com

echo '$TTL      604800
@       IN      SOA     baratayuda.abimanyu.d11.com.    root.baratayuda.abimanyu.d11.com. (
                        2022100601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800  )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.d11.com.
@       IN      A       10.27.1.4       ; IP Abimanyu
www     IN      CNAME   baratayuda.abimanyu.d11.com.
@       IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.d11.com

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no; # conform to RFC1035
        listen-on-v6 { any; };

};' > /etc/bind/named.conf.options

service bind9 restart
```

setelah selesai setup di DNS Master dan DNS Slave lakukan ping domain di client untuk membuktikan setup sudah benar
```
ping baratayuda.abimanyu.d11.com -c 5
ping www.baratayuda.abimanyu.d11.com -c 5
```

<img width="519" alt="9" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/031bdc1a-0ecd-4bd2-a65a-953b94653f0d">

## Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Answer :
Lakukan setup pada node WerkudaraDNSSlave 

```
echo '$TTL      604800
@       IN      SOA     baratayuda.abimanyu.d11.com.    root.baratayuda.abimanyu.d11.com. (
                        2022100601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800  )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.d11.com.
@       IN      A       10.27.1.4       ; IP Abimanyu
www     IN      CNAME   baratayuda.abimanyu.d11.com.
rjp     IN      A       10.27.1.4       ; IP Abimanyu
www.rjp IN      A       10.27.1.4
@       IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.d11.com
```

setelah selesai setup di DNS Slave lakukan ping domain di client untuk membuktikan setup sudah benar

```
ping rjp.baratayuda.abimanyu.d11.com -c 5
ping www.rjp.baratayuda.abimanyu.d11.com -c 5
```

<img width="521" alt="10" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/d7430c46-8931-4b62-951d-1d91d4482de8">

## Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Answer :
Lakukan setup pada ArjunaLoadBalancer

```
echo '
server {
 listen 80;
 server_name arjuna.d11.com www.arjuna.d11.com;

 location / {
 proxy_pass http://myweb;
        }
}' > /etc/nginx/sites-available/lb-jarkom


ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled

rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```

Lakukan setup pada masing masing worker yaitu AbimanyuWebServer, PrabukusumaWebServer, WisanggeniWebServer

```
service php7.0-fpm start

echo 'server {

 listen 80;

 root /var/www/jarkom;

 index index.php index.html index.htm;
 server_name _;

 location / {
 try_files $uri $uri/ /index.php?$query_string;
 }

 # pass PHP scripts to FastCGI server
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 }

 location ~ /\.ht {
 deny all;
 }

 error_log /var/log/nginx/jarkom_error.log;
 access_log /var/log/nginx/jarkom_access.log;
 }' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled

apt-get update && apt-get install wget && apt-get install unzip

wget --no-check-certificate "https://drive.google.com/uc?export=download&id=17tAM_XDKYWDvF-JJix1x7txvTBEax7vX" -O /var/$

unzip /var/www/jarkom/arjuna.d11.com.zip -d /var/www/jarkom/
mv /var/www/jarkom/arjuna.yyy.com/index.php /var/www/jarkom/

rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```

setelah melakukan setup lakukan test pada client

```
lynx http://10.27.1.4
lynx http://10.27.1.5
lynx http://10.27.1.6
lynx http://arjuna.d11.com
```
<img width="863" alt="11" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/7a25737f-7a4b-407c-b195-4ffb44bb4165">

<img width="854" alt="12" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/995acfbe-617d-481a-abe1-3ac45c7034a6">

<img width="862" alt="13" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/c3f038cd-2cba-45ff-9663-2899a45b6f73">


## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh

- Prabakusuma:8001
- Abimanyu:8002
- Wisanggeni:8003

Answer : 
Lakukan setup tambahan pada ArjunaLoadBalancer

```
echo '
# Default menggunakan Round Robin
upstream myweb  {
        server 10.27.1.4:8001; #IP Abimanyu
 server 10.27.1.5:8002; #IP Prabukusuma
        server 10.27.1.6:8003; #IP Wisanggeni
}

server {
 listen 80;
 server_name arjuna.d11.com www.arjuna.d11.com;

 location / {
 proxy_pass http://myweb;
        }
}' > /etc/nginx/sites-available/lb-jarkom
```

Lakukan setup tambahan pada tiap worker, pada `listen 800X` di ubah menyesuaikan port tiap worker. Pada praktikum kali ini kami mengatur port 8001 Abimanyu, 8002 Prabukusuma, 8003 Wisanggeni

```
echo 'server {

 listen 800X;

 root /var/www/jarkom;

 index index.php index.html index.htm;
 server_name _;

 location / {
 try_files $uri $uri/ /index.php?$query_string;
 }

 # pass PHP scripts to FastCGI server
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 }

 location ~ /\.ht {
 deny all;
 }

 error_log /var/log/nginx/jarkom_error.log;
 access_log /var/log/nginx/jarkom_access.log;
 }' > /etc/nginx/sites-available/jarkom
```

setelah melakukan setup lakukan test pada client

```
lynx http://10.27.1.4:8001
lynx http://10.27.1.5:8002
lynx http://10.27.1.6:8003
lynx http://arjuna.d11.com
```
<img width="856" alt="14" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/3a9384f3-a3d8-4767-b90b-a91b0a87f601">

<img width="860" alt="15" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/a902b7c6-0e68-4fc2-a758-d83d7aea5a2e">

<img width="854" alt="16" src="https://github.com/gracetrianaa/Jarkom-Modul-2-D11-2023/assets/130858750/bc019c34-c64c-4561-81b3-46344c732cd3">
