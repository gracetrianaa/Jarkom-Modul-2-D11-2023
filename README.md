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

## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
