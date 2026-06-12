## Comandi utili per LINUX

1. Creazione dello Script Bash per sincronizzare cartelle con il server grazie a *rsync*

Apri il terminale e digita:

`nano ~/syncnome.sh`

Incolla il codice (modificando i percorsi delle cartelle di partenza):

```
#!/bin/bash

echo "=== Inizio Sincronizzazione Immagini ==="
# Escludiamo una cartella chiamata "Inutili" e una chiamata "Personale" dentro Immagini
rsync -av --progress \
  --exclude="Inutili/" \
  --exclude="Personale/" \
  /home/domenico/Immagini/ 192.168.1.xxx:/mnt/nasm2/wall/

echo "=== Inizio Sincronizzazione Scaricati ==="
# Escludiamo una cartella chiamata "Cache" e tutti i file .iso non finiti (es. di Torrent)
rsync -av --progress \
  --exclude="Cache/" \
  --exclude="*.iso.part" \
  /home/domenico/Scaricati/ 192.168.1.192:/mnt/nasm2/condivisione/

echo "=== Backup dello Script sul Server ==="
rsync -av --progress "$0" 192.168.1.xxx:/mnt/nasm2/

echo "=== Sincronizzazione Completata! ==="

```

Salva (CTRL+O, Invio) ed esci (CTRL+X), poi rendilo eseguibile:

`chmod +x ~/sync_to_server.sh`

`./syncnome.sh` per lanciare il backup

---

2. Comandi base per **fail2ban**

- *Check Fail2ban status*

`sudo systemctl status fail2ban`

- *Start Fail2ban*

`sudo systemctl start fail2ban`

- *Stop Fail2ban*

`sudo systemctl stop fail2ban`

- *Restart Fail2ban*

`sudo systemctl restart fail2ban`

- *Reload Fail2ban configuration without restarting*

`sudo fail2ban-client reload`

- *Enable Fail2ban to start on boot*

`sudo systemctl enable fail2ban`

- *Disable Fail2ban from starting on boot*

`sudo systemctl disable fail2ban`

- *View a list of all jails*

`sudo fail2ban-client status`

##### Example output

```
Status
|- Number of jail: 1
`- Jail list: sshd
```

- *List all banned IP addresses in all jails*

`sudo fail2ban-client banned`

- *Check the status of a specific jail (e.g., sshd)*

`sudo fail2ban-client status <JAIL>`

- *Shows the status of a specific Fail2ban jail, such as SSH:*

`sudo fail2ban-client status sshd`

##### Example output for the jail sshd

```
Status for the jail: sshd
|- Filter
|  |- Currently failed:	1
|  |- Total failed:	23829
|  `- File list:	/var/log/auth.log
`- Actions
   |- Currently banned:	2
   |- Total banned:	2569
   `- Banned IP list:	192.0.2.1 198.51.100.1
```

- *Manually ban an IP address*

`sudo fail2ban-client set <JAIL> banip <IP>`

The specified IP is banned and included in the specified jail. E.g., if you want to ban an IP from connect through SSH:

`sudo fail2ban-client set sshd banip 192.0.2.1`

- *Manually unban an IP address*

`sudo fail2ban-client set <JAIL> unbanip <IP>`

The specified IP in the specified jail is unbanned. E.g., if you want to unban an IP and allow it to connect through SSH:

`sudo fail2ban-client set sshd unbanip 192.0.2.1`

- *Display the current Fail2ban version*

`sudo fail2ban-client version`

- *Check fail2ban.log*

`sudo tail /var/log/fail2ban.log`

- *Get help and information about all Fail2ban commands*

`sudo fail2ban-client --help`  

---

3. Comandi base di **ufw**

- *Abilitare il firewall*

`sudo ufw enable`

- *Disabilitare il firewall*

`sudo ufw disable`

- *Verificare lo stato*

`sudo ufw status verbose`

- *Ripristinare le impostazioni di fabbrica*

`sudo ufw reset`

- *Bloccare tutto il traffico in entrata*

`sudo ufw default deny incoming`

- *Consentire tutto il traffico in uscita*

`sudo ufw default allow outgoing`

- *Consentire una porta (es. 22)*

`sudo ufw allow 22`

- *Bloccare una porta*

`sudo ufw deny 22`

- *Consentire una porta specifica per un protocollo*

`sudo ufw allow 80/tcp`

- *Consentire un servizio specifico (es. OpenSSH)*

`sudo ufw allow OpenSSH`

- *Rimuovere una regola: anteponi delete al comando originale (es. `sudo ufw delete allow 22`)

- *Consentire l'accesso da un IP specifico*

`sudo ufw allow from 192.168.1.100`

- *Bloccare un indirizzo IP specifico*

`sudo ufw deny from 203.0.113.110`

- *Consentire un indirizzo IP su una porta specifica*

`sudo ufw allow from 192.168.1.100 to any port 22`  

---
 
 4. Creazione della SSH-KEY in Windows 10 e invio al server
 
 - Aprire la PowerShell e digitare:
 
 `ssh-keygen`
 
 - Rispondere con INVIO alle richieste e annotare il percorso dove è stata salvata la key.
 
 es.
 ```
 C:\Users\nomeutente/.ssh/id_ed25519.pub
 ```
 - Sempre da PowerShell digitare il seguente comando per inviare la chiave al server (se non lo si era aftto prima è necessario loggarsi almeno una volta al server con la PowerShell prima di mandare la chiave che altrimenti non verrà accettata)
 
 `cat C:\Users\nomeutente/.ssh/id_ed25519.pub | ssh nomeutente_server@192.168.1.xxx "cat >> ~/.ssh/authorized_keys"`  
