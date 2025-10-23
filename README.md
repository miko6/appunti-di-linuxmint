## My Linux Mint LMDE 7 customization ![alt text](https://github.com/miko6/appunti-di-linuxmint/blob/main/immagini/mintlogo.png "mintlogo")

#### Impostazioni del sistema

1. *Avvio automatico* nelle impostazioni di installazione.

2. Attivare **Timeshift**.

3. Impostazioni di sistema/Salvaschermo - disattivare le due voci in Impostazioni di blocco.

4. Disattivare *bluetooth* in Applicazioni d'avvio.

5. Rimuovere il *separatore* accanto al menù.

6. Azzerare tempo *bootloader*.

- `sudo nano /etc/default/grub`
- settare GRUB_TIMEOUT a 0
- dare un `sudo update-grub`

1. Attivare il **Firewall**

2. Settare un *IP statico* e cambiare *DNS*.

- Clic sull'icona di rete/Impostazioni di rete/Selezioniamo l'interfaccia di rete e clic sull'icona ingranaggio delle impostazioni, nella sezione IPV4 impostiamo i valori di Getaway e Subnet mask.
- Impostiamo anche i nostri *DNS*:

| DNS        | Primario | Secondario |
| ---------- | -------- | ---------- |
| Cloudflare | 1.1.1.1  | 1.0.0.1    |
| Google     | 8.8.8.8  | 8.8.4.4    |

9. *Data e ora* stile windows10.

- Tasto destro sull'orologio, Configura, Utilizza un formato data personalizzato e scrivi nel campo Formato della data %R%n%x

10. Dal *Gestore software* attivare i Flatpack non verificati (per installare ad esempio **Avidemux**).

11. Tasto destro sul desktop Personalizza per aggiungere le icone *Home*, *Cestino*, *Rete* ecc.

12. *Pannello trasparente*

- Aprire il menu Estensioni, aggiungere l'applet Pannelli trasparenti, attivarli.

13. *Migliorare font su temi scuri*
- `sudo nano /etc/environment`
- aggiungere la seguente stringa alla fine del file:
- | `FREETYPE_PROPERTIES="cff:no-stem-darkening=0 autofitter:no-stem-darkening=0"`

14. *Font rendering fix*
- Create a folder in home call it .fontconfig inside this folder create a blank text call it fonts.conf and paste this then log OFF/ON:  
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

15.  *Abilitare condivisione cartella*

- `sudo apt install samba` (se non installato)
- Controllare che l'utente in Utenti e Gruppi sia inserito nel gruppo sambashare
- `sudo smbpasswd -a nomeutente`
- digitare prima la password amministratore e poi due volte la password per la condivisione
- riavviare il server samba con `sudo service smbd restart` o riavviare il sistema
- Aggiungere una nuova regola sul firewall, le voci devono essere:  
``` | Politica | Consenti | ```  
``` | Direzione | Ingresso | ```  
``` | Categoria | Network | ```  
``` | Sotto-categoria | Tutte | ```  
``` | Applicazione | Samba | ```  
- Creare una cartella in una posizione a piacere (ad esempio il desktop), clic con il tasto destro e scegliere Opzioni condivisione, abilitare la voce Condividi questa cartella e mettere la spunta a Permetti ad altri di creare...  
- Modifichiamo ora il file smb.conf  
`sudo nano /etc/samba/smb.conf`  
* aggiungiamo le seguenti voci alla fine del file:  
` [nomeascelta]`  
` path = /home/nomeutente/nomecartella`  
` read only = no`  
` force user = nomeutente`  
` guest ok = no`  

#### Software

16. `sudo apt install htop`

17. `sudo apt install preload`  (caricamento in memoria dei programmi più usati)

18. `sudo apt install unrar`

19. `sudo apt install git `

20. `sudo apt install jq`  
> :memo: **Note:** jq in Linux è un processore di JSON da riga di comando, flessibile e leggero, utilizzato per manipolare e trasformare dati in formato JSON

21. Icone **NUMIX**
    
- `sudo apt install numix-icon-theme-circle `

22. [Icone **KORA**](https://github.com/bikass/kora)

23. Installare *font microsoft*.

- `sudo apt install ttf-mscorefonts-installer`

24. [**MediaInfo** tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)

25. Installare [**cpu-x**](https://community.linuxmint.com/software/view/cpu-x)

26. - [**Chrome**](https://support.google.com/chrome/a/answer/9025926?hl=it)
    - Avidemux
    - Arduino IDE
    - [**Edge**](https://www.microsoft.com/it-it/edge/download?form=MA13FJ)
    - [**Visual Studio Code**](https://code.visualstudio.com/docs/setup/linux)
    - Audacity
    - Freecad
    - Kodi
    - [**Acestreamplayer**](https://snapcraft.io/install/acestreamplayer/debian)
    - Gimp
    - Telegram
    - PDF Arranger