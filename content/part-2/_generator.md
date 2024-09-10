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

# Macro argomenti

## 1. Gestione periferiche

## 2. Strumenti di analisi e gestione dei processi

## 3. Networking

---

## 1. Gestione periferiche

---


<div class='multiCol'>
<div class='col text-center'>

![../images/pci-bus.svg](../images/pci-bus.svg)

</div>
<div class='col'>

### Connessione delle periferiche

- Tutte le periferiche sono connesse tramite un bus standard *PCI* (*Peripheral Component Interconnect*)
- Alcune di esse comunicano direttamente con il bus, altre tramite i *controller*
- I controller regolano I/O con i dispositivi, alcuni sono direttamente integrati nella periferica (es. dischi), 
altri invece consentono di utilizzare protocolli standard per connettersi a periferiche esterne (es. controller USB)


**Ogni pc ha una diversa configurazione delle periferiche, l'immagine è solamente un esempio semplificato di una configurazione.**

</div>
</div>

---

### Analisi delle periferiche connesse al bus PCI

### `lspci`

```console
$ lspci
00:00.0 Host bridge: Intel Corporation Coffee Lake HOST and DRAM Controller (rev 0b)
00:02.0 VGA compatible controller: Intel Corporation WhiskeyLake-U GT2 [UHD Graphics 620]
00:04.0 Signal processing controller: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 0b)
00:08.0 System peripheral: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model
00:12.0 Signal processing controller: Intel Corporation Cannon Point-LP Thermal Controller (rev 30)
00:14.0 USB controller: Intel Corporation Cannon Point-LP USB 3.1 xHCI Controller (rev 30)
00:14.2 RAM memory: Intel Corporation Cannon Point-LP Shared SRAM (rev 30)
00:14.3 Network controller: Intel Corporation Cannon Point-LP CNVi [Wireless-AC] (rev 30)
00:15.0 Serial bus controller: Intel Corporation Cannon Point-LP Serial IO I2C Controller #0 (rev 30)
00:15.1 Serial bus controller: Intel Corporation Cannon Point-LP Serial IO I2C Controller #1 (rev 30)
00:16.0 Communication controller: Intel Corporation Cannon Point-LP MEI Controller #1 (rev 30)
00:1c.0 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #1 (rev f0)
00:1c.4 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #5 (rev f0)
00:1d.0 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #9 (rev f0)
00:1d.4 PCI bridge: Intel Corporation Cannon Point-LP PCI Express Root Port #13 (rev f0)
00:1e.0 Communication controller: Intel Corporation Cannon Point-LP Serial IO UART Controller #2 (rev 30)
00:1e.2 Serial bus controller: Intel Corporation Cannon Point-LP Serial IO SPI Controller (rev 30)
00:1f.0 ISA bridge: Intel Corporation Cannon Point-LP LPC Controller (rev 30)
00:1f.3 Audio device: Intel Corporation Cannon Point-LP High Definition Audio Controller (rev 30)
00:1f.4 SMBus: Intel Corporation Cannon Point-LP SMBus Controller (rev 30)
00:1f.5 Serial bus controller: Intel Corporation Cannon Point-LP SPI Controller (rev 30)
02:00.0 3D controller: NVIDIA Corporation GP108M [GeForce MX150] (rev a1)
04:00.0 Non-Volatile memory controller: Sandisk Corp WD Blue SN500 / PC SN520 x2 M.2 2280 NVMe SSD (rev 01)
```

### `lspci -tv`

Con l'aggiunta dei flag `-t` (visione ad albero) e `-v` (mostra tutte le informazioni) è possibile avere una visione gerarchica dei dispositivi connessi al bus PCI 

---

#### Perchè alcune periferiche sono assenti da questa lista? 

Perchè significa che tali dispositivi non sono direttamente connessi al bus, ma passano per un controller. 
Ad esempio, tutti i miei dispositivi USB sono connessi al *controller USB*, che a sua volta è connesso al bus pci, il quale infatti è presente nella lista sopra.

### `lsusb`

