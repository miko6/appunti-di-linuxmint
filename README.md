## My Linux Mint LMDE 7 customization ![alt text](https://github.com/miko6/appunti-di-linuxmint/blob/main/immagini/mintlogo.png "mintlogo")

1. *Avvio automatico* nelle impostazioni di installazione.

2. Attivare **Timeshift**.

3. Impostazioni di sistema/Salvaschermo - disattivare le due voci in Impostazioni di blocco.

4. Disattivare *bluetooth* in Applicazioni d'avvio.

5. Rimuovere il *separatore* accanto al menù.

6. Azzerare tempo *bootloader*.

- `sudo nano /etc/default/grub`
- settare GRUB_TIMEOUT a 0
- dare un `sudo update-grub`

7. Attivare il **Firewall**

8. Settare un *IP statico* e cambiare *DNS*.

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