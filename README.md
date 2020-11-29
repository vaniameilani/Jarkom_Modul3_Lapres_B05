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
<img src="https://user-images.githubusercontent.com/61272072/100545059-813cfc80-328c-11eb-9dce-71d40512843d.PNG" width="500" height="auto">

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
<img src="https://user-images.githubusercontent.com/61272072/100545336-f826c500-328d-11eb-97a1-bd77f3e1819c.PNG" width="500" height="auto">
<img src="https://user-images.githubusercontent.com/61272072/100545338-f957f200-328d-11eb-938f-f994b22c991f.PNG" width="500" height="auto">

b. DNS Server MALANG
<img src="https://user-images.githubusercontent.com/61272072/100545334-f65d0180-328d-11eb-8cd3-8156ea1683c9.PNG" width="500" height="auto">

c. DHCP Server TUBAN
<img src="https://user-images.githubusercontent.com/61272072/100545343-fbba4c00-328d-11eb-901b-7b44213cbe4d.PNG" width="500" height="auto">

d. Proxy Server MOJOKERTO
<img src="https://user-images.githubusercontent.com/61272072/100545333-f4933e00-328d-11eb-9147-40b9102a537d.PNG" width="500" height="auto">

e. Client GRESIK
<img src="https://user-images.githubusercontent.com/61272072/100545331-f1984d80-328d-11eb-95a4-6117b4984c2e.PNG" width="500" height="auto">

f. Client SIDOARJO
<img src="https://user-images.githubusercontent.com/61272072/100545340-fa891f00-328d-11eb-895b-9af392f91626.PNG" width="500" height="auto">

g. Client BANYUWANGI
<img src="https://user-images.githubusercontent.com/61272072/100545330-ef35f380-328d-11eb-97f8-63584252de46.PNG" width="500" height="auto">

h. Client MADIUN
<img src="https://user-images.githubusercontent.com/61272072/100545332-f3621100-328d-11eb-83c1-fc86ba5a4224.PNG" width="500" height="auto">

- Setelah itu di-restart dengan perintah `service networking interfaces`.
- Lalu, cek dengan menggunakan `ifconfig` dan hasil nya seperti berikut ini :<br>
  <img src="https://user-images.githubusercontent.com/61272072/100545206-530bec80-328d-11eb-8d47-4c26b422f8db.PNG" width="500" height="auto">
  <img src="https://user-images.githubusercontent.com/61272072/100545203-51422900-328d-11eb-91a2-cb931ca60d6d.PNG" width="500" height="auto">
  <img src="https://user-images.githubusercontent.com/61272072/100545198-4b4c4800-328d-11eb-9472-21dde2a69c41.PNG" width="500" height="auto">

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