```console
$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 13d3:56cb IMC Networks USB2.0 HD IR UVC WebCam
Bus 001 Device 003: ID 8087:0aaa Intel Corp. Bluetooth 9460/9560 Jefferson Peak (JfP)
Bus 001 Device 013: ID ffff:5678 USB Disk 2.0
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 003: ID 152d:0578 JMicron Technology Corp. / JMicron USA Technology Corp. JMS578 SATA 6Gb/s
```

### `lsusb -tv`
```console
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/12p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 002: Dev 036, If 0, Class=Mass Storage, Driver=usb-storage, 480M
        ID ffff:5678 USB Disk 2.0
    |__ Port 005: Dev 002, If 0, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:56cb IMC Networks 
    |__ Port 005: Dev 002, If 1, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:56cb IMC Networks 
    |__ Port 005: Dev 002, If 2, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:56cb IMC Networks 
    |__ Port 005: Dev 002, If 3, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:56cb IMC Networks 
    |__ Port 010: Dev 003, If 0, Class=Wireless, Driver=btusb, 12M
        ID 8087:0aaa Intel Corp. Bluetooth 9460/9560 Jefferson Peak (JfP)
    |__ Port 010: Dev 003, If 1, Class=Wireless, Driver=btusb, 12M
        ID 8087:0aaa Intel Corp. Bluetooth 9460/9560 Jefferson Peak (JfP)
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/6p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
    |__ Port 001: Dev 005, If 0, Class=Mass Storage, Driver=uas, 5000M
        ID 152d:0578 JMicron Technology Corp. / JMicron USA Technology Corp. JMS578 SATA 6Gb/s
```


---



<div class='multiCol'>
<div class='col text-center'>

<img src="../images/linux-device-drivers.svg" width=80% />

</div>
<div class='col'>

### Driver 

I *driver* sono moduli software di basso livello che consentono al sistema operativo di pilotare un dispositivo hardware. 
Un insieme di driver standard sono già previsti all'interno del sistema operativo, per dispositivi particolari potrebbe essere necessario installare driver aggiuntivi.

Il sistema operativo espone poi delle interfacce standard per interagire con i dispositivi in modo **trasparente** per l'applicazione utente.

</div>
</div>

---


<div class='multiCol'>
<div class='col text-center'>

<img src="../images/linux-device-drivers.svg" width=80% />

</div>
<div class='col'>

### File dispositivo

La filosofia *"everything is a file"* si applica anche per le periferiche connesse alla macchina,
infatti esistono dei file speciali nella cartella */dev* che sono la rappresentazione nel file system dei dispositivi.

Essendo dei file, il loro contenuto può essere analizzato come se fossero come tutti gli altri (utilizzando comandi come `cat`, `ls`, ...),
però essi rappresentano delle interfacce di basso livello che le applicazioni utente possono utilizzare per interagire con i dispositivi con operazioni di I/O. 

</div>
</div>

---


<div class='multiCol'>
<div class='col text-center'>

### File dispositivo

All'interno della cartella */dev* risiedono due categorie di file: 
- **Character special files (c)**: I/O basato sullo scambio di singoli caratteri (di dimensione 1 byte). Fanno parte di questa categoria dispotivi come *mouse*, *tastiere* e *schede audio*.
- **Block special files (b)**: I/O basato sullo scambio di *blocchi* di dati. Fanno parte di questa categoria *usb* e *hard disk*.

</div>
<div class='col'>

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

</div>
</div>

---

### Character device

Per comprendere meglio che cos'è un device che comunica tramite un *character special file*, consideriamo il mouse.

Tutti i file all'interno della directory `/dev/input` consentono la comunicazione con le periferiche di input del nostro sistema (ad esempio: tastiera, mouse, webcam, ...).

All'interno della directory `/dev/input/by-path` è possibile esplorare i dispositivi in maniera "user-friendly". 

<br />

**Domanda**: Cosa succede eseguendo il comando `cat /dev/input/mice`?

{{% fragment %}}
Sto visualizzando i byte che il mio mouse invia al sistema operativo ad ogni movimento!
{{% /fragment %}}


---

### /dev/sd[a-z][1-15]

Associati al file con prefisso *sd* (SCSI Disk) storicamente si potevano individuare tutti i dispositivi che utilizzavano SCSI (stampanti, scanner, hard disk, ...), oggi il protocollo è in disuso, ma i sistemi Linux continuano a indirizzare hark disk (SCSI, IDE e SATA) e chiavette USB a questo percorso. 

