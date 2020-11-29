# Jarkom_Modul3_Lapres_B05
- Vania Meilani Taqiyyah  05111840000045
- Elvira Catrine Natalie  05111840000016

## TEKNIS PENGERJAAN
1. Default memori UML adalah 64M, kecuali :<br>
  a. SURABAYA   : 256M</br>
  b. MALANG     : 160M<br>
  c. MOJOKERTO  : 128M<br>
  d. TUBAN      : 128M</br>
2. Menghitung dan menggunakan IP sesuai dengan NID Tuntap dan NID DMZ masing-masing kelompok.<br>
3. IP Tuntap : NID_tuntap_tiap_kelompok + 1 = 10.151.74.25
4. IP Interfaces Router SURABAYA :<br>
  a. eth0 : NID_tuntap_tiap_kelompok + 2 = 10.151.74.26<br>
  b. eth1 : 192.168.0.1	=> CLIENT SUBNET 1<br>
  c. eth2 : 192.168.1.1	=> CLIENT SUBNET 3<br>
  d. eth3 : NID_DMZ_tiap_kelompok + 1 = 10.151.83.49 => SERVER SUBNET 2</br>
5. IP Server (SUBNET 2) :<br>
  a. MALANG 	: NID_DMZ_tiap_kelompok + 2 = 10.151.83.50<br>
  b. MOJOKERTO	: NID_DMZ_tiap_kelompok + 3 = 10.151.83.51<br>
  c. TUBAN	: NID_DMZ_tiap_kelompok + 4 = 10.151.83.52

## SOAL
<img src="https://user-images.githubusercontent.com/61219556/100546834-f103b500-3295-11eb-850a-f664d9f6fced.PNG" width="500" height="auto">

### 1. Membuat Topologi Jaringan berdasarkan dengan kriteria :<br>
a. SURABAYA                                 = Router<br>
b. MALANG                                   = DNS Server<br>
c. TUBAN                                    = DHCP Server<br>
d. MOJOKERTO                                = Proxy Server<br>
e. BANYUWANGI, MADIUN, SIDOARJO, dan GRESIK = Client

Proses pembuatannya :<br>
- Membuat **topologi.sh** pada Putty seperti pada gambar di bawah ini :<br>
<img src="https://user-images.githubusercontent.com/61219556/100544052-c3af0b00-3285-11eb-9a82-60f35edb4611.PNG" width="500" height="auto">

- Lalu, di `bash topologi.sh`.
- Tahap-tahap selanjutnya sama dengan modul Pengenalan UML. Perbedaannya adalah isi config dari `nano /etc/network/interfaces` pada setiap UML.<br>
a. Router SURABAYA
<img src="https://user-images.githubusercontent.com/61219556/100546811-ddf0e500-3295-11eb-8bf8-748743e268d0.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546813-de897b80-3295-11eb-9c9e-14dd0e57c2ca.PNG" width="500" height="auto">

b. DNS Server MALANG
<img src="https://user-images.githubusercontent.com/61219556/100546798-d6c9d700-3295-11eb-854c-efaa5b7683fe.PNG" width="500" height="auto">

c. DHCP Server TUBAN
<img src="https://user-images.githubusercontent.com/61219556/100546817-e0533f00-3295-11eb-9f3a-b816abe8ad9e.PNG" width="500" height="auto">

d. Proxy Server MOJOKERTO
<img src="https://user-images.githubusercontent.com/61219556/100546806-db8e8b00-3295-11eb-837e-ec039839c852.PNG" width="500" height="auto">

e. Client GRESIK
<img src="https://user-images.githubusercontent.com/61219556/100546804-d9c4c780-3295-11eb-844a-6b38e868de9f.PNG" width="500" height="auto">

f. Client SIDOARJO
<img src="https://user-images.githubusercontent.com/61219556/100546815-dfbaa880-3295-11eb-848a-706723555701.PNG" width="500" height="auto">

