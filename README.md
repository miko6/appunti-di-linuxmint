## My Linux Mint LMDE 7 customization  ![alt](https://github.com/miko6/appunti-di-linuxmint/blob/main/immagini/mintlogo.png)

#### Impostazioni del sistema


1. *Avvio automatico* nelle impostazioni di installazione.
2. Attivare **Timeshift**.
3. *Impostazioni di sistema/Salvaschermo* - disattivare le due voci in *Impostazioni di blocco*.
4. Disattivare *bluetooth* in Applicazioni d'avvio.
5. Rimuovere il *separatore* accanto al menù.
6. Azzerare tempo *bootloader*.
7. Abilitare lo *scalamento frazionario* nelle impostazioni del monitor.

* `sudo nano /etc/default/grub`
* settare *GRUB_TIMEOUT* a *0*
* dare un `sudo update-grub`


8. Attivare il **Firewall**
9. Settare un *IP statico* e cambiare *DNS*.

* Clic sull'icona di rete/Impostazioni di rete/Selezioniamo l'interfaccia di rete e clic sull'icona ingranaggio delle impostazioni, nella sezione IPV4 impostiamo i valori di Getaway e Subnet mask.
* Impostiamo il *DNS* del server con PiHole oppure usiamo i classici:  

| DNS | Primario | Secondario |
|----|----|----|
| Cloudflare | 1.1.1.1 | 1.0.0.1 |
| Google | 8.8.8.8 | 8.8.4.4 |


10. *Data e ora* stile windows10.

* Tasto destro sull'orologio, Configura, Utilizza un formato data personalizzato e scrivi nel campo Formato della data %R%n%x


11. Dal *Gestore software* attivare i Flatpack non verificati (per installare ad esempio **Avidemux**).
12. Tasto destro sul desktop Personalizza per aggiungere le icone *Home*, *Cestino*, *Rete* ecc.
13. *Pannello trasparente*

* Aprire il menu Estensioni, aggiungere l'applet Pannelli trasparenti, attivarli.


14. In *Effetti* disabilitare **Effetti del desktop e della finestra**
15. *Migliorare font su temi scuri*

* `sudo nano /etc/environment`
* aggiungere la seguente stringa alla fine del file:

```
FREETYPE_PROPERTIES="cff:no-stem-darkening=0 autofitter:no-stem-darkening=0"
```


16. *Font rendering fix*

* Create a folder in home call it *.fontconfig* inside this folder create a blank text call it *fonts.conf* and paste this then log OFF/ON:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <match target="font">
        <edit name="antialias" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hinting" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="hintstyle" mode="assign">
            <const>hintslight</const>
        </edit>
        <edit name="rgba" mode="assign">
            <const>rgb</const>
        </edit>
        <edit name="autohint" mode="assign">
            <bool>true</bool>
        </edit>
        <edit name="lcdfilter" mode="assign">
            <const>lcddefault</const>
        </edit>
        <edit name="dpi" mode="assign">
            <double>102</double>
        </edit>
    </match>
    <match target="font">
        <test name="weight" compare="more">
            <const>medium</const>
        </test>
        <edit name="autohint" mode="assign">
            <bool>true</bool>
        </edit>
    </match>
