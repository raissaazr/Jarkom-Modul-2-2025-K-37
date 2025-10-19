# Jarkom-Modul-2-2025-K-37
| Nama                         | NRP        |
| ---------------------------- | ---------- |
| Azaria Raissa Maulidinnisa   | 5027241043 |
| Daniswara Fausta Novanto     | 5027241050 |

## Soal 1
Buat topologi jaringan sesuai soal, di mana terdapat satu router dengan tiga jalur yang akan dilalui, di Barat ada node Earendil dan Elwing, di Timur ada Cirdan, Elrond, dan Maglor, serta di Pelabuhan DMZ terdapat Sirion, Tirion, Valmar, Lindon, dan Vingilot. 

<img width="476" height="452" alt="Screenshot 2025-10-12 143410" src="https://github.com/user-attachments/assets/a1606f57-891b-4f0e-be73-118a9b4e6a28" />

Setelahnya, atur alamat IP dan gateway masing-masing node. 
```
# Eonwe 
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.82.1.1 
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.82.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.82.3.1
	netmask 255.255.255.0

# Earendil
auto eth0
iface eth0 inet static
	address 10.82.1.2  
	netmask 255.255.255.0
	gateway 10.82.1.1  

# Elwing
auto eth0
iface eth0 inet static
	address 10.82.1.3  
	netmask 255.255.255.0
	gateway 10.82.1.1  

# Cirdan
auto eth0
iface eth0 inet static
	address 10.82.2.2  
	netmask 255.255.255.0
	gateway 10.82.2.1  

# Elrond
auto eth0
iface eth0 inet static
	address 10.82.2.3  
	netmask 255.255.255.0
	gateway 10.82.2.1  

# Maglor
auto eth0
iface eth0 inet static
	address 10.82.2.4  
	netmask 255.255.255.0
	gateway 10.82.2.1  

# Sirion
auto eth0
iface eth0 inet static
	address 10.82.3.2  
	netmask 255.255.255.0
	gateway 10.82.3.1  

# Tirion
auto eth0
iface eth0 inet static
	address 10.82.3.3  
	netmask 255.255.255.0
	gateway 10.82.3.1  

# Valmar
auto eth0
iface eth0 inet static
	address 10.82.3.4  
	netmask 255.255.255.0
	gateway 10.82.3.1  

# Lindon
auto eth0
iface eth0 inet static
	address 10.82.3.5  
	netmask 255.255.255.0
	gateway 10.82.3.1  

# Vingilot
auto eth0
iface eth0 inet static
	address 10.82.3.6  
	netmask 255.255.255.0
	gateway 10.82.3.1
```
Setelah mengatur konfigurasi awal, coba hubungkan ke router Eonwe. 

<img width="804" height="189" alt="Screenshot 2025-10-13 110651" src="https://github.com/user-attachments/assets/308f4ec6-f522-4111-adcb-817339acb5b6" />

Kemudian, hubungkan ke salah satu klien dari setiap jalur yang dimiliki Eonwe, misal Earendil, Cirdan dan Sirion. Di masing-masing terminal jalankan ``ip route show`` untuk melihat default gateway, lalu lakukan perintah ``ping`` untuk memeriksa apakah router sudah tersambung.

<img width="660" height="339" alt="Screenshot 2025-10-13 110819" src="https://github.com/user-attachments/assets/178dd01f-4d62-40c0-b9b1-a48746544d1a" />

<img width="679" height="341" alt="Screenshot 2025-10-13 110835" src="https://github.com/user-attachments/assets/7c579eb6-32cd-4638-a682-3ed74f3b426b" />

<img width="657" height="344" alt="Screenshot 2025-10-13 110847" src="https://github.com/user-attachments/assets/94231893-9ed8-458f-b0b0-c02b914d4f31" />

## Soal 2
Untuk melihat status IP forwarding di kernel, lakukan perintah ``cat /proc/sys/net/ipv4/ip_forward``. Jika hasil mengembalikan ``1``,  maka berarti router meneruskan paket yang masuk ke interface A, dan ditujukan ke jaringan di interface B. 