g. Client BANYUWANGI
<img src="https://user-images.githubusercontent.com/61219556/100546803-d92c3100-3295-11eb-9a7f-46b48f2a0c9b.PNG" width="500" height="auto">

h. Client MADIUN
<img src="https://user-images.githubusercontent.com/61219556/100546805-da5d5e00-3295-11eb-9ce1-c04d73bb3443.PNG" width="500" height="auto">

- Setelah itu di-restart dengan perintah `service networking interfaces`.
- Lalu, cek dengan menggunakan `ifconfig` dan hasil nya seperti berikut ini :<br>
  <img src="https://user-images.githubusercontent.com/61219556/100546909-78512880-3296-11eb-8394-8daef92e21c8.PNG" width="500" height="auto">
  <img src="https://user-images.githubusercontent.com/61219556/100546906-76876500-3296-11eb-9811-930964f38163.PNG" width="500" height="auto">
  <img src="https://user-images.githubusercontent.com/61219556/100546904-75563800-3296-11eb-8bdf-fae3a5f87145.PNG" width="500" height="auto">

- Untuk mengakses jaringan keluar, ketik perintah `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada UML SURABAYA.

### 2. SURABAYA dijadikan sebagai DHCP RELAY.
- Untuk penggunaannya, perlu install DHCP Relay pada UML SURABAYA.<br>
a. Update package lists di router **SURABAYA** dengan perintah
```
apt-get update
```
b. Install **isc-dhcp-relay** di router **SURABAYA** dengan perintah 
```
apt-get install isc-dhcp-relay
```

- Buka file konfigurasi interface dengan perintah 
```
nano /etc/default/isc-dhcp-relay
```
Lalu, edit file seperti dibawah ini :
 <img src="https://user-images.githubusercontent.com/61219556/100546929-9dde3200-3296-11eb-98d0-c8a213f8e1a5.PNG" width="500" height="auto">

### 3-6
- Pada **TUBAN**, install DHCP Server dengan update package lists terlebih dahulu 
```
apt-get update
```
```
apt-get install isc-dhcp-server
```
- Lalu, buka file `nano /etc/default/isc-dhcp-server` dan edit pada **INTERFACES** dengan 
```
INTERFACES="eth0"
```
- Buka file konfigurasi DHCP dengan perintah
```
nano /etc/dhcp/dhcpd.conf
```
- Tambahkan script berikut seperti dibawah ini
<img src="https://user-images.githubusercontent.com/61219556/100546953-b64e4c80-3296-11eb-9dcb-49c0c208a34a.PNG" width="500" height="auto">

Pada soal no.3, pada **SUBNET1** yaitu **SUBNET 192.168.0.0**, atur range dari `192.168.0.10` sampai `192.168.0.100` dan dari `192.168.0.110` sampai `192.168.0.200`.<br>
Pada soal no.4, pada **SUBNET3** yaitu **SUBNET 192.168.1.0**, atur range dari `192.168.1.50` sampai `192.168.1.70`.<br>
Pada soal no.5, kedua subnet bagian `option domain-name-servers` ditulis IP MALANG yaitu `10.151.83.50` dan DNS `202.46.129.2` dari DHCP.<br>
Pada soal no.6, pada **SUBNET1** pada `default-lease-time` nya 300 detik, sedangkan pada **SUBNET3** ditulis waktu 600 detik.

- Restart service `isc-dhcp-server` dengan perintah 
```
service isc-dhcp-server restart
```
- Buka `nano /etc/network/interfaces` untuk mengonfigurasi interface `Client GRESIK`.
- Comment atau hapus konfigurasi yang lama (konfigurasi IP statis). Lalu tambahkan 
```
auto eth0
iface eth0 inet dhcp
```
<img src="https://user-images.githubusercontent.com/61219556/100546985-f44b7080-3296-11eb-84b1-3d9cb18319ff.PNG" width="500" height="auto">

- Restart network dengan perintah `service networking restart`
<img src="https://user-images.githubusercontent.com/61219556/100546991-f9102480-3296-11eb-8788-413729c0b891.PNG" width="500" height="auto">

- Cek kembali **IP GRESIK** dengan menjalankan `ifconfig`
<img src="https://user-images.githubusercontent.com/61219556/100546992-fad9e800-3296-11eb-94b3-d580c8a1baf9.PNG" width="500" height="auto">

- Periksa juga apakah **GRESIK** sudah mendapatkan DNS server sesuai konfigurasi di DHCP. Periksa `/etc/resolv.conf` dengan menggunakan perintah
```
cat /etc/resolv.conf
```
<img src="https://user-images.githubusercontent.com/61219556/100546993-fb727e80-3296-11eb-8431-897a8ee3a6cc.PNG" width="500" height="auto">

- Lakukan kembali langkah - langkah di atas pada client :<br>
a.**SIDOARJO**
<img src="https://user-images.githubusercontent.com/61219556/100546997-ff060580-3296-11eb-8f8a-4d7daf1a511b.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546998-ff9e9c00-3296-11eb-8b5a-06da56c7557d.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100547000-00373280-3297-11eb-98d5-21b4966653fd.PNG" width="500" height="auto">
<br>
b.**MADIUN**
<img src="https://user-images.githubusercontent.com/61219556/100546994-fca3ab80-3296-11eb-81f9-127007faeea7.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546995-fdd4d880-3296-11eb-803f-a06ba9116cd2.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546996-fe6d6f00-3296-11eb-99af-b05e2bbab246.PNG" width="500" height="auto">
<br>
c.**BANYUWANGI**
<img src="https://user-images.githubusercontent.com/61219556/100546986-f6153400-3296-11eb-90fd-a20fb14e27d4.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546987-f6adca80-3296-11eb-9383-7e5fe5a651e7.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546990-f8778e00-3296-11eb-9203-7dfd5d8976c3.PNG" width="500" height="auto">

### 7-11
- Install terlebih dahulu Squid pada **MOJOKERTO** 
```
apt-get install squid
```
- Cek status squid untuk memastikan bahwa Squid telah berjalan dengan baik
```
service squid status
```
<img src="https://user-images.githubusercontent.com/61219556/100545915-fe6a7080-3290-11eb-965c-aca1a726409c.PNG" width="500" height="auto">

- Backup terlebih dahulu file konfigurasi default yang disediakan Squid.
```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```
- buat konfigurasi baru dengan mengetikkan `nano /etc/squid/squid.conf`. 
Kemudian, pada file config yang baru dengan menambahkan isian :
```
http_port 8080
visible_hostname mojokerto
```
- Pada soal no.7 disuruh untuk membuat **username** dann **password** dengan format :<br>
Username 		: userta_yyy		= userta_b05<br>
Password	  : inipassw0rdta_yyy	= inipassw0rdta_b05<br>
Maka, install terlebih dahulu `apache2-utils` dengan perintah `apt-get install apache2-utils
`.
- Buat user dan password baru. Ketikkan:
```
htpasswd -c /etc/squid/passwd userta_b05
```
<img src="https://user-images.githubusercontent.com/61219556/100545918-00ccca80-3291-11eb-88c1-708a100ffce2.PNG" width="500" height="auto">

- Lalu, buka kembali file  `nano /etc/squid/squid.conf`. Kemudian, edit file seperti ini   :
<img src="https://user-images.githubusercontent.com/61219556/100545916-00343400-3291-11eb-9f44-24df058f7021.PNG" width="500" height="auto">

- Restart Squid dengan perintah `service squid restart`, dan hasilnya adalah  :
<img src="https://user-images.githubusercontent.com/61219556/100545919-032f2480-3291-11eb-8b3b-7f4dc3ef66e6.PNG" width="500" height="auto">

- Lalu pada soal no.8, diminta untuk set jadwal akses jaringan internet dengan menggunakan proxy tersebut pada hari **Selasa-Rabu jam 13:00-18:00**. Sedangkan pada soal no.9 diminta untuk set jadwal akses jaringan internet dengan menggunakan proxy tersebut pada hari **Selasa-Kamis jam 21:00-09.00 (sasmpai Jumat jam 09:00)** .
- Maka, buat file `nano /etc/squid/acl.conf` dengan isian sebagai berikut :
<img src="https://user-images.githubusercontent.com/61219556/100546298-32df2c00-3293-11eb-90d4-9769fe19b999.PNG" width="500" height="auto">

Maka, buka kembali file `nano /etc/squid/squid.conf` dan edit file seperti ini :
<img src="https://user-images.githubusercontent.com/61219556/100546296-31adff00-3293-11eb-8241-98fe003e7e1d.PNG" width="500" height="auto">

- Untuk soal no.10, saat akses **google.com**, web akan mendirect ke **monta.if.its.ac.id**, sehingga tambahkan konfigurasi pada file yang sama dengan   :
```
acl REDIRSITE dstdomain google.com
http_access deny REDIRSITE
deny_info http://monta.if.its.ac.id REDIRSITE
```
- Pada soal no.11, diminta untuk mengubah **ERROR PAGE DEFAULT SQUID** dengan unduh error page pada perintah `wget 10.151.36.202/ERR_ACCESS_DENIED` dan lakukan perintah `cp -r ERR_ACCESS_DENIED /usr/share/squid/errors/en/`
- Restart squid.
<img src="https://user-images.githubusercontent.com/61219556/100546441-e3e5c680-3293-11eb-9d06-bb81fb04bec1.PNG" width="500" height="auto">

- Dan hasilnya sebagai berikut :
<img src="https://user-images.githubusercontent.com/61219556/100546444-e516f380-3293-11eb-882a-ea5a754ad246.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61219556/100546445-e6e0b700-3293-11eb-8b94-9dd335f965a6.PNG" width="500" height="auto">

### 12. Membuat DNS dengan domain janganlupa-ta.yyy.pw dan port 8080.
- Buka **UML MALANG** dan update package list dengan command 
```
apt-get update
```
- Install aplikasi bind9 pada **UML MALANG** dengan 
 ```
 apt-get install bind9 -y
