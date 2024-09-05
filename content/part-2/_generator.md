+++

title = "Basi di Linux, secondo modulo"
description = "Corso Linux Base @SCM Group"
outputs = ["Reveal"]
aliases = [
  "1"
]

[reveal_hugo]
custom_theme = "martina-theme.scss"
custom_theme_compile = true

+++

# BASI DI LINUX, SECONDO MODULO

## [Martina Baiardi](mailto:m.baiardi@unibo.it)

### Compiled {{% today %}}

---

## Gestione dei dispositivi in Linux

<div class='multiCol'>
<div class='col text-center'>

![../images/linux-device-drivers.svg](../images/linux-device-drivers.svg)

</div>
<div class='col'>

La filosofia *"everything is a file"* si applica anche per i dispositivi connessi alla macchina,
infatti esistono dei file speciali nella cartella */dev* che sono la rappresentazione del file system dei dispositivi.

Essendo dei file, il loro contenuto può essere analizzato come se fossero come tutti gli altri (utilizzando comandi come `cat`, `ls`, ...),
però essi rappresentano delle interfacce di basso livello che le applicazioni utente possono utilizzare per interagire con i dispositivi con operazioni di I/O. 

</div>
</div>

---

## Gestione dei dispositivi in Linux

```console
$ ls -alh /dev
total 4,0K
drwxr-xr-x  21 root root            4,3K  5 set 12.54 .
drwxr-xr-x  18 root root            4,0K  4 set 15.30 ..
[...]
drwxr-xr-x   3 root root           60 21 ago 09.36 bus
drwxr-xr-x   2 root root         4,8K  5 set 17.26 char
[...]
drwxr-xr-x   4 root root          400  5 set 17.26 input
[...]
crw-rw-rw-   1 root root     1,     3  2 set 14.47 null
crw-------   1 root root   241,     0  2 set 14.47 nvme0
brw-rw----   1 root disk   259,     0  2 set 14.47 nvme0n1
brw-rw----   1 root disk   259,     1  2 set 14.47 nvme0n1p1
brw-rw----   1 root disk   259,     2  2 set 14.47 nvme0n1p2
brw-rw----   1 root disk   259,     3  2 set 14.47 nvme0n1p3
[...]
crw-rw-rw-   1 root root     1,     8  2 set 14.47 random
[...]
brw-rw----   1 root disk     8,     0  2 set 14.47 sda
brw-rw----   1 root disk     8,     1  2 set 14.47 sda1
brw-rw----   1 root disk     8,     2  2 set 14.47 sda2
[...]
```

All'interno della cartella */dev* risiedono due categorie di file: 
- **Character special files (c)**: I/O basato sullo scambio di singoli caratteri (di dimensione 1 byte). Fanno parte di questa categoria dispotivi come *mouse*, *tastiere* e *schede audio*.
- **Block special files (b)**: I/O basato sullo scambio di *blocchi* di dati. Fanno parte di questa categoria *usb* e *hard disk*.

---

#### Hard Disk

Gli hard disk sono associati al file con prefisso *sd*. 

- `/dev/sda` rappresenta il primo hard disk, `/dev/sbd` il secondo, e così via. 

In caso il dispositivo presenta delle **partizioni**, esse sono rappresentate con un numero posto accanto al nome del dispositivo.

- `/dev/sda1` è la prima partizione, `/dev/sda2` la seconda, e così via.

---

## Partizioni di un disco

TODO()

---

#### Pseudo-dispositivi

Sono dispositivi che non possiedono un componente fisico collegato al pc, ma offrono funzionalità implementate direttamente a livello di sistema operativo.
Alcuni esempi:
- `/dev/random`: Produce uno stream infinito di bytes con valori *pseudo-casuali*. Vengono utilizzati per scopi crittografici.

> Un computer è una macchina che segue delle regole per svolgere le sue funzioni, di conseguenza, anche se in grado di produrre numeri che all'apparenza sembrano casuali, essi non sono totalmente impredicibili come eventi naturali (ad esempio, il rumore atmosferico), per questo motivo vengono definiti come pseudo-casuali.

- `/dev/null`: Accetta qualsiasi tipo di input e lo scarta, senza produrre nessun output. L'utilizzo più comune per questo comando è quello di ignorare degli stream in uscita prodotti da processi.
Un esempio:

```console
$ cat /dev/random > /dev/null
```

- `/dev/zero`: Accetta qualsiasi tipo di input e lo scarta, producendo come output uno stream continuo di NULL (0 bytes). Utile per creare nuovi file contenenti unicamente dei byte a zero, con il fine di pre-allocare lo spazio sul disco.


---

Marti potresti partire da qui quando fai il file system UNIX

Dato che tutto è un file, *anche i dispositivi sono rappresentati come file*, e sono *"montati"* in una directory del file system.

immagine con struttura file system

---

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