<img width="519" height="43" alt="Screenshot 2025-10-13 110749" src="https://github.com/user-attachments/assets/255a937b-bb4c-4a68-a7ab-90708c3e4631" />

Agar seluruh alamat internal dapat mencapai alamat luar, lakukan konfigurasi ``iptables`` di Eonwe. 
```
apt update && apt install iptables -y
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.82.0.0/16
```
Kemudian di node klien tambahkan konfigurasi.
```
echo "nameserver 192.168.122.1" > /etc/resolv.conf
```
Untuk memastikan apakah sudah terhubung ke luar, lakukan ``ping`` ke ``google.com``. 

<img width="867" height="182" alt="Screenshot 2025-10-13 111837" src="https://github.com/user-attachments/assets/94cdf716-6f09-4482-ae9d-554a36a0fb72" />

<img width="870" height="186" alt="Screenshot 2025-10-13 112210" src="https://github.com/user-attachments/assets/e25dc6d9-c93b-4064-bd0e-cb964609bdb3" />

## Soal 3
Untuk membuktikan bahwa kabar dari barat sampai ke timur, lakukan percobaan ``ping`` dari klien barat yaitu Earendil ke klien timur. 
```
# Di Earendil
ping 10.82.2.2
ping 10.82.2.3
ping 10.82.2.3
```

<img width="669" height="366" alt="Screenshot 2025-10-13 112750" src="https://github.com/user-attachments/assets/b7732e86-f1c6-4b97-8e8a-c41c889dd757" />

Setelahnya agar setiap hos non-router dapat mengakses internet sejak awal, tambahkan ``up echo "nameserver 192.168.122.1" > /etc/resolv.conf`` di konfigurasi. 

## Soal 4
Membangun infrastruktur DNS Authoritative yang redundant untuk zona ``K37.com`` dengan Tirion sebagai Master (ns1) dan Valmar sebagai Slave (ns2), serta memastikan semua host menggunakan kedua server ini sebagai resolver utama. Sebelum melakukan konfigurasi, install ``bind9`` di Tirion dan Valmar.
```
apt update && apt install bind9 -y
```

Konfigurasi DNS Master di Tirion ``10.82.3.3``, sumber utama data zona ``K37.com`` dan mengizinkan transfer ke Slave (Valmar).
```
nano /etc/bind/named.conf.local

options {
    directory "/var/cache/bind";
    forwarders {
        192.168.122.1;  # Forwarding ke resolver eksternal (internet)
    };
    allow-query{any;};
    listen-on { 10.82.3.3; 127.0.0.1; }; # Listen hanya di IP Tirion
};

// Definisi Zona Master
zone "K37.com" {
    type master;
    file "/etc/bind/db.K37.com";
    allow-transfer { 10.82.3.4; };  # Mengizinkan transfer zona hanya ke Valmar
    notify yes;                    # Memberitahu Slave (Valmar) jika ada update
};
```

Setelahnya buat file baru ``nano /etc/bind/db.K37.com`` untuk mengatur A record. 
```
$TTL    604800
@       IN      SOA     ns1.K37.com. root.K37.com. (
                              2025101301 ; Serial (Harus dinaikkan jika ada perubahan!)
                              604800     ; Refresh
                               86400     ; Retry
                            2419200      ; Expire
                              604800 )   ; Negative Cache TTL

@       IN      NS      ns1.K37.com.
@       IN      NS      ns2.K37.com.

; A Record untuk Nameserver
ns1     IN      A       10.82.3.3
ns2     IN      A       10.82.3.4

; A Record Apex (Front Door)
@       IN      A       10.82.3.2
```

