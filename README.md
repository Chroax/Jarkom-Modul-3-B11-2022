# Jarkom-Modul-3-B11-2022

### Kelompok B11

| **No** | **Nama**              | **NRP**    |
| ------ | --------------------- | ---------- |
| 1      | Cahyadi Surya Nugraha | 5025201184 |
| 2      | Sarah Alissa Putri    | 5025201272 |
| 3      | Ravin Pradhitya       | 5025201068 |

### List

1. [Soal 1](#Question-1)
2. [Soal 2](#Question-2)
3. [Soal 3](#Question-3)
4. [Soal 4](#Question-4)
5. [Soal 5](#Question-5)
6. [Soal 6](#Question-6)
7. [Soal 7](#Question-7)
8. [Soal 8-9](#Question-8-9)
9. [Soal 10](#Question-10)
10. [Soal 11-12](#Question-11-12)

## Topografi

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/topografi.PNG)

## Konfigurasi

- **Ostania**

    ```
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
    	address 192.178.1.1
    	netmask 255.255.255.0

    auto eth2
    iface eth2 inet static
    	address 192.178.2.1
    	netmask 255.255.255.0

    auto eth3
    iface eth3 inet static
    	address 192.178.3.1
    	netmask 255.255.255.0
    ```

- **WISE**

    ```
    auto eth0
    iface eth0 inet static
    	address 192.178.2.2
    	netmask 255.255.255.0
    	gateway 192.178.2.1
    ```

- **Berlint**

    ```
    auto eth0
    iface eth0 inet static
    	address 192.178.2.3
    	netmask 255.255.255.0
    	gateway 192.178.2.1
    ```

- **Westalis**

    ```
    auto eth0
    iface eth0 inet static
    	address 192.178.2.4
    	netmask 255.255.255.0
    	gateway 192.178.2.1
    ```

- **SSS**

    ```
    auto eth0
    iface eth0 inet dhcp
    ```

- **Garden**

    ```
    auto eth0
    iface eth0 inet dhcp
    ```

- **Eden**

    ```
    auto eth0
    iface eth0 inet dhcp
    ```

- **NewstonCastle**

    ```
    auto eth0
    iface eth0 inet dhcp
    ```

- **KemonoPark**

    ```
    auto eth0
    iface eth0 inet dhcp
    ```

## Initial Script

Pada initial project, kami mengubah `root/.bashrc` masing-masing node sehingga saat dijalankan akan langsung melakukan command berikut ini

- **Ostania**

    ```
    # ~/.bashrc: executed by bash(1) for non-login shells.
    # see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
    # for examples
    ...
    iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.178.0.0/16
    apt-get update
    echo "" | apt-get install isc-dhcp-relay -y
    ```

- **Wise**

    ```
    # ~/.bashrc: executed by bash(1) for non-login shells.
    # see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
    # for examples
    ...
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install bind9 -y
    ```

- **Berlint**

    ```
    # ~/.bashrc: executed by bash(1) for non-login shells.
    # see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
    # for examples
    ...
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install squid -y
    service squid start
    ```

- **Westalis**

    ```
    # ~/.bashrc: executed by bash(1) for non-login shells.
    # see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
    # for examples
    ...
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install isc-dhcp-server -y
    ```

## Question 1

> Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no1.sh`

- **Westalis**
    ```
    echo "# Defaults for isc-dhcp-server initscript
    # sourced by /etc/init.d/isc-dhcp-server
    # installed at /etc/default/isc-dhcp-server by the maintainer scripts

    #
    # This is a POSIX shell fragment
    #

    # Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
    #DHCPD_CONF=/etc/dhcp/dhcpd.conf

    # Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
    #DHCPD_PID=/var/run/dhcpd.pid

    # Additional options to start dhcpd with.
    #       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
    #OPTIONS=\"\"

    # On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
    #       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
    INTERFACES=\"eth0\"" > /etc/default/isc-dhcp-server

    service isc-dhcp-server start
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Berlint**, untuk menjalankannya bisa langsung dengan melakukan command `bash no1.sh`

- **Berlint**
    ```
    service squid status
    ```

### Test

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal1/Capture1.PNG)


## Question 2

> dan Ostania sebagai DHCP Relay

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Ostania**, untuk menjalankannya bisa langsung dengan melakukan command `bash no2.sh`

- **Ostania**
    ```
    echo -e '
    # Defaults for isc-dhcp-relay initscript
    # sourced by /etc/init.d/isc-dhcp-relay
    # installed at /etc/default/isc-dhcp-relay by the maintainer scripts

    #
    # This is a POSIX shell fragment
    #

    # What servers should the DHCP relay forward requests to?
    SERVERS="192.178.2.4"

    # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
    INTERFACES="eth1 eth2 eth3"

    # Additional options that are passed to the DHCP relay daemon?
    OPTIONS=""
    ' > /etc/default/isc-dhcp-relay

    service isc-dhcp-relay start
    ```


## Konfigurasi 3-6

### Script

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no3-6.sh`

- **Westalis**

    ```
    echo -e '
    subnet 192.178.1.0 netmask 255.255.255.0 {
        range 192.178.1.50 192.178.1.88;
        range 192.178.1.120 192.178.1.155;
        option routers 192.178.1.1;
        option broadcast-address 192.178.1.255;
        option domain-name-servers 192.178.2.2;
        default-lease-time 300;
        max-lease-time 6900;
    }

    subnet 192.178.3.0 netmask 255.255.255.0 {
        range 192.178.3.10 192.178.3.30;
        range 192.178.3.60 192.178.3.85;
        option routers 192.178.3.1;
        option broadcast-address 192.178.3.255;
        option domain-name-servers 192.178.2.2;
        default-lease-time 600;
        max-lease-time 6900;
    }

    subnet 192.178.2.0 netmask 255.255.255.0 {
        option routers 192.178.2.1;
    }
    ' > /etc/dhcp/dhcpd.conf

    service isc-dhcp-server restart
    service isc-dhcp-server status
    ```


## Question 3

> Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no3-6.sh`

- **Westalis**

    ```
    ...

    subnet 192.178.1.0 netmask 255.255.255.0 {
        range 192.178.1.50 192.178.1.88;
        range 192.178.1.120 192.178.1.155;
        option routers 192.178.1.1;
        option broadcast-address 192.178.1.255;
        option domain-name-servers 192.178.2.2;
    }

    ...
    ```

PENJELASAN

- **SSS & Garden**

    ```
    ip a
    ```

### Test

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal3/Capture1.PNG)


## Question 4

> Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no3-6.sh`

- **Westalis**

    ```
    ...

    subnet 192.178.3.0 netmask 255.255.255.0 {
        range 192.178.3.10 192.178.3.30;
        range 192.178.3.60 192.178.3.85;
        option routers 192.178.3.1;
        option broadcast-address 192.178.3.255;
        option domain-name-servers 192.178.2.2;
    }

    ...
    ```

PENJELASAN

- **Eden, NewstonCastle & KemonoPark**

    ```
    ip a
    ```

### Test

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal4/Capture1.PNG)


## Question 5

> Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node WISE**, untuk menjalankannya bisa langsung dengan melakukan command `bash no5.sh`

- **WISE**

    ```
    echo -e '
    options {
            directory "/var/cache/bind";

            // If there is a firewall between you and nameservers you want
            // to talk to, you may need to fix the firewall to allow multiple
            // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
               forwarders {
                    192.168.122.1;
               };

            //========================================================================
            // If BIND logs error messages about the root key being expired,
            // you will need to update your keys.  See https://www.isc.org/bind-keys
            //========================================================================
            //dnssec-validation auto;
            allow-query{any;};
            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
    };
    ' > /etc/bind/named.conf.options

    service bind9 start
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Eden, NewstonCastle, KemonoPark, SSS, & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no5.sh`

- **Eden, NewstonCastle, KemonoPark, SSS, & Garden**

    ```
    cat /etc/resolv.conf
    echo -e "\n"
    ping google.com -c 3
    ```

### Test

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal5/Capture1.PNG)

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal5/Capture2.PNG)


## Question 6

> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no3-6.sh`

- **Westalis**

    ```
    subnet 192.178.1.0 netmask 255.255.255.0 {
        ...
        default-lease-time 300;
        max-lease-time 6900;
    }

    subnet 192.178.3.0 netmask 255.255.255.0 {
        ...
        default-lease-time 600;
        max-lease-time 6900;
    }
    ```


## Question 7

> Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Westalis**, untuk menjalankannya bisa langsung dengan melakukan command `bash no7.sh`

- **Westalis**

    ```
    ...
    host Eden{
        hardware ethernet a6:09:e5:e8:1a:14;
        fixed-address 192.178.3.13;
    }
    ...

    service isc-dhcp-server restart
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Eden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no7.sh`

- **Eden**

    ```
    echo -e '
    auto eth0
    iface eth0 inet dhcp
    hwaddress ether a6:09:e5:e8:1a:14
    ' > /etc/network/interfaces
    ```

### Test

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal7/Capture1.PNG)


## Question 8-9

> SSS, Garden, dan Eden digunakan sebagai client Proxy agar pertukaran informasi dapat terjamin keamanannya, juga untuk mencegah kebocoran data. Pada Proxy Server di Berlint, Loid berencana untuk mengatur bagaimana Client dapat mengakses internet. Artinya setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh). Adapun pada hari dan jam kerja sesuai nomor (8), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Berlint**, untuk menjalankannya bisa langsung dengan melakukan command `bash no8-9.sh`

- **Berlint**

    ```
    echo -e '
    .franky-work.com
    .loid-work.com
    ' > /etc/squid/domain.acl

    echo -e '
    http_port 8080
    visible_hostname Berlint

    #---------------------------------------------------------
    #Time
    acl AVAILABLE_1 time MTWHF 00:00-07:59
    acl AVAILABLE_2 time MTWHF 17:01-23:59
    acl AVAILABLE_3 time AS 00:00-23:59

    acl WORK_TIME time MTWHF 08:00-17:00

    #---------------------------------------------------------
    #Domain
    acl RESTRICTED_DOMAIN dstdomain "/etc/squid/domain.acl"

    #---------------------------------------------------------

    http_access deny RESTRICTED_DOMAIN AVAILABLE_1
    http_access deny RESTRICTED_DOMAIN AVAILABLE_2
    http_access deny RESTRICTED_DOMAIN AVAILABLE_3

    http_access allow AVAILABLE_1
    http_access allow AVAILABLE_2
    http_access allow AVAILABLE_3

    http_access allow RESTRICTED_DOMAIN WORK_TIME

    ' > /etc/squid/squid.conf

    service squid restart
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node WISE**, untuk menjalankannya bisa langsung dengan melakukan command `bash no9.sh`

- **WISE**

    ```
    echo -e '
    zone "loid-work.com" {
        type master;
        file "/etc/bind/wise/loid-work.com";
    };

    zone "franky-work.com" {
        type master;
        file "/etc/bind/wise/franky-work.com";
    };
    ' > /etc/bind/named.conf.local

    mkdir /etc/bind/wise

    echo -e '
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     franky-work.com. root.franky-work.com. (
                            2022100601      ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      franky-work.com.
    @       IN      A       192.178.3.13
    www     IN      CNAME   franky-work.com.
    @       IN      AAAA    ::1
    ' > /etc/bind/wise/franky-work.com

    echo -e '
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     loid-work.com. root.loid-work.com. (
                            2022100601      ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      loid-work.com.
    @       IN      A       192.178.3.13
    www     IN      CNAME   loid-work.com.
    @       IN      AAAA    ::1
    ' > /etc/bind/wise/loid-work.com

    service bind9 restart
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Eden**, untuk menjalankannya bisa langsung dengan melakukan command `bash install.sh`

- **Eden**

    ```
    apt-get update
    echo "" | apt-get install apache2
    echo "" | apt-get install libapache2-mod-php7.0
    service apache2 start

    echo "" | apt-get install wget -y
    echo "" | apt-get install unzip -y
    echo "" | apt-get install php

    cd /var/www

    wget 'https://github.com/Chroax/Jarkom-Modul-2-B11-2022/raw/main/resources/wise.zip'

    unzip wise.zip

    rm wise.zip
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Eden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no9.sh`

- **Eden**

    ```
    echo "<VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/loid-work.com
            ServerName loid-work.com
            ServerAlias www.loid-work.com
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>" > /etc/apache2/sites-available/loid-work.com.conf

    a2ensite loid-work.com

    mkdir /var/www/loid-work.com

    cp -RT /var/www/wise /var/www/loid-work.com

    echo "<VirtualHost *:80>
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/franky-work.com
            ServerName franky-work.com
            ServerAlias www.franky-work.com

            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>" > /etc/apache2/sites-available/franky-work.com.conf

    a2ensite franky-work.com

    mkdir /var/www/franky-work.com

    cp -RT /var/www/wise /var/www/franky-work.com

    service apache2 restart
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node SSS & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash install.sh`

- **SSS & Garden***

    ```
    apt-get update
    apt-get install -y lynx
    echo "" | apt install speedtest-cli
    ```

PENJELASAN

> Script dibawah ini terdapat pada **root node Eden, SSS, & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no8.sh`

- **SSS & Garden***

    ```
    export http_proxy="http://192.178.2.3:8080"
    env | grep -i proxy
    ```

### Test
PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture1.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture2.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture3.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture4.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture5.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture6.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture7.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture8.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal8-9/Capture9.PNG)


## Question 10

> Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Berlint**, untuk menjalankannya bisa langsung dengan melakukan command `bash no10.sh`

- **Berlint**

    ```
    ...

    echo -e '
    http_port 8080
    visible_hostname Berlint

    #---------------------------------------------------------
    #Time
    acl AVAILABLE_1 time MTWHF 00:00-07:59
    acl AVAILABLE_2 time MTWHF 17:01-23:59
    acl AVAILABLE_3 time AS 00:00-23:59

    acl WORK_TIME time MTWHF 08:00-17:00

    #---------------------------------------------------------
    #Domain
    acl RESTRICTED_DOMAIN dstdomain "/etc/squid/domain.acl"

    #---------------------------------------------------------

    http_access deny RESTRICTED_DOMAIN AVAILABLE_1
    http_access deny RESTRICTED_DOMAIN AVAILABLE_2
    http_access deny RESTRICTED_DOMAIN AVAILABLE_3
    http_access deny all

    http_access allow AVAILABLE_1
    http_access allow AVAILABLE_2
    http_access allow AVAILABLE_3
    http_access deny all

    http_access allow RESTRICTED_DOMAIN WORK_TIME

    ' > /etc/squid/squid.conf

    service squid restart
    ```

### Test

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal10/Capture1.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal10/Capture2.PNG)


## Question 11-12

> Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps). Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur

### Script

PENJELASAN

> Script dibawah ini terdapat pada **root node Berlint**, untuk menjalankannya bisa langsung dengan melakukan command `bash no11-12.sh`

- **Eden**

    ```
    ...

    echo -e '
    ...
    #---------------------------------------------------------
    #Domain
    acl RESTRICTED_DOMAIN dstdomain "/etc/squid/domain.acl"

    #---------------------------------------------------------
    ...
    #---------------------------------------------------------
    #Limit Bandwith
    delay_pools 1
    delay_class 1 1
    delay_parameters 1 16000/16000
    delay_access 1 allow AVAILABLE_3

    #---------------------------------------------------------
    ...

    ' > /etc/squid/squid.conf

    service squid restart
    ```

### Test

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal11-12/Capture1.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal11-12/Capture2.PNG)

PENJELASAN

![image](https://raw.githubusercontent.com/Chroax/Jarkom-Modul-3-B11-2022/main/image/Soal11-12/Capture3.PNG)


## Kendala

- Kendala 1

- Kendala 2