## My Linux Mint LMDE 7 customization ![alt text](https://github.com/miko6/appunti-di-linuxmint/blob/main/immagini/mintlogo.png "mintlogo")

#### <p style="text-align: center;"> Impostazioni del sistema </p>

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

#### <p style="text-align: center;"> Software </p>

14. Installare **htop**

- `sudo apt install htop`

15. Installare **preload** (caricamento in memoria dei programmi più usati)
    
- `sudo apt install preload`

16. `sudo apt install unrar`

17. `sudo apt install git `

18. Icone **NUMIX**
    
- `sudo apt install numix-icon-theme-circle `

19. [Icone **KORA**](https://github.com/bikass/kora)

20. Installare *font microsoft*.

- `sudo apt install ttf-mscorefonts-installer`

21. [**MediaInfo** tab](https://github.com/linux-man/nemo-mediainfo-tab/releases/tag/v1.0.4)

22. Installare [**cpu-x**](https://community.linuxmint.com/software/view/cpu-x)

23. - [**Chrome**](https://support.google.com/chrome/a/answer/9025926?hl=it)
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