- `/dev/sda` rappresenta il primo hard disk, `/dev/sbd` il secondo, e così via. 

In caso il dispositivo presenti delle **partizioni**, esse sono rappresentate con un numero posto accanto al nome del dispositivo.

- `/dev/sda1` è la prima partizione, `/dev/sda2` la seconda, e così via.

---

### /dev/nvme[0-N]n[1-N]p[0-15]

Viene associato a tutti i dispositivi che utilizzano *NVM Express* (*NVMe*) come tecnologia per accedere al dispositivo di memoria non-volatile.
Viene comunemente utilizzato per i *dischi a stato solido* (*SSD*).

All'interno della cartella */dev* è possibile distinguere:
- Il character device `/dev/nvme0` che rappresenta il **controller** del primo disco. Esso regola la scrittura sul disco, consentendo l'esistenza dei *namespace*.
- Il block device `/dev/nvme0n1` rappresenta il namespace 1 all'interno del primo disco NVMe. 
- Il block device `/dev/nvme0n1p[1-15]` rappresenta la partizione [1-15] all'interno del namespace 1 del primo disco NMVe.

---



<div class='multiCol'>
<div class='col'>

### Namespace di un disco NVMe

I *namespace* di un disco NMVe sono una suddivisione logica dei dati, gestita direttamente dal *controller* del disco stesso. Ogni namespace consente di essere trattato come un "disco" separato, consentendo un accesso concorrente rispetto gli altri namespace. I namespace vengono impiegati in computer con esigenze particolari, comunemente nei pc desktop si utilizza il namespace di default, ovvero il numero 1. Più dettagli riguardo ai namespace sono riportati sul sito di NVMe: https://nvmexpress.org/resource/nvme-namespaces/.

### Partizioni di un disco

Una partizione del disco definisce delle aree separate di memoria che possono essere gestite separatamente dal sistema operativo, ad ognuna di essere può essere associato un tipo di file system differente. 


</div>
<div class='col text-center'>

<img src="../images/nvme-namespaces.svg" width=80% />

</div>
</div>

---

### Pseudo-dispositivi

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

### Cartella `/dev/disk`

Consente di esplorare tutte le unità di memoria connesse al computer in modalità "user-friendly"

### Cartella `/dev/bus`

Contiene tutti i controller (character devices) che gestiscono i dispositivi USB connessi al pc. 

---

## Ma... per salvare un file su una chiavetta usb?

<div class='multiCol'>
<div class='col text-center'>

{{% fragment %}}


![../images/desperate-programmer.webp](../images/desperate-programmer.webp)


{{% /fragment %}}

</div>
<div class='col'>

{{% fragment %}}

Diversi strumenti aiutano sia l'esplorazione che l'utilizzo di dispositivi di memoria esterni:

- `lsblk`: Informazioni sui dispositivi a blocchi connessi al pc.
- `fdisk`: Utility di sistema per gestire le partizioni dei dischi.
- `mount`: Utility per montare un file system ad un percorso.

{{% /fragment %}}


</div>
</div>

---

### `lsblk`

Consente di consultare i dispositivi a blocchi presenti sul pc (fatta eccezione per la RAM), e informazioni importanti come il **punto di mount**, utilizzato per accedervi.

```console
$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    1  29,3G  0 disk 
nvme0n1     259:0    0 476,9G  0 disk 
├─nvme0n1p1 259:1    0   300M  0 part /boot/efi
├─nvme0n1p2 259:2    0 459,7G  0 part /
└─nvme0n1p3 259:3    0    17G  0 part [SWAP]
```

Come mostra l'output del comando `lsblk` il disco in utilizzo sul pc presenta 3 partizioni, ciascuna delle quali ha un *punto di mount*.

Un *punto di mount* è una directory del file system che è logicamente connessa ad un altro file system, in questo esempio proveniente dal disco primario.

---

### `fdisk`

Utility di sistema per gestire le partizioni di un disco, in questo ambito utile per investigare a quale file di sistema è stato associato il dispositivo USB connesso.