Setelahnya, simpan file konfigurasi dan jalankan ``named-checkzone K37.com /etc/bind/db.K37.com`` untuk memeriksa sintaks file zona ``db.K37.com``, ``named-checkconf`` untuk memeriksa sintaks file konfigurasi utama ``BIND``, dan ``/etc/init.d/named restart`` untuk memuat ulang layanan ``BIND9`` agar konfigurasi Master diterapkan.</br>
Lakukan konfigurasi DNS Master yang sama di Valmar dengan mengganti bagian zona master menjadi untuk slave. 
```
options {
    directory "/var/cache/bind";
    forwarders {
        192.168.122.1; # Forwarding ke resolver eksternal (internet)
    };
    allow-query{any;};
    listen-on { 10.82.3.4; 127.0.0.1; }; # Listen hanya di IP Valmar
};

zone "K37.com" {
    type slave;
    file "db.K37.com";
    masters { 10.82.3.3; }; # Menentukan IP Master (Tirion)
};
```

Setelahnya jalankan juga ``named-checkconf`` dan ``/etc/init.d/named restart``. Untuk verifikasi internal jalankan ``dig @127.0.0.1 K37.com`` yang harus menampilkan jawaban dengan
status ``ANSWER SECTION`` yang berisi A Record Apex, dan flag ``aa (Authoritative Answer)``. Selanjutnya, setelah memastikan konfigurasi berjalan baik, langkah yang harus dilakukan 
adalah mengubah urutan resolver klien untuk memastikan host menggunakan server DNS lokal yang redundant sebelum beralih ke resolver eksternal. Jalankan di terminal klien. 
```
sudo apt update 
sh -c 'echo "nameserver 10.82.3.3" > /etc/resolv.conf'      # NS1 (Tirion) - Primary
sh -c 'echo "nameserver 10.82.3.4" >> /etc/resolv.conf'     # NS2 (Valmar) - Secondary
sh -c 'echo "nameserver 192.168.122.1" >> /etc/resolv.conf' # Resolver Eksternal
```

<img width="781" height="70" alt="Screenshot 2025-10-13 124710" src="https://github.com/user-attachments/assets/1a194b7b-61e0-44f9-9c86-e944a51432c5" />

Setelahnya, lakukan varifikasi konektivitas dan resolusi DNS dengan ``dig K37.com`` dan ``dig google.com``.

<img width="733" height="445" alt="Screenshot 2025-10-13 125046" src="https://github.com/user-attachments/assets/cfdf33bd-0ba9-4b19-b90a-bb1e03fd4da4" />

## Soal 5
Menetapkan hostname sistem yang unik untuk setiap host (Eonwë, Earendil, Elwing, Cirdan, dll.). Membuat DNS Record yang sesuai untuk setiap host tersebut dalam zona ``K37.com``. Memastikan hostname yang baru diatur dikenali oleh DNS Authoritative (ns1 di Tirion dan ns2 di Valmar). Di setiap host yang bersangkutan (Eonwë, Earendil, Elwing, Cirdan, Elrond, Maglor, Sirion, Tirion, Valmar, Lindon, Vingilot), jalankan perintah ``hostname`` untuk menetapkan nama sistem.
```
hostname eonwe
hostname 
```

Edit file zona master ``/etc/bind/db.K37.com`` di Tirion untuk menambahkan A Record untuk semua host baru.
```
nano /etc/bind/db.K37.com

 2025101301 ; Serial --> 2025101202 ; Serial (WAJIB DITINGKATKAN)

eonwe  IN      A        10.82.3.1
earendil IN      A       10.82.1.2
elwing  IN      A       10.82.1.3
cirdan  IN      A       10.82.2.2
elrond  IN      A       10.82.2.3
maglor  IN      A       10.82.2.4
sirion IN       A       10.82.3.2
tirion  IN      A       10.82.3.3
valmar  IN      A       10.82.3.4
lindon  IN      A       10.82.3.5
vingilot IN      A       10.82.3.6
```

Lalu, jalankan ``named-checkzone K37.com /etc/bind/db.K37.com`` untuk memverifikasi sintaks file zona yang baru ditambahkan, pastikan output mengembalikan ``OK``.

<img width="609" height="82" alt="Screenshot 2025-10-13 125147" src="https://github.com/user-attachments/assets/22c970e7-5608-4738-a89c-641a9c9472de" />

