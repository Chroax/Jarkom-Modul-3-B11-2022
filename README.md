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

![image](https://i.ibb.co/pKHz4VD/topografi.png)

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

Setelah melakukakn install dhcp server, ubah config interface di isc-dhcp-server menjadi eth0.
```INTERFACES=\"eth0\"```

Kemudian lakukan ```service isc-dhcp-server start```.


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

Lakukan  ```service squid status``` untuk mengecek status dhcp di berlint.

> Script dibawah ini terdapat pada **root node Berlint**, untuk menjalankannya bisa langsung dengan melakukan command `bash no1.sh`

- **Berlint**
    ```
    service squid status
    ```

### Test

![image](https://i.ibb.co/NNdfYCX/Capture1.png)


## Question 2

> dan Ostania sebagai DHCP Relay

### Script

1. Lakukan setup DHCP Relay di Ostania ```isc-dhcp-relay initscript```, ```/etc/init.d/isc-dhcp-relay```
2. Pasang DHCP Relay dari ```/etc/init.d/isc-dhcp-relay``` ke server yang dituju dan interfacesnya.
3. Lakukan ```service isc-dhcp-relay start```

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

Pada file no3-6.sh, di switch3 isi dengan range yang diminta.
```range 192.178.1.50 192.178.1.88;
   range 192.178.1.120 192.178.1.155;
```

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

Lakukan tes di SSS & Garden.

- **SSS & Garden**

    ```
    ip a
    ```

### Test

![image](https://i.ibb.co/tBH44mm/Capture1.png)


## Question 4

> Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti. Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

### Script

Pada file no3-6.sh, di switch3 isi dengan range yang diminta.
```range 192.178.3.10 192.178.3.30;
   range 192.178.3.60 192.178.3.85;
```

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

Lakukan test di Eden, NewstonCastle & KemonoPark.

- **Eden, NewstonCastle & KemonoPark**

    ```
    ip a
    ```

### Test

![image](https://i.ibb.co/fqVCLvG/Capture1.png)


## Question 5

> Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### Script

Pada WISE, buat file no5.sh lalu tambahkan konfigurasi forwarders. Selanjutnya start bind9 ```service bind9 start```.

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

Untuk mengetest, lakukakn ping ke google.com. Jika tersambung maka sudah berhasil.

> Script dibawah ini terdapat pada **root node Eden, NewstonCastle, KemonoPark, SSS, & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no5.sh`

- **Eden, NewstonCastle, KemonoPark, SSS, & Garden**

    ```
    cat /etc/resolv.conf
    echo -e "\n"
    ping google.com -c 3
    ```

### Test

![image](https://i.ibb.co/nj66hz2/Capture1.png)

![image](https://i.ibb.co/tH2hQtj/Capture2.png)


## Question 6

> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

### Script

Tambahkan kode berikut pada Switch1 
```default-lease-time 300;
   max-lease-time 6900;```
   
Tambahkan kode berikut pada Switch3
```default-lease-time 600;
   max-lease-time 6900;
```

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

Lakukan `service isc-dhcp-server restart`.

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

Lakukan `/etc/network/interfaces`.

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

![image](https://i.ibb.co/sHC9zGk/Capture1.png)


## Question 8-9

> SSS, Garden, dan Eden digunakan sebagai client Proxy agar pertukaran informasi dapat terjamin keamanannya, juga untuk mencegah kebocoran data. Pada Proxy Server di Berlint, Loid berencana untuk mengatur bagaimana Client dapat mengakses internet. Artinya setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh). Adapun pada hari dan jam kerja sesuai nomor (8), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)

### Script

Lakukan `/etc/squid/squid.conf`, lalu `service squid restart`.

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

Lakukan `/etc/bind/wise/franky-work.com` dan `/etc/bind/wise/loid-work.com`. Lalu `service bind9 restart`.

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

Lakukan `service apache2 start`

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

Lakukan `/var/www/loid-work.com`, lalu `service apache2 restart`.

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

Lakukan `install -y lynx`.

> Script dibawah ini terdapat pada **root node SSS & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash install.sh`

- **SSS & Garden***

    ```
    apt-get update
    apt-get install -y lynx
    echo "" | apt install speedtest-cli
    export PYTHONHTTPSVERIFY=0
    ```

Lakukan `http://192.178.2.3:8080`.

> Script dibawah ini terdapat pada **root node Eden, SSS, & Garden**, untuk menjalankannya bisa langsung dengan melakukan command `bash no8.sh`

- **SSS & Garden***

    ```
    export http_proxy="http://192.178.2.3:8080"
    env | grep -i proxy
    ```

### Test
Lakukan `ping loid-work.com` dan `ping franky-work.com`.

![image](https://i.ibb.co/QKH8JHc/Capture1.png)

Lalu, akan muncul seperti di gambar berikut.

![image](https://i.ibb.co/7yhfZtp/Capture2.png)

Lakukan `env | grep -i proxy`.

![image](https://i.ibb.co/5Wn0MMD/Capture3.png)

Lalu, akan muncul seperti di gambar berikut.

![image](https://i.ibb.co/1KhfdGZ/Capture4.png)

Setelah itu, akan muncul tulisan dari `http://its.ac.id`

![image](https://i.ibb.co/3f9xsZ0/Capture5.png)

![image](https://i.ibb.co/kgMX1bm/Capture6.png)

![image](https://i.ibb.co/pbJCtxP/Capture7.png)

Lalu, jika memasukan link lain akan muncul seperti di gambar berikut.

![image](https://i.ibb.co/H4CsgXg/Capture8.png)

![image](https://i.ibb.co/Fx9zRKH/Capture9.png)


## Question 10

> Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Script

Lakukan `/etc/squid/squid.conf`, lalu `service squid restart`.

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

Setelah itu, akan muncul seperti di gambar berikut.

![image](https://i.ibb.co/3RYSFxn/Capture1.png)


## Question 11-12

> Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps). Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur

### Script

Lakukan `/etc/squid/squid.conf`, lalu `service squid restart`.

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

Lakukan `monday-test1.sh`.

![image](https://i.ibb.co/R9CsV86/Capture1.png)

Lakukan `monday-test2.sh`.

![image](https://i.ibb.co/T01dP74/Capture2.png)

Lakukan `speedtest`.

![image](https://i.ibb.co/mvKwyts/Capture3.png)


## Kendala

- Kendala 1

- Kendala 2
