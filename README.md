# Jarkom-Modul-5-A03-2021

## Anggota

| Nama | NRP |
| :----: | :----: |
| Jessica Tasyanita | 05111940000043 |
| Daniel Sugianto | 05111940000075 |
| Ryan Fernaldy | 05111940000152 |

## Soal A

## Soal B
### VLSM
- Pembagian subnetting
![image](https://user-images.githubusercontent.com/62937814/145019988-cd6ee2b9-493f-40f1-836f-0eb280f1180a.png)

- Tree
![image](https://user-images.githubusercontent.com/62937814/145020116-cf9424e1-8c85-4682-aaa5-7aa0204a91b0.png)

![image](https://user-images.githubusercontent.com/62937814/145020197-5d583871-193e-41bf-b3cf-d788491d5b96.png)


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
## Soal 2

## Soal 3

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


## Soal 6
