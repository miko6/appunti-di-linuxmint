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
* Impostiamo anche i nostri *DNS*:

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


1. *Font rendering fix*

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


1. *Abilitare condivisione cartella*

* `sudo apt install samba` (se non installato)
* Controllare che l'utente in Utenti e Gruppi sia inserito nel gruppo sambashare
* `sudo smbpasswd -a nomeutente`
* digitare prima la password amministratore e poi due volte la password per la condivisione
* riavviare il server samba con `sudo service smbd restart` o riavviare il sistema
* \
  \
  \
  \
  \
  Aggiungere una nuova regola sul firewall, le voci devono essere:`| Politica | Consenti || Direzione | Ingresso || Categoria | Network || Sotto-categoria | Tutte || Applicazione | Samba |`
* Creare una cartella in una posizione a piacere (ad esempio il desktop), clic con il tasto destro e scegliere *Opzioni condivisione*, abilitare la voce *Condividi questa cartella* e mettere la spunta a *Permetti ad altri di creare...*
* \
  Modifichiamo ora il file smb.conf`sudo nano /etc/samba/smb.conf`


* aggiungiamo le seguenti voci alla fine del file:

```
[nomeascelta]  
path = /home/nomeutente/nomecartella  
read only = no
force user = nomeutente  
guest ok = no
```

#### Software


18. `sudo apt install htop`
19. `sudo apt install preload`  (caricamento in memoria dei programmi più usati)
20. `sudo apt install unrar`
21. `sudo apt install git `
22. `sudo apt install jq`

> :memo: **Note:** jq in Linux è un processore di JSON da riga di comando, flessibile e leggero, utilizzato per manipolare e trasformare dati in formato JSON


23. `sudo apt install mpv`

> \
> :memo: *script* da aggiungere: **[autoload.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua)** - **[autocrop.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autocrop.lua)**Per poter scorrere tra i file di una cartella con i tasti *PG ↑ & PG ↓* creare il file *input.conf* nella cartella */home/.config/mpv* con le seguenti righe:

```
PGUP playlist-prev ; show-text "${playlist-pos-1}/${playlist-count}"
PGDWN playlist-next ; show-text "${playlist-pos-1}/${playlist-count}"
```


24. Icone **NUMIX**

* `sudo apt install numix-icon-theme-circle `


25. [Icone ](https://github.com/bikass/kora)**[KORA](https://github.com/bikass/kora)**
26. Installare *font microsoft*.

* `sudo apt install ttf-mscorefonts-installer`


27. **[MediaInfo](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)**[ tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)
28. Installare **[cpu-x](https://community.linuxmint.com/software/view/cpu-x)**
29. \
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
    * Virtualbox
30. Disinstallare **Firefox**, **Thunderbird**, **Matrix**, **Celluloid**

#### Extra


31. Sul Thinkpad installare **tlp** per l'ottimizzazione batteria

`sudo apt install tlp tlp-rdw`


32. Su Linux Mint Zara 22.2 abilitare *snap* per installare **acestreamplayer**



`sudo rm /etc/apt/preferences.d/nosnap.prefsudo apt install snapdsudo snap install acestreamplayer`