```console
$ fdisk -l
Disk /dev/nvme0n1: 476,94 GiB, 512110190592 bytes, 1000215216 sectors
Disk model: WDC PC SN520 SDAPNUW-512G-1002          
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: BCA6CA00-08B6-6245-83F8-FC173D095FFA

Device             Start        End   Sectors   Size Type
/dev/nvme0n1p1      4096     618495    614400   300M EFI System
/dev/nvme0n1p2    618496  964576913 963958418 459,7G Linux filesystem
/dev/nvme0n1p3 964576914 1000206899  35629986    17G Linux swap


Disk /dev/sda: 29,3 GiB, 31457280000 bytes, 61440000 sectors
Disk model: ProductCode     
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

Altri utilizzi dell'utility `fdisk` non sono coperti da questo corso, ma possono essere approfonditi dal manuale utente: https://man7.org/linux/man-pages/man8/fdisk.8.html

---

### `mount`

Consente di montare un file system esterno su una cartella desiderata all'interno di un percorso del pc

Il file system esterno può provenire da un dispositivo rimovibile o da un percorso di rete, in questa lezione copriamo il primo caso.

Con il comando `fdisk -l` sono a conoscenza di qual è il percorso che mi consente di accedere al mio dispositivo USB (in questo caso `/dev/sda`), 
mentre il comando `lsblk` mostra che il device `/dev/sda` al momento non possiede un mount point per accedervi.

Il processo è semplice:

1. Creo una cartella che voglio utilizzare per accedere al file system del mio dispositivo usb

```console
$ mkdir $HOME/usb
```

2. Monto il dispositivo all'interno della cartella che ho appena creato

```console
$ mount /dev/sda $HOME/usb
```

3. Posso accedere ai dati in lettura e in scrittura

```console
$ ls -alh $HOME/usb
...
```

4. Per "scollegare" il dispositivo dal file system basta utilizzare il comando `umount`

```console
$ umount /dev/sda
$ ls -alh $HOME/usb
```

---

## 2. Strumenti di analisi e gestione dei processi

---

### Processi

Ogni programma eseguito in un sistema linux ha il suo *processo*.

I processi hanno una struttura gerarchica, la radice comune a tutti è il processo *init* responsabile dell'esecuzione di tutti i software all'interno del sistema.

Ogni processo:
- Ha un suo identificativo univoco *PID*.
- È indipendente dagli altri, consentendo di eseguire simultaneamente.
- Ha il suo spazio di indirizzi in memoria (RAM), consentendo al processo di eseguire, scrivere e leggere liberamente in quella porzione di memoria senza sovrascrivere i dati di altri processi.

---

### Stati di un processo

Lo stato di un processo può essere:
- *In esecuzione* (*Running*): il processo è attualmente in esecuzione e sta svolgendo il suo compito.
- *In attesa* (*Sleeping*): il processo è in attesa di un evento per continuare la sua esecuzione, ad esempio l'attesa di un input da parte dell'utente. 
- *Sospeso* (*Stopped*): il processo ha ricevuto un segnale di terminazione, attraverso l'utilizzo dei comandi `kill` o `killall` e non sta eseguendo nessuna operazione, ma **la sua esecuzione può essere riesumata**.
- *Zombie*: Il processo ha terminato la sua esecuzione e rilasciato tutte le risorse di sistema, ma non è stato rimosso dall'elenco dei processi attivi. Questo accade perchè l'*exit-code* non è stato letto dal processo padre, operazione necessaria per rimuoverlo dall'elenco. 

<br/>
<br/>

L'*exit-code* è un valore numerico (0-255) restituito al processo padre nel momento in cui un processo termina. 
La convenzione per gli exit-code è:
- `0` significa che il processo è terminato correttamente
- `1-255` il processo è terminato con un errore. Idealmente ogni valore identifica un errore differente, ma il significato che ad essi è associato può variare.

---

### Tipi di processo

Un processo in esecuzione può essere di tre tipi: 
- *Foreground*: Richiedono che un utente li faccia partire e/o che interagisca con loro durante l'esecuzione. Di default tutti i comandi eseguono come processi in foreground. Un esempio di interazione con l'utente è la visualizzazione degli output di un comando lanciato da terminale: finchè esso non termina, infatti, l'utente non può eseguire un altro comando nello stesso terminale perchè "occupato" dal processo in esecuzione.
- *Background*: Eseguono senza necessità di interagire con l'utente.
- *Daemon*: Sono processi costantemente in esecuzione all'interno del sistema, alcuni in esecuzione e alcuni sospesi (stopped). Essi consentono di fornire servizi di sistema. Verranno approfonditi nel modulo corso di linux avanzato.

---

È possibile eseguire un processo in background post-ponendo `&` al comando su terminale. Ad esempio:

```console
$ sh -c 'sleep 5 && echo Terminato!' &
[1] 131593
$ jobs
[1]  + 131593 running    sh -c 'sleep 5 && echo Terminato!'
$ Terminato!
[1]  + 131593 done       sh -c 'sleep 5 && echo Terminato!'
```

Questo comando esegue uno script shell che attende 5 secondi poi scrive sullo standard output "Terminato!".

Se non lo eseguissi in background (rimuovendo `&`) dovrei attendere 5 secondi prima di poter interagire nuovamente con il terminale da cui l'ho lanciato, perchè di default è un processo in foreground. 

Per gestire i processi in background **figli del processo corrente** posso eseguire:
- `jobs` consente di visualizzare l'elenco dei sotto-processi in background
- `fg` comando che consente di spostare l'esecuzione in foreground di un processo in background. Se non viene specificato l'id, viene prelevato l'ultimo che è stato posto in background.    

---

### Visualizzazione dei processi in esecuzione (`top` / `htop`)

`top` Consente di avere una visione real-time dei processi in esecuzione, insieme ad una preziosa panoramica delle risorse utilizzate rispetto alle totali.

```console
$ top
top - 15:33:59 up  4:42,  2 users,  load average: 2,70, 6,53, 4,70
Tasks: 339 total,   1 running, 338 sleeping,   0 stopped,   0 zombie
%Cpu(s):  3,4 us,  1,5 sy,  0,0 ni, 94,4 id,  0,1 wa,  0,5 hi,  0,2 si,  0,0 st 
MiB Mem :  15808,3 total,    847,4 free,  10927,9 used,   5369,8 buff/cache     
MiB Swap:  17397,4 total,  17341,2 free,     56,2 used.   4880,4 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
 137878 anitvam   20   0 1239992 231768 211956 S   6,6   1,4   0:00.83 konsole
    847 root      20   0 1182360 207444 141000 S   5,0   1,3  12:04.65 Xorg
   1728 anitvam   20   0 2534772 279132 130920 S   4,7   1,7  10:47.28 kwin_x11
   2716 anitvam   20   0   33,2g 658404 324392 S   4,3   4,1  19:14.92 chrome
   2763 anitvam   20   0   32,4g 165380 110848 S   3,0   1,0   4:21.39 chrome
  77629 anitvam   20   0 1156,0g 401540 139904 S   2,0   2,5   4:31.55 chrome
    679 root      20   0   22992  16612   6500 S   1,7   0,1   1:08.24 python3
   1769 anitvam   20   0 3644448 417728 166268 S   1,3   2,6   5:16.92 plasmashell
   4771 101       20   0 1555036 155724  46304 S   1,0   1,0   1:37.89 mongod
    435 root     -51   0       0      0      0 S   0,7   0,0   0:16.40 irq/109-ELAN1401:00
   1878 anitvam   20   0  246104  35724  30604 S   0,7   0,2   1:15.28 ksystemstats
  78013 anitvam   20   0 1155,9g 173896 110072 S   0,7   1,1   0:27.40 chrome
 138113 anitvam   20   0 1131,9g 369660  93528 S   0,7   2,3   0:22.63 code
 140042 anitvam   20   0   11072   8060   5884 R   0,7   0,0   0:00.07 top
    171 root       0 -20       0      0      0 I   0,3   0,0   0:01.47 kworker/2:1H-events_highpri
    678 root      20   0  413844  25020  20668 S   0,3   0,2   0:16.80 NetworkManager
   1725 anitvam   20   0  772716  64736  53452 S   0,3   0,4   0:04.25 ksmserver
   1963 anitvam   20   0 5887584 285932  41824 S   0,3   1,8   1:16.35 jetbrains-toolb
  77659 anitvam   20   0 1219,6g 319376 111132 S   0,3   2,0   4:21.60 chrome
