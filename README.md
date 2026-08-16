## My Linux Mint LMDE 7 customization  ![alt](http://gitea.local/miko/appunti-di-linuxmint/raw/branch/main/immagini/mintlogo.png)

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

16. Generare la chiave SSH

`ssh-keygen -t ed25519`

`ssh-copy-id 192.168.1.xxx`

`ssh 192.168.1.xxx`  per test  

- in caso di formattazione del dispositivo al quale si era inviata la key riceveremo un messaggio di errore relativo alla key qualora tentassimo di riconnetterci, dobbiamo quindi cancellare la vecchia key e rimandarla con i seguenti comandi

`ssh-keygen -R <IP_DEL_SERVER>`

`ssh-copy-id <utente>@<IP_DEL_SERVER>`

17. *Montaggio automatico disco secondario e disco di rete*

- creiamo il punto di mount per i due dischi  

`sudo mkdir -p /mnt/crucial`  
`sudo mkdir -p /mnt/nasm2`  
`sudo mkdir -p /mnt/seagate`  

- per montare all'avvio il disco secondario per prima cosa identificare il suo *UUID* con il comando  

`sudo blkid`  

- modifica del file /etc/fstab  

`sudo nano /etc/fstab`  

- aggiungiamo questa riga alla fine del file  

```
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  /mnt/crucial  ntfs  defaults,noatime,nofail  0  2`  
//192.168.1.192/nasm2 /mnt/NASm2 cifs username=username,password=password,rw,uid=1000,gid=1000,x-gvfs-show 0 0
//192.168.1.192/seagate /mnt/seagate cifs username=username,password=password,rw,uid=1000,gid=1000,vers=3.0,_netdev,nofail,x-gvfs-show 0 0
```

- verifiche, prima senza riavviare  

`sudo mount -a` (se non si sono errori il file funziona)  

- riavviamo  

`sudo systemctl reboot`  

- al riavvio `lsblk` per conferma  

- Apriamo l'utility *Dischi*, clicchiamo sul disco e poi sull'icona degli ingranaggi per entrare nelle opzioni, selezioniamo *Modifica opzioni di montaggio* spuntiamo *Mostrare nell'interfaccia grafica* per vedere il disco nel tree del file manager  


#### Software

18. `sudo apt install htop`
19. `sudo apt install preload`  (caricamento in memoria dei programmi più usati)
20. `sudo apt install unrar`
21. `sudo apt install git `
22. `sudo apt install imagemagick`

23. `sudo apt install mpv`

> :memo: *script* da aggiungere nella cartella */home/.config/mpv/scripts*: **[autoload.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua)** - **[blacklist-extensions.lua](https://github.com/occivink/mpv-scripts/blob/master/scripts/blacklist-extensions.lua)** file da aggiungere nella cartella */home/.config/mpv/script-opts*:  

# autoload.conf  
```
directory_mode=ignore
```

# blacklist_extension.conf
```
# only one of blacklist, whitelist should be defined at a time

# only allow video and image formats
whitelist=mkv,webm,mp4,avi

# alternatively, blacklist formats commonly found near videos
#blacklist=srt,ass,mks,mka,png,jpg,jpeg,gif

remove_files_without_extension=yes

# if the script should be applied only at the beginning, or anytime the playlist changes
oneshot=yes
```

- Per poter scorrere tra i file di una cartella con i tasti *PG ↑ & PG ↓* creare il file *input.conf* nella cartella */home/.config/mpv* con le seguenti righe:

```
PGUP playlist-prev ; show-text "${playlist-pos-1}/${playlist-count}"
PGDWN playlist-next ; show-text "${playlist-pos-1}/${playlist-count}"
```

24. `sudo apt install flameshot`  

- Apri il Menu delle applicazioni di Linux Mint.  
- Cerca e apri le Impostazioni di sistema (o digita direttamente Tastiera).  
- Spostati sulla scheda Scorciatoie (o Shortcuts) in alto.  
- Nel pannello di sinistra, clicca sulla voce Scorciatoie personalizzate.  
- Fai clic sul pulsante Aggiungi scorciatoia personalizzata.  
- Compila i campi della finestra pop-up con queste informazioni:    
```
Nome: Flameshot  
Comando: flameshot gui (Se hai installato la versione Flatpak, usa il comando: flatpak run org.flameshot.Flameshot gui)  
Clicca su Aggiungi.  
Nella sezione inferiore "Associazioni di tasti", fai doppio clic sulla prima riga vuota con la scritta non assegnato. La dicitura cambierà in "Premi un tasto...". Premi il tasto Stamp (o PrtSc) sulla tastiera.    
Il sistema mostrerà un avviso informandoti che il tasto è già in uso. Conferma cliccando su Sì (o Riassegna) per trasferire il tasto a Flameshot.  
```

25. Installare *font microsoft*.

* `sudo apt install ttf-mscorefonts-installer`

26. **[MediaInfo](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)**[ tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)
27. Installare **[cpu-x](https://community.linuxmint.com/software/view/cpu-x)**

28.  
    * **[Chrome](https://support.google.com/chrome/a/answer/9025926?hl=it)**
    * Avidemux
    * Arduino IDE
    * Audacity
    * Freecad
    * **[Acestreamplayer](https://snapcraft.io/install/acestreamplayer/debian)**
    * Gimp
    * Telegram
    * GParted
    * FileZilla
    * **[Lite-xl](https://lite-xl.com/setup/getting-started/)**
    * GearLever

29. **Fish Shell**

`echo 'deb http://download.opensuse.org/repositories/shells:/fish/Debian_13/ /' | sudo tee /etc/apt/sources.list.d/shells:fish.list`  

`curl -fsSL https://download.opensuse.org/repositories/shells:fish/Debian_13/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/shells_fish.gpg > /dev/null`  