Kemudian ``restart BIND`` di tiorion dan valmar ``/etc/init.d/named restart``. Wajib dilakukan di Tirion untuk memuat data baru. Valmar akan menarik zona secara otomatis melalui mekanisme Notify dan Transfer DNS. Untuk verifikasi lakukan ``dig elwing.K37.com`` (bisa host lain juga) dari host Earendil yang harus menampilkan status ``NOERROR``.

<img width="725" height="436" alt="Screenshot 2025-10-13 133747" src="https://github.com/user-attachments/assets/0f76de33-88bf-4fd0-82a0-9786223e79c3" />

## Soal 6
Memastikan proses Zone Transfer dari server DNS Master (Tirion) ke server Slave (Valmar) berhasil. Keberhasilan transfer ditandai dengan kesamaan nilai Serial SOA (Start of Authority) di kedua server. Setiap kali data zona ``db.K37.com`` di Master diubah (seperti penambahan host record di Soal 5), nilai Serial wajib dinaikkan. Slave menggunakan nilai Serial ini untuk mengetahui apakah ia perlu menarik salinan zona yang baru dari Master. Nilai Serial yang sama di kedua server menunjukkan zona telah tersinkronisasi. Jalankan di tirion ``dig @127.0.0.1 K37.com SOA`` untuk memeriksa nilai Serial terbaru pada server Master. 

<img width="897" height="465" alt="Screenshot 2025-10-13 135038" src="https://github.com/user-attachments/assets/cccd391d-f971-4395-bc0a-04f0503669bb" />

Lalu, jalankan perintah yang sama di Valmar. 

<img width="903" height="471" alt="Screenshot 2025-10-13 135058" src="https://github.com/user-attachments/assets/2ca0ee84-221a-4a8b-a2f2-9908ba1bf6a9" />

Seperti dapat dilihat di answer section bahwa keduanya menunjukkan angka SOA yaitu ``2025101303`` yang berarti transfer dikeduanya sudah berhasil tersinkronisasi. 

## Soal 7
Memetakan layanan utama jaringan menggunakan CNAME record yang fleksibel, dan memverifikasi resolusi yang konsisten dari dua klien berbeda. Nilai Serial SOA di Master dan Slave harus tetap sinkron setelah perubahan. Saat mengedit ulang konfigurasi di file ``/etc/bind/db.K37.com`` pastikan untuk selalu menaikkan nilai SOA setiap kali akan melakukan perubahan. CNAME Record digunakan untuk memberikan nama alias ke A Record yang sudah ada, mempermudah manajemen IP di masa depan.
```
; CNAME Records
www     IN      CNAME   sirion.K37.com.  
static  IN      CNAME   lindon.K37.com.
app     IN      CNAME   vingilot.K37.com.
```

Setelahnya, jalankan lagi ``named-checkzone K37.com /etc/bind/db.K37.com`` dan restart BIND ``/etc/init.d/named restart`` di tirion dan valmar. Kemudian, verifikasi dilakukan dari dua klien berbeda misalnya Earendil dan satu host lain, misal Cirdan untuk memastikan konsistensi.

<img width="664" height="427" alt="Screenshot 2025-10-13 140320" src="https://github.com/user-attachments/assets/8afce882-ca5e-4398-a8cd-3de59b8b56f7" />

<img width="722" height="455" alt="Screenshot 2025-10-13 140403" src="https://github.com/user-attachments/assets/f582e721-3576-43e8-8a6e-919f8a761297" />

## Soal 8
Mendeklarasikan dan mengisi zona Reverse DNS untuk segmen DMZ ``10.82.3.x`` di mana Sirion, Lindon, dan Vingilot berada. Ini memungkinkan Reverse Lookup, pencarian balik IP ke hostname dan memastikan Slave (Valmar) berhasil menarik zona tersebut dari Master (Tirion). Di tirion, edit konfigurasi file ``/etc/bind/named.conf.local`` dan tambahkan konfigurasi berikut di paling bawah. 
```
zone "3.82.10.in-addr.arpa" {
    type master;
    file "/etc/bind/db.10.82.3";
    allow-transfer { 10.82.3.4; };
    notify yes;
};
```

