## My Linux Mint LMDE 7 customization  ![alt](https://github.com/miko6/appunti-di-linuxmint/blob/main/immagini/mintlogo.png)

#### Impostazioni del sistema


1. *Avvio automatico* nelle impostazioni di installazione.
2. *Impostazioni di sistema/Salvaschermo* - disattivare le due voci in *Impostazioni di blocco*.
3. Disattivare *bluetooth* in Applicazioni d'avvio.
4. Rimuovere il *separatore* accanto al menù, cambiare l'icona del menù e spostare le app al centor del pannello basso.
5. Azzerare tempo *bootloader*.

* `sudo nano /etc/default/grub`
* settare *GRUB_TIMEOUT* a *0*
* dare un `sudo update-grub`

6. Abilitare lo *scalamento frazionario* nelle impostazioni del monitor.
7. Attivare il **Firewall**
8. Settare un *IP statico* e cambiare *DNS*.

* Clic sull'icona di rete/Impostazioni di rete/Selezioniamo l'interfaccia di rete e clic sull'icona ingranaggio delle impostazioni, nella sezione IPV4 impostiamo i valori di Getaway e Subnet mask.
* Impostiamo il *DNS* del server con PiHole oppure usiamo i classici:  

| DNS | Primario | Secondario |
|----|----|----|
| Cloudflare | 1.1.1.1 | 1.0.0.1 |
| Google | 8.8.8.8 | 8.8.4.4 |


9. *Data e ora* stile windows10.

* Tasto destro sull'orologio, Configura, Utilizza un formato data personalizzato e scrivi nel campo Formato della data %R%n%x

10. Dal *Gestore software* attivare i Flatpack non verificati (per installare ad esempio **Avidemux**).
11. Tasto destro sul desktop Personalizza per aggiungere le icone *Home*, *Cestino*, *Rete* ecc.
12. *Pannello trasparente*

* Aprire il menu Estensioni, aggiungere l'applet Pannelli trasparenti, attivarli.

13. In *Effetti* disabilitare **Effetti del desktop e della finestra**
14. *Migliorare font su temi scuri*

* `sudo nano /etc/environment`
* aggiungere la seguente stringa alla fine del file:

```
FREETYPE_PROPERTIES="cff:no-stem-darkening=0 autofitter:no-stem-darkening=0"
```
15. *Font rendering fix*

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

16. *Montaggio automatico disco secondario e disco di rete*

- creiamo il punto di mount per i due dischi  

`sudo mkdir -p /mnt/crucial`
`sudo mkdir -p /mnt/NASm2`  

- per montare all'avvio il disco secondario per prima cosa identificare il suo *UUID* con il comando  

`sudo blkid`  

- modifica del file /etc/fstab  

`sudo nano /etc/fstab`  

- aggiungiamo questa riga alla fine del file  

```
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  /mnt/crucial  ntfs  defaults,noatime,nofail  0  2`  
//192.168.1.XXX/NASm2 /media/NASm2 cifs username=username,password=password,rw,uid=1000,gid=1000 0 0
```

- verifiche, prima senza riavviare  

`sudo mount -a` (se non si sono errori il file funziona)  

- riavviamo  

`sudo systemctl reboot`  

- al riavvio `lsblk` per conferma  

- Apriamo l'utility *Dischi*, clicchiamo sul disco e poi sull'icona degli ingranaggi per entrare nelle opzioni, selezioniamo *Modifica opzioni di montaggio* spuntiamo *Mostrare nell'interfaccia grafica* per vedere il disco nel tree del file manager


#### Software

17. `sudo apt install htop`
18. `sudo apt install preload`  (caricamento in memoria dei programmi più usati)
19. `sudo apt install unrar`
20. `sudo apt install git `

21. `sudo apt install mpv`

> :memo: *script* da aggiungere nella cartella */home/.config/mpv/scripts*: **[autoload.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua)** - Per poter scorrere tra i file di una cartella con i tasti *PG ↑ & PG ↓* creare il file *input.conf* nella cartella */home/.config/mpv* con le seguenti righe:

```
PGUP playlist-prev ; show-text "${playlist-pos-1}/${playlist-count}"
PGDWN playlist-next ; show-text "${playlist-pos-1}/${playlist-count}"
```

22. Installare *font microsoft*.

* `sudo apt install ttf-mscorefonts-installer`

23. **[MediaInfo](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)**[ tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)
24. Installare **[cpu-x](https://community.linuxmint.com/software/view/cpu-x)**
25.  
    * **[Chrome](https://support.google.com/chrome/a/answer/9025926?hl=it)**
    * Avidemux
    * Arduino IDE
    * **[VSCode](https://code.visualstudio.com/docs/setup/linux)**
    * Audacity
    * Freecad
    * **[Acestreamplayer](https://snapcraft.io/install/acestreamplayer/debian)**
    * Gimp
    * Telegram
    * PDF Arranger
    * GParted
    * FileZilla

26. **Fish Shell**

`echo 'deb http://download.opensuse.org/repositories/shells:/fish/Debian_13/ /' | sudo tee /etc/apt/sources.list.d/shells:fish.list`  

`curl -fsSL https://download.opensuse.org/repositories/shells:fish/Debian_13/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/shells_fish.gpg > /dev/null`  

`sudo apt update`  

`sudo apt install fish`  

- A terminale aperto, cliccare con il tasto destro in un punto e cliccare su *Preferenze*. Andare nella sezione del profilo, tab *Comando*, spuntare l'opzione *Eseguire un comando personalizzato invece della shell* e nella sezione *Comando personalizzato* inserire */usr/bin/fish*  

- Per togliere le due righe del benvenuto del nuovo terminale lanciamo il seguente comando  
`set -U fish_greeting ""`  

- Installare uno dei *[Nerd Font](https://github.com/ryanoasis/nerd-fonts?tab=readme-ov-file)*  - 
- Il mio preferito è *Meslo*:
```
git clone --depth 1 https://github.com/ryanoasis/nerd-fonts.git  
cd nerd-fonts  
./install.sh Meslo  
```
`fc-cache -fv`  

andiamo nelle Preferenze del terminale e nella sezione del profilo settiamo il nerd font installato come predefinito e *10* come dimensione  

- Installare *[fisher](https://github.com/jorgebucaran/fisher)* + plugin *tide*  
seguiamo i passaggi per configurarlo, le mie scelte:  
**3 (Rainbow) - 1 (True color) - 2 (24-hour format) - 2 (Vertical) - 1 (Sharp) - 1 (Flat) - 4 (Two lines, character and frame) - 3 (Solid) - 2 (Yes) - 4 (Darkest) - 2 (Sparse) - 2 (Many icons) - 2 (Yes) - y (Per sovrascrivere i cambiamenti)**  

- Per terminare possiamo aggiungere qualche configurazione al file `/home/nomeutente/.config/fish/config.fish`  
es. *alias clera clear*  

- Digitare `fish_config` per ulteriori configurazioni  

Riavviare  

27. Disinstallare **Firefox**, **Thunderbird**, **Matrix**, **Celluloid**, **Xreader**, **Libreria**, **Impronta digitale**

28. Per evitare conflitti tra le *WebUi* dei servizi installati nel server andiamo a modificare il file `/etc/hosts` nel seguente modo: 

`sudo nano /etc/hosts`  

aggiungiamo al file le seguenti linee  

```
192.168.1.xxx   pi.hole  
192.168.1.xxx   webmin.local  
192.168.1.xxx   portainer.local  
```

#### Extra

29. Sul Thinkpad installare **tlp** per l'ottimizzazione batteria

`sudo apt install tlp tlp-rdw`  
