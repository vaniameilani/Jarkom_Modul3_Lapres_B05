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
- Lalu, edit file seperti dibawah ini :
[IMG relaySBY]

### 3. client SUBNET 1 (GRESIK & SIDOARJO) mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
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
- 