Kemudian, lanjutkan dengan pembuatan file data ``db.10.82.3``.
```
nano /etc/bind/db.10.82.3

$TTL    604800
@       IN      SOA     ns1.k37.com. root.k37.com. (
                              2025101301 ; Serial
                              604800     ; Refresh
                               86400     ; Retry
                            2419200      ; Expire
                              604800 )   ; Negative Cache TTL

@       IN      NS      ns1.k37.com.
@       IN      NS      ns2.k37.com.

; PTR Records untuk Host Server di DMZ
2       IN      PTR     sirion.k37.com.    ; 10.82.3.2
5       IN      PTR     lindon.k37.com.    ; 10.82.3.5
6       IN      PTR     vingilot.k37.com.  ; 10.82.3.6
```

Lalu, jalankan ulang verifikasi sintaks dan restart ulang layanan di tirion. Selanjutnya deklarasi zona reverse di Valmar hanya perlu diatur sebagai Slave yang menunjuk ke Master (Tirion).
```
nano /etc/bind/named.conf.local

zone "3.82.10.in-addr.arpa" {
    type slave;
    file "db.10.82.3";
    masters { 10.82.3.3; }; // Tirion (ns1) adalah Master
};
```

Lalu, memulai BIND9 di Slave, yang akan otomatis menarik zona reverse dari Tirion. Verifikasi dilakukan dengan menggunakan ``dig -x`` (pencarian reverse) ke IP target, dan memastikan hasilnya dijawab oleh kedua server DNS Authoritative (ns1 dan ns2).

<img width="661" height="404" alt="Screenshot 2025-10-13 142456" src="https://github.com/user-attachments/assets/13fb9691-8793-4207-ad1e-596d5b8acd67" />

## Soal 9
Mengaktifkan web server Apache2 di Lindon ``static.K37.com`` dan mengonfigurasi Virtual Host untuk melayani directory listing (autoindex) pada path ``/annals/``. Langkah awal adalah instalasi web server dan pembuatan struktur direktori untuk menampung arsip. Di lindon jalankan :
```
apt update && apt install apache2 -y

# Buat direktori arsip yang diminta
mkdir -p /var/www/K37.com/annals

# Buat beberapa file dummy untuk menguji directory listing
touch /var/www/K37.com/annals/arsip1.txt
touch /var/www/K37.com/annals/dokumen_penting.pdf

# Atur kepemilikan agar apache dapat membacanya
chown -R www-data:www-data /var/www/K37.com
```

Kemudian, konfigurasi virtual host ``static.K37.com.conf`` mendefinisikan cara Apache merespons request untuk hostname ini. 
```
nano /etc/apache2/sites-available/static.K37.com.conf

<VirtualHost *:80>
    ServerName static.K37.com
    ServerAdmin webmaster@K37.com
    DocumentRoot /var/www/K37.com

    <Directory /var/www/K37.com/annals/>
        Options +Indexes
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Direktif Options ``+Indexes`` di dalam tag ``<Directory>`` untuk ``/annals/`` adalah yang memaksa Apache menampilkan daftar file (autoindex) jika tidak ada index file (seperti index.html) yang ditemukan. Kemudian jalankan ``a2ensite static.K37.com.conf`` untuk mengaktifkan Virtual Host yang baru dibuat dan ``/etc/init.d/apache2 restart`` menjalankan ulang untuk menerapkan konfigurasi Virtual Host Apache yang baru. </br>
Kemudian untuk Verifikasi Akhir Di Klien Earendil, jalankan ``dig static.K37.com`` dengan hasil ideal Resolusi A Record berhasil.

<img width="651" height="423" alt="Screenshot 2025-10-13 140331" src="https://github.com/user-attachments/assets/5a3759b0-a94b-4bca-9eee-122d729273b4" />

Kemudian jalankan ``curl static.K37.com/annals/`` untuk Menguji akses melalui hostname.

## Soal 10
## Soal 11
## Soal 12
## Soal 13
## Soal 14
## Soal 15
## Soal 16
## Soal 17
## Soal 18
## Soal 19
## Soal 20