...
```

---

<div class='multiCol'>
<div class='col'>

`top` è un comando interattivo, che aggiorna le informazioni ogni intervallo di tempo prestabilito.

Una volta individuato un processo critico che sta utilizzando un numero inaspettatamente superiore delle risorse previste, è possibile mandargli il segnale di terminazione direttamente mentre `top` è in esecuzione, premendo il tasto `k` sulla tastiera.

Una volta premuto k posso indicare a direttamente l'id e il segnale da mandare al processo.

</div>
<div class='col text-center'>

Esiste anche una versione più semplice da navigare con anche una presentazione a colori, chiamata `htop`. 

<img src="../images/htop.png" />

</div>
</div>

---

### Load average

La prima riga di output del comando `top` riporta delle informazioni importanti per un amministratore di sistema,
infatti, esse mostrano da quanto tempo il computer è in esecuzione, quanti utenti sono attivi in un determinato momento e qual è il *load average* del sistema.

```console
$ top
top - 15:33:59 up  4:42,  2 users,  load average: 2,70, 6,53, 4,70
```

Il *load average* non è altro che la media del numero di processi in attesa di essere eseguiti dal processore.

La metrica mostra tre valori in quanto rappresentano la media per gli ultimi 1, 5 e 15 minuti d'esecuzione.

<br />

**Si può ottenere la stessa informazione eseguendo il comando `uptime`**

```console 
$ uptime
 16:06:02 up  5:14,  2 users,  load average: 1,02, 1,19, 1,77