</fontconfig>
```


17. *Abilitare condivisione cartella*

* `sudo apt install samba` (se non installato)
* Controllare che l'utente in Utenti e Gruppi sia inserito nel gruppo sambashare
* `sudo smbpasswd -a nomeutente`
* digitare prima la password amministratore e poi due volte la password per la condivisione
* riavviare il server samba con `sudo service smbd restart` o riavviare il sistema
* Aggiungere una nuova regola sul firewall, le voci devono essere:  

`| Politica | Consenti |`  
`| Direzione | Ingresso |`  
`| Categoria | Network |`  
`| Sotto-categoria | Tutte |`  
`| Applicazione | Samba |`  

* Creare una cartella in una posizione a piacere (ad esempio il desktop), clic con il tasto destro e scegliere *Opzioni condivisione*, abilitare la voce *Condividi questa cartella* e mettere la spunta a *Permetti ad altri di creare...*  

* aggiungiamo le seguenti voci alla fine del file:

```
[nomeascelta]  
path = /home/nomeutente/nomecartella  
read only = no
force user = nomeutente  
guest ok = no
```  

18. *Montaggio automatico disco secondario*

- identificare il disco ed il suo *UUID* con il comando  

`sudo blkid`  

- creiamo il punto di mount  

`sudo mkdir -p /mnt/xxxx`  

- modifica del file /etc/fstab  

`sudo nano /etc/fstab`  

- aggiungiamo questa riga alla fine del file  

`UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  /mnt/xxxx  ext4  defaults,noatime,nofail  0  2`  

- verifiche, prima senza riavviare  

`sudo mount -a` (se non si sono errori il file funziona)  

- riavviamo  

`sudo systemctl reboot`  

- al riavvio `lsblk` per conferma  

19. *Montare disco di rete all'avvio*  

* Per verificare che l'utility sia installata lanciare un:  

`sudo apt update && sudo apt install cifs-utils`  

* Crea un File per le Credenziali  

`sudo mkdir -p /etc/samba`  
`sudo nano /etc/samba/credenziali-server`  

* Inserisci le tue credenziali nel file seguendo questa struttura senza spazi intorno al segno =:  

```
username=nome_utente_samba  
password=tua_password  
domain=DOMINIO_OPPURE_WORKGROUP  
```

* Salva il file e proteggilo rendendolo scrivibile solo da root:  

`sudo chmod 644 /etc/samba/credenziali-server`  

* Crea il Punto di Montaggio Locale  

`sudo mkdir /media/NASm2`  

* Modifica il File */etc/fstab*  

`sudo nano /etc/fstab` 

* Aggiungi la seguente riga alla fine del file, sostituendo i valori segnaposto con i tuoi:  

`//192.168.1.192/NASm2 /media/NASm2 cifs credentials=/etc/samba/credenziali-server,uid=1000,gid=1000,iocharset=utf8,_netdev,x-systemd.automount,vers=3.0 0 0`  

* Riavvia  

> :memo: **Note:** se riceviamo un *permisison denied* relativo a *cifs* dobbiamo aggiungere l'utente *samba* con `sudo smbpasswd -a nomeutente`  


#### Software


20. `sudo apt install htop`
21. `sudo apt install preload`  (caricamento in memoria dei programmi più usati)
22. `sudo apt install unrar`
23. `sudo apt install git `
24. `sudo apt install jq`

> :memo: **Note:** jq in Linux è un processore di JSON da riga di comando, flessibile e leggero, utilizzato per manipolare e trasformare dati in formato JSON


25. `sudo apt install mpv`

> \
> :memo: *script* da aggiungere: **[autoload.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua)** - **[autocrop.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autocrop.lua)**Per poter scorrere tra i file di una cartella con i tasti *PG ↑ & PG ↓* creare il file *input.conf* nella cartella */home/.config/mpv* con le seguenti righe:

```
PGUP playlist-prev ; show-text "${playlist-pos-1}/${playlist-count}"
PGDWN playlist-next ; show-text "${playlist-pos-1}/${playlist-count}"
```


26. Icone **NUMIX**

* `sudo apt install numix-icon-theme-circle `


27. [Icone ](https://github.com/bikass/kora)**[KORA](https://github.com/bikass/kora)**
28. Installare *font microsoft*.

* `sudo apt install ttf-mscorefonts-installer`


29. **[MediaInfo](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)**[ tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)
30. Installare **[cpu-x](https://community.linuxmint.com/software/view/cpu-x)**
31. \
    * **[Chrome](https://support.google.com/chrome/a/answer/9025926?hl=it)**
    * Avidemux
    * Arduino IDE
    * **[Edge](https://www.microsoft.com/it-it/edge/download?form=MA13FJ)**
    * **[Visual Studio Code](https://code.visualstudio.com/docs/setup/linux)**
    * Audacity
    * Freecad
    * Kodi
    * **[Acestreamplayer](https://snapcraft.io/install/acestreamplayer/debian)**
    * Gimp
    * Telegram
    * PDF Arranger
    * GParted
    * FFmpeg

32. Disinstallare **Firefox**, **Thunderbird**, **Matrix**, **Celluloid**, **Xreader**

33. Per evitare conflitti tra le *WebUi* dei servizi installati nel server andiamo a modificare il file `/etc/hosts` nel seguente modo: 

`sudo nano /etc/hosts`  

aggiungiamo al file le seguenti linee  

```
192.168.1.xxx   pi.hole  
192.168.1.xxx   webmin.local  
192.168.1.xxx   portainer.local  
```

#### Extra


34. Sul Thinkpad installare **tlp** per l'ottimizzazione batteria

`sudo apt install tlp tlp-rdw`


35. Su Linux Mint abilitare *snap* per installare **acestreamplayer**

`sudo rm /etc/apt/preferences.d/nosnap.prefsudo apt install snapdsudo snap install acestreamplayer`
