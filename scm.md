# Corso Linux @SCM Group

## Base

### Parte 1 -- Danilo Pianini

* Rudimenti di sistemi operativi
* UNIX, POSIX, e il sistema operativo GNU/Linux
* Distribuzioni Linux
* Rudimenti del file system UNIX
    * Struttura del file system

* Terminali per sistemi POSIX: sh, bash, zsh

* Manipolazione di base del file system
    * Query
        * pwd
        * ls [-a]
        * cd
    * Modifica
        * touch
        * mkdir [-p]
    * Wildcards: ?, * e **

* Strumenti basilari del terminale
    * clear
    * echo
    * history e ^R

* Spostamento, copia, rimozione, visualizzazione di file e cartelle
    * cp
    * mv
    * rm [-rf]
    * cat
    * less
    * head [-n]
    * tail [-fn]
    * du
    * locate e find
    * which

* Redirezione
    * Standard output, input, ed error
    * standard output redirection con > e >>
    * standard input redirection con <
    * process piping (|)
    * filtraggio con grep [-vnc]

* Manualistica
    * whatis
    * man
    * apropos

* Utenti e gruppi
    * gruppi, utenti, permessi
    * permessi ottali
    * ls [-ahl]
    * whoami
    * who
    * chmod
    * chown
    * useradd
    * usermod
    * visudo

Processi, segnali, e loro gestione
    * PID
    * ps
    * top e htop
    * backgrounding (& e ^Z)
    * kill
    * killall

* Secure Shell
    * ssh
-    * autenticazione a chiavi assimetriche
-    * ssh-keygen
-    * il demone ssh
-    * ssh.conf e sshd.conf
    * scp
    * filezilla

Rudimenti di programmazione bash
    * variabili
    * script, shebang line
    * condizionali (if, until)
    * iterazione (for, while)
    * sort
    * sed

### Parte 2 -- Martina Baiardi

- Basic Linux Networking
  - LAN vs WAN
  - ISO OSI
- Layer 2
  - mac address
  - Loopback interface
  - Ethernet/Wi-Fi interfaces
- Layer 3
  - TCP/IPv4 addresses
    - format
    - netmasks
  - DHCP
  - NAT
  - DNS
  - NTP
  - Network manager
- Network configuration
    - ip command
    - systemd/networkd
    - nmcli command
    - /etc/network/interfaces
    - ping command
    - traceroute
    - nslookup
    - netstat
    - configurazione rete tramite dhcp
    - configurazione rete WPA2/WPA3
        - wpa_cli command
- Network tools
  - /etc/hosts
  - curl
  - wget

## Strumenti per analisi processi
- Foreground and Background processes
- Process states
- load average
- ps command
- top/htop command
- kill command

## Strumenti per analisi risorse di sistema

## Log di sistema
- log rotate e gestione dei log di systemctl (size, persistence time)
- /var/log
- journalctl


* Gestione di file systems
    * df
    * montaggio e smontaggio di partizioni (mount, umount)

## Utilizzo periferiche (es: porte USB) su Linux
- Rintracciare dispositivi esterni
- montare il percorso di tali dispositivi
- lsusb
- lspci

https://info-ee.surrey.ac.uk/Teaching/Unix/
