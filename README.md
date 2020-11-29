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
[IMG]

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
[IMG "Interface SBYa] 
[IMG "Interface SBYb]

b. DNS Server MALANG
[IMG "interface MLG"]

c. DHCP Server TUBAN
[IMG "interface TUBAN]

d. Proxy Server MOJOKERTO
[IMG "interface MJK]

e. Client GRESIK
[IMG "interface GRESIK]

f. Client SIDOARJO
[IMG "interface SRJ]

g. Client BANYUWANGI
[IMG "interface BWI]

h. Client MADIUN
[IMG "interface MDN]

- Setelah itu di-restart dengan perintah `service networking interfaces`.
- Lalu, cek dengan menggunakan `ifconfig` dan hasil nya seperti berikut ini :<br>
  [IMG ifconfig SBY]
  [IMG ifconfig MLG]
  [IMG ifconfig GRESIK]

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
[IMG relaySBY]

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
[IMG secrver dhcp config]

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
[IMG statisGRESIK]

- Restart network dengan perintah `service networking restart`
[IMG client_GRESIK]

- Cek kembali **IP GRESIK** dengan menjalankan `ifconfig`
[IMG client_ifconfiGRESIK]

- Periksa juga apakah **GRESIK** sudah mendapatkan DNS server sesuai konfigurasi di DHCP. Periksa `/etc/resolv.conf` dengan menggunakan perintah
```
cat /etc/resolv.conf
```
[IMG clientGRESIK_nameserver]

- Lakukan kembali langkah - langkah di atas pada client :<br>
a.**SIDOARJO**
[IMG clientSIDOARJO]
[IMG clientSIDOARJO_ifconfig]
[IMG clientSIDOARJO2]
<br>
b.**MADIUN**
[IMG clientMADIUN]
[IMG clientMADIUN_ifconfig]
[IMG clientMADIUN2]
<br>
c.**BANYUWANGI**
[IMG BANYUWANGI ifconfig]
[IMG BANYUWANGI ifconfig2]
[IMG BANYUWANGI ifconfig3]

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

- Lalu, buat konfigurasi baru dengan mengetikkan `nano /etc/squid/squid.conf`. 
Kemudian, pada file config yang baru, edit file seperti ini   :
<img src="https://user-images.githubusercontent.com/61219556/100545916-00343400-3291-11eb-9f44-24df058f7021.PNG" width="500" height="auto">

- Restart Squid dengan perintah `service squid restart`
<https://user-images.githubusercontent.com/61219556/100545919-032f2480-3291-11eb-8b3b-7f4dc3ef66e6.PNG" width="500" height="auto">