```
- Buka file dengan perintah `nano /etc/bind/named.conf.local`, lalu isi configurasi domain dengan syntax sebagai berikut : <br>
```
zone "janganlupa-ta.b05.pw" {
	type master;
	file "/etc/bind/jarkom/janganlupa-ta.b05.pw";
};
```
</br>
<img src="https://user-images.githubusercontent.com/61219556/100546705-5c995280-3295-11eb-83d0-35987cb8765f.PNG" width="500" height="auto">

- Buat folder jarkom di dalam /etc/bind dengan perintah 
```
mkdir /etc/bind/jarkom
```
- Copy file **db.local** ke dalam folder **jarkom** dan ubah namanya menjadi **janganlupa-ta.b05.pw**.
```
cp /etc/bind/db.local /etc/bind/jarkom/janganlupata-b05.pw
```
- Buka file **janganlupata-b05.pw** dengan perintah `nano /etc/bind/jarkom/janganlupata-b05.pw`. Lalu, edit file dengan memasukkan IP MOJOKERTO seperti berikut	:

<img src="https://user-images.githubusercontent.com/61219556/100546707-5e631600-3295-11eb-81f3-0e3087d1d179.PNG" width="500" height="auto">

- Restart bind9 untuk meng-update perubahan dengan perintah `service bind9 restart`.
<img src="https://user-images.githubusercontent.com/61219556/100546708-5e631600-3295-11eb-8bbe-ee32221582dc.PNG" width="500" height="auto">



