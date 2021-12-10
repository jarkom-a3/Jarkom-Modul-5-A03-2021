# Jarkom-Modul-5-A03-2021

## Anggota

| Nama | NRP |
| :----: | :----: |
| Jessica Tasyanita | 05111940000043 |
| Daniel Sugianto | 05111940000075 |
| Ryan Fernaldy | 05111940000152 |

## Soal A
Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Luffy dibawah ini:<br>
![image](https://user-images.githubusercontent.com/68326540/145647812-93a4d668-12bf-40cd-9a18-8ef393d07a38.png)
### Kendala
Tidak ada

## Soal B
### VLSM
- Pembagian subnetting
  ![image](https://user-images.githubusercontent.com/62937814/145019988-cd6ee2b9-493f-40f1-836f-0eb280f1180a.png)

  ![image](https://user-images.githubusercontent.com/68326540/145649231-b95a4fac-b839-48a8-86bb-3bb093a3e611.png)

- Tree
![image](https://user-images.githubusercontent.com/62937814/145020116-cf9424e1-8c85-4682-aaa5-7aa0204a91b0.png)
### Kendala
Tidak ada


## Soal C
Kalian juga diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.
- Konfigurasi GNS3 & Routing
  - FOOSHA
    ```
    auto eth0
    iface eth0 inet static
      address 192.168.122.2
      netmask 255.255.255.252
      gateway 192.168.122.1

    auto eth1
    iface eth1 inet static
        address 192.170.7.145
        netmask 255.255.255.252

    auto eth2
    iface eth2 inet static
        address 192.170.7.149
        netmask 255.255.255.252
    ```
    Routing
    ```
    route add -net 192.170.7.0 netmask 255.255.255.128 gw 192.170.7.146
    route add -net 192.170.0.0 netmask 255.255.252.0 gw 192.170.7.146
    route add -net 192.170.7.128 netmask 255.255.255.248 gw 192.170.7.146

    route add -net 192.170.4.0 netmask 255.255.254.0 gw 192.170.7.150
    route add -net 192.170.6.0 netmask 255.255.255.0 gw 192.170.7.150
    route add -net 192.170.7.136 netmask 255.255.255.248 gw 192.170.7.150
    ```
    
  - WATER7
    ```
    auto eth0
    iface eth0 inet static
        address 192.170.7.146
        netmask 255.255.255.252
        gateway 192.170.7.145

    auto eth1
    iface eth1 inet static
        address 192.170.7.1
        netmask 255.255.255.128

    auto eth2
    iface eth2 inet static
        address 192.170.7.129
        netmask 255.255.255.248

    auto eth3
    iface eth3 inet static
        address 192.170.0.1
        netmask 255.255.252.0
    ```
    Routing
    ```
    route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.170.7.145
    ```
    
  - GUANHAO
    ```
    auto eth0
    iface eth0 inet static
        address 192.170.7.150
        netmask 255.255.255.252
        gateway 192.170.7.149

    auto eth1
    iface eth1 inet static
        address 192.170.4.1
        netmask 255.255.254.0

    auto eth2
    iface eth2 inet static
        address 192.170.7.137
        netmask 255.255.255.248

    auto eth3
    iface eth3 inet static
        address 192.170.6.1
        netmask 255.255.255.0
    ```
    Script
    ```
    route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.170.7.149
    ```
  - DORIKI
    ```
    auto eth0
    iface eth0 inet static
        address 192.170.7.130
        netmask 255.255.255.248
        gateway 192.170.7.129
    ```
  - JIPANGU
    ```
    auto eth0
    iface eth0 inet static
        address 192.170.7.131
        netmask 255.255.255.248
        gateway 192.170.7.129
    ```
  - JORGE
    ```
    auto eth0
    iface eth0 inet static
      address 192.170.7.138
      netmask 255.255.255.248
      gateway 192.170.7.137
    ```
  - MAINGATE
    ```
    auto eth0
    iface eth0 inet static
      address 192.170.7.139
      netmask 255.255.255.248
      gateway 192.170.7.137
    ```
  - Hasil
    Ping dari JIPANGU ke JORGE<br>
    ![image](https://user-images.githubusercontent.com/68326540/145647979-c7b4ef91-8cc4-4c7f-9171-5e13a1d47bed.png)
    Ping dari BLUENO ke MAINGATE<br>
    ![image](https://user-images.githubusercontent.com/68326540/145648012-879ce4f0-c299-42ee-b3c1-abc7cf8c44b3.png)
### Kendala
Tidak ada

## Soal D
Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.
- JIPANGU (DHCP Server)
  - Instalasi
    ```
    apt-get update
    apt-get install isc-dhcp-server -y
    service isc-dhcp-server start
    ```
  - Tambahkan interface di /etc/default/isc-dhcp-server
    ```
    INTERFACES="eth0"
    ```
  - Tambahkan konfigurasi di /etc/dhcp/dhcpd.conf
    ```
    subnet 192.170.7.128 netmask 255.255.255.248 {
      option routers 192.170.7.129;
    }

    subnet 192.170.7.0 netmask 255.255.255.128 {
      range 192.170.7.1 192.170.7.126;
      option routers 192.170.7.1;
      option broadcast-address 192.170.7.127;
      option domain-name-servers 192.170.7.130;
      default-lease-time 360;
      max-lease-time 7200;
    }

    subnet 192.170.0.0 netmask 255.255.252.0 {
      range 192.170.0.1 192.170.3.254;
      option routers 192.170.0.1;
      option broadcast-address 192.170.3.255;
      option domain-name-servers 192.170.7.130;
      default-lease-time 360;
      max-lease-time 7200;
    }

    subnet 192.170.4.0 netmask 255.255.254.0 {
            range 192.170.4.1 192.170.5.254;
            option routers 192.170.4.1;
            option broadcast-address 192.170.5.255;
            option domain-name-servers 192.170.7.130;
            default-lease-time 360;
            max-lease-time 7200;
    }


    subnet 192.170.6.0 netmask 255.255.255.0 {
            range 192.170.6.1 192.170.6.254;
            option routers 192.170.6.1;
            option broadcast-address 192.170.6.255;
            option domain-name-servers 192.170.7.130;
            default-lease-time 360;
            max-lease-time 7200;
    }
    ```
  - Restart
    ```
    service isc-dhcp-server restart
    ```
- WATER7 & GUANHAO (DHCP Relay)
  - Instalasi
    ```
    apt-get update
    apt-get install isc-dhcp-relay -y
    ```
  - Konfigurasi /etc/default/isc-dhcp-relay
    ```
    # Defaults for isc-dhcp-relay initscript
    # sourced by /etc/init.d/isc-dhcp-relay
    # installed at /etc/default/isc-dhcp-relay by the maintainer scripts

    #
    # This is a POSIX shell fragment
    #

    # What servers should the DHCP relay forward requests to?
    SERVERS="192.170.7.131"

    # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
    INTERFACES="eth0 eth1 eth2 eth3"

    # Additional options that are passed to the DHCP relay daemon?
    OPTIONS=""

    ```
  - Restart
    ```
    service isc-dhcp-relay restart
    ```
- Konfigurasi GNS3 Client BLUENO, CIPHER, ELENA, FUKUROU
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
- Hasil<br>
  ![image](https://user-images.githubusercontent.com/68326540/144961452-5b77e43b-1d4b-4669-b356-66d6a41c4793.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144961473-1cc4a172-668e-4628-93d6-21c8da474564.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144961489-6a3a5110-fe27-4f9c-8764-63c1fe503306.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144961506-ca7da406-136d-4060-bc2f-dd02a4af98af.png)<br>
### Kendala
Tidak ada

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
- FOOSHA
  - Konfigurasi GNS3
    ```
    auto eth0
    iface eth0 inet static
      address 192.168.122.2
      netmask 255.255.255.252
      gateway 192.168.122.1
    ```
  - Jalankan
    ```
    iptables -t nat -A POSTROUTING -s 192.170.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.2
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    ```
- WATER7, GUANHAO, DORIKI, JIPANGU, JORGE, MAINGATE
  ```
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- DORIKI
  - Instalasi DNS
    ```
    apt-get update
    apt-get install bind9 -y
    service bind9 start
    ```
  - Tambahkan perintah berikut pada /etc/bind/named.conf.options
    ```
     forwarders {
              192.168.122.1;
      };
      allow-query{any;};
    ```
  - Komen baris
    ```
    //dnssec-validation auto;
    ```
  - Hasil
    ![image](https://user-images.githubusercontent.com/68326540/145648102-93d80abe-b8d5-4a95-aa2b-045ef94a808a.png)
### Kendala
Tidak ada

## Soal 2
### Kendala
Tidak ada

## Soal 3
### Kendala
Tidak ada

## Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.
- DORIKI
  ```
  iptables -A INPUT -s 192.170.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
  iptables -A INPUT -s 192.170.7.0/25 -j REJECT
  
  iptables -A INPUT -s 192.170.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
  iptables -A INPUT -s 192.170.0.0/22 -j REJECT
  ```
- Hasil<br>
  BLUENO Ping google.com pada hari senin 6 Desember 2021 pukul 10:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983304-fcaa4506-1c64-4570-a661-7f1883f15042.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983328-22118b92-7d2e-4f45-9795-c79c082cfe44.png)<br>

  BLUENO Ping google.com pada hari Jumat 3 Desember 2021 pukul 18:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983529-afdf7968-8239-440b-be53-0573530b88a4.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983445-bafb228a-90ff-4148-9dda-032ae67289f3.png)

  CIPHER Ping google.com pada hari senin 6 Desember 2021 pukul 10:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983304-fcaa4506-1c64-4570-a661-7f1883f15042.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983648-c558b78e-932c-4767-b034-1ded3184c1fd.png)

  CIPHER Ping google.com pada hari Jumat 3 Desember 2021 pukul 18:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983529-afdf7968-8239-440b-be53-0573530b88a4.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144983674-d7ab79d9-3f5d-4ea7-beef-c509fb4c084d.png)
### Kendala
Tidak ada

## Soal 5
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
- DORIKI
  ```
  iptables -A INPUT -s 192.170.4.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
  iptables -A INPUT -s 192.170.4.0/23 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
  iptables -A INPUT -s 192.170.4.0/23 -j REJECT
  
  iptables -A INPUT -s 192.170.6.0/24 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
  iptables -A INPUT -s 192.170.6.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
  iptables -A INPUT -s 192.170.6.0/24 -j REJECT
  ```
- Hasil<br>
  ELENA Ping google.com pada hari minggu 5 Desember 2021 pukul 19:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984092-e47ef542-5aef-480c-8a97-dba6b7e2ab1e.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984330-3fd77835-54c3-4171-894b-4c512bb4f444.png)<br>

  ELENA Ping google.com pada hari senin 6 Desember 2021 pukul 10:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984390-8e52768a-676c-4ec6-bb13-c12b59c9b5b0.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984361-773fb341-5df3-4c89-95d2-b28ac17c495e.png)<br>

  
  FUKUROU ping google.com pada hari minggu 5 Desember 2021 pukul 19:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984095-93bf93e3-b528-4da1-8538-3cbf570fe696.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984428-085aeb93-cc54-4f5d-9ecc-f0c595c82724.png)<br>


  FUKUROU Ping google.com pada hari senin 6 Desember 2021 pukul 10:00<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984406-03785074-3bdf-4665-8317-d8efc67db12b.png)<br>
  ![image](https://user-images.githubusercontent.com/68326540/144984449-199a99c4-cecd-45bb-8f98-b88223f22d6b.png)<br>
### Kendala
Tidak ada

## Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate
- MAINGATE & JORGE<br>
  Instalasi Web Server
  ```
  apt-get update
  apt-get install apache2 -y

  service apache2 start
  ```
- DORIKI
  - Tambahkan konfigurasi berikut /etc/bind/named.conf.local (kita menggunakan domain jualbelikapal.A03.com)
    ```
    zone "jualbelikapal.A03.com" {
      type master;
      file "/etc/bind/jarkom/jualbelikapal.A03.com";
    };
    ```
  - Buat Folder
    ```
    mkdir /etc/bind/jarkom
    ```
  - Atur konfigurasi binding di /etc/bind/jarkom/jualbelikapal.A03.com. Arahkan domain tersebut ke IP address pada jaringan A8 yang belum dipakai (192.170.7.140)
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     jualbelikapal.A03.com. root.jualbelikapal.A03.com. (
                                    2       ; Serial
                            604800          ; Refresh
                            86400           ; Retry
                            2419200         ; Expire
                            604800 )        ; Negative Cache TTL
    ;
    @       IN      NS      jualbelikapal.A03.com.
    @       IN      A       192.170.7.140
    ```
  - Restart
    ```
    service bind9 restart
    ```
 - GUANHAO
   Tambahkan rule iptables berikut (Tambahkan PREROUTING untuk setiap paket dengan tujuan 192.170.7.140 pada port 80 maka akan diarahkan ke JORGE dan MAINGATE secara bergantian dan POSTROUTING untuk setiap paket dari JORGE ke MAINGATE diubah source-nya menjadi 192.170.7.140)
   ```
   iptables -A PREROUTING -t nat -p tcp -d 192.170.7.140 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.170.7.138

   iptables -A PREROUTING -t nat -p tcp -d 192.170.7.140 --dport 80 -j DNAT --to-destination 192.170.7.139

   iptables -t nat -A POSTROUTING -p tcp -s 192.170.7.138 --dport 80 -j SNAT --to-source 192.170.7.140

   iptables -t nat -A POSTROUTING -p tcp -s 192.170.7.139 --dport 80 -j SNAT --to-source 192.170.7.140
   ```
- Hasil
  lynx jualbelikapal.A03.com dari BLUENO yang pertama (dilayani JORGE)<br>
  ![image](https://user-images.githubusercontent.com/68326540/145648780-16df7e6b-028a-4112-bf4f-d85197cfbc3b.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648815-d3619b7d-ad16-49ca-be14-845636928bc0.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648830-5acb73b2-ffe2-48ac-9731-ca9ef26b3ee2.png)
  <br>
  lynx jualbelikapal.A03.com dari BLUENO yang kedua (dilayani MAINGATE)<br>
  ![image](https://user-images.githubusercontent.com/68326540/145648862-14dc19a3-b616-4768-a9a7-c82baa1967b2.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648890-60917687-d41e-46a6-8e94-6e632c6df88b.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648898-80f4b599-3ed7-4523-8ec8-5ad676625576.png)
  <br>
  lynx jualbelikapal.A03.com dari BLUENO yang ketiga (dilayani JORGE)<br>
  ![image](https://user-images.githubusercontent.com/68326540/145648962-5ddc3f5f-5648-44a8-8d90-3a6abe0a8891.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648976-dddeeb0d-ce8b-4210-bee1-d78720e042a4.png)
  ![image](https://user-images.githubusercontent.com/68326540/145648978-2936a40f-4554-4b68-b025-46caca2ea897.png)
  
### Kendala
Tidak ada




  