```
---

### Amministrazione dei processi

I processi in esecuzione, scrivono importanti informazioni sull'andamento della loro esecuzione in file di `log`.

I file di log sono fondamentali per gestire un processo fallito in modo inaspettato e identificare una soluzione.

I file di log sono memorizzati nella cartella `/var/log` e possono essere di due formati: *log di sistema* (in binario) e *log di testo normale*. 

Gran parte delle applicazioni memorizzano utilizzando il servizio di log di sistema, per interagire con essi è necessario passare dall'utility di `journalctl`. 

---

### `journalctl`

<br />

#### `journalctl -b`
Filtra i log a partire dal riavvio più recente. Altrimenti l'output parte dai log più vecchi memorizzati.
È possibile specificare di quanti riavvii il journal deve filtrare gli output con degli indici negativi; 
se `0` indica il riavvio corrente, `-1` indica quello precedente e così via.

#### `journalctl -p err`
Filtra i log per livello di gravità "err".

#### `journalctl -k`
Filtra i log relativi al kernel.

#### `journalctl --since yesterday`
Filtra i log a partire dal giorno precedente.

#### `journalctl --since "2024-01-09" --until "2024-09-09"`
Filtra i log appartenenti ad un intervallo di tempo prestabilito, specificato con lo standard ISO 8601 (*YYYY-MM-DD*).  

#### `journalctl -f`
Mostra i log correnti e mostra i log in tempo reale nel terminale.

---




<div class='multiCol'>
<div class='col'>

#### Ma questi dati non saturano il disco a lungo termine?

{{% fragment %}}

**Potenzialmente sì**.

Per questo motivo, a intervalli di tempo stabiliti, i file di log vengono *ruotati*, ovvero:
1. Rinominati 
2. Sostituiti da nuovi file di log
3. Compressi con gzip

Grazie alla rotazione dei log è quindi possibile ottimizzare lo spazio occupato da essi, seppur mantenendo la possibilità di analizzarli in futuro. 
`logrotate` è il comando che si occupa della rotazione dei file di log, configurato con il file di sistema `/etc/logrotate.conf`.

{{% /fragment %}}

</div>
<div class='col'>

{{% fragment %}}

```console
$ cat /etc/logrotate.conf
# see "man logrotate" for details
# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# restrict maximum size of log files
#size 20M

# create new (empty) log files after rotating old ones
create

# uncomment this if you want your log files compressed
#compress

# Logs are moved into directory for rotation
# olddir /var/log/archive

# Ignore pacman saved files
tabooext + .pacorig .pacnew .pacsave

# Arch packages drop log rotation information into this directory
include /etc/logrotate.d

/var/log/wtmp {
    monthly
    create 0664 root utmp
    minsize 1M
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}
```

{{% /fragment %}}

</div>
</div>




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

