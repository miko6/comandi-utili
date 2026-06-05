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
  /home/domenico/Immagini/ 192.168.1.192:/mnt/nasm2/wall/

echo "=== Inizio Sincronizzazione Scaricati ==="
# Escludiamo una cartella chiamata "Cache" e tutti i file .iso non finiti (es. di Torrent)
rsync -av --progress \
  --exclude="Cache/" \
  --exclude="*.iso.part" \
  /home/domenico/Scaricati/ 192.168.1.192:/mnt/nasm2/condivisione/

echo "=== Backup dello Script sul Server ==="
rsync -av --progress "$0" 192.168.1.192:/mnt/nasm2/

echo "=== Sincronizzazione Completata! ==="

```

Salva (CTRL+O, Invio) ed esci (CTRL+X), poi rendilo eseguibile:

`chmod +x ~/sync_to_server.sh`

`./syncnome.sh` per lanciare il backup