`sudo apt update`  

`sudo apt install fish`  

- A terminale aperto, cliccare con il tasto destro in un punto e cliccare su *Preferenze*. Andare nella sezione del profilo, tab *Comando*, spuntare l'opzione *Eseguire un comando personalizzato invece della shell* e nella sezione *Comando personalizzato* inserire */usr/bin/fish*  

- Per togliere le due righe del benvenuto del nuovo terminale lanciamo il seguente comando  
`set -U fish_greeting ""`  

- Installare uno dei *[Nerd Font](https://github.com/ryanoasis/nerd-fonts?tab=readme-ov-file)*  
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

30. Disinstallare **Firefox**, **Thunderbird**, **Matrix**, **Celluloid**, **Xreader**, **Libreria**, **Impronta digitale**

31. Configuriamo òa scheda di rete per raggiungere i domini .local presenti sul server locale:

##### 0. - Troviamo il nome della connessione di rete
`nmcli connection show --active` 

##### 1. Imposta il DNS sul tuo server ed esclude quelli automatici del router
`sudo nmcli connection modify "Wired connection 1" ipv4.dns "192.168.1.192" ipv4.ignore-auto-dns yes`

##### 2. Disattiva l'IPv6 su questa connessione per evitare che scavalchi il Pi-hole
`sudo nmcli connection modify "Wired connection 1" ipv6.method "ignore"`

##### 3. Dice al sistema di inviare le richieste .local al tuo DNS
`sudo nmcli connection modify "Wired connection 1" ipv4.dns-search "~local"`

##### 4. Riattiva la scheda e svuota la cache
`sudo nmcli connection down "Wired connection 1" && sudo nmcli connection up "Wired connection 1" && sudo resolvectl flush-caches`

##### 5. Verifica finale
`resolvectl query portainer.local`

  * Per evitare conflitti tra le *WebUi* o se ci sono problemi a far digerire pihole come DNSResolve, andiamo a modificare il file `/etc/hosts` nel seguente modo: 

`sudo nano /etc/hosts`  

aggiungiamo al file le seguenti linee  

```
192.168.1.xxx   pihole.local  
192.168.1.xxx   webmin.local  
192.168.1.xxx   portainer.local  
192.168.1.xxx   bentopdf.local 
```  

###### Extra

32. Sul Thinkpad installare **tlp** per l'ottimizzazione batteria

`sudo apt install tlp tlp-rdw`  

33. Modificare la dimensione del *Terminale* a **110** colonne

34. nvim (appimage)  

- `curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage`  
- `chmod u+x nvim-linux-x86_64.appimage`  
- `sudo mv nvim-linux-x86_64.appimage /usr/local/bin/nvim`  
- installiamo alcune dipendenze:  
  `sudo apt update`  
  `sudo apt install build-essential git curl -y`  
  `sudo apt install nodejs npm -y`  
  `sudo apt install tree-sitter-cli -y`  
- copiare il contenuto del file *init.lua* nel percorso */home/user/.config/nvim/*  
- riavviare *nvim* e accertarsi che si sia intallato tutto, qualora **Lazy** desse problemi utilizziamo il seguente comando *curl*:  
  `git clone --filter=blob:none "https://github.com/folke/lazy.nvim.git" --branch=stable ~/.local/share/nvim/lazy/lazy.nvim`  
- riavviamo *nvim* che qeusta volta dovrebbe avviarsi senza problemi  
- per finire diamo questi due comandi:  
  `cd ~/.local/share/nvim/lazy/markdown-preview.nvim/app`  
  `npm install`  