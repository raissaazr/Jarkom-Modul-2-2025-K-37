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

## Soal 4
## Soal 5
## Soal 6
## Soal 7
## Soal 8
## Soal 9
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
