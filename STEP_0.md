# Step 0

## Obiettivo
Permettere alla macchina dove risiede l'Ansible Automation Platform Controller l'accesso di root a una VM target tramite scambio di chiavi SSH. Successivamente, configurare una Credential di tipo "Machine" sulla piattaforma contenente la chiave pubblica del Controller.

---

## **Prerequisiti**
1. Una macchina con Ansible Automation Platform installato (RHEL 9).
2. Una macchina target con sistema operativo Linux (Ubuntu 24) configurata per accettare connessioni SSH.
3. Accesso root o privilegi `sudo` su entrambe le macchine.

---

## **Procedura**

### **1. Configurazione della Macchina Target (Ubuntu 24)**

1. **Abilitare e verificare il servizio SSH sulla macchina target:**
   ```bash
   sudo systemctl enable --now ssh
   sudo systemctl status ssh
   ```

- Questo comando attiva e verifica che il servizio SSH sia in esecuzione.

2. **Verificare lo stato di UFW (firewall):**
   ```bash
   sudo ufw status
   ```
- Se attivo, consenti il traffico SSH con:
   ```bash
   sudo ufw allow ssh
   sudo ufw reload
   ```

3. **Consentire l'accesso root via SSH (se disabilitato):**

- Modifica il file di configurazione SSH:
   ```bash
   sudo vim /etc/ssh/sshd_config
   ```

- Trova e modifica i seguenti parametri:
   ```plaintext
   PermitRootLogin yes
   PasswordAuthentication yes
   ```

- Salva il file e riavvia il servizio SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

### **2. Generare la Chiave SSH sul Controller (RHEL 9)**

1. **Accedi al terminale del Controller.**

2. **Genera una coppia di chiavi RSA senza passphrase**:
   ```bash
   sudo ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ''
   ```

- Questo comando crea i file:
   - Chiave privata: `/root/.ssh/id_rsa`
   - Chiave pubblica: `/root/.ssh/id_rsa.pub`

---

### **3. Copiare la Chiave Pubblica sulla Macchina Target**

1. **Copia la chiave pubblica utilizzando `ssh-copy-id`**:
   ```bash
   sudo ssh-copy-id -i /root/.ssh/id_rsa.pub root@<indirizzo ip>
   ```

- Inserisci la password di root della macchina target quando richiesto.

2. **Se la connessione non funziona (ad esempio, "Connection refused" o "Permission denied")**:

- Verifica che il servizio SSH sia attivo sulla macchina target:
   ```bash
   sudo systemctl status ssh
   ```

- Controlla le impostazioni di `/etc/ssh/sshd_config` sulla target, assicurandoti che `PermitRootLogin` e `PasswordAuthentication` siano configurati correttamente, come indicato nel punto precedente.

- Riavvia il servizio SSH:
   ```bash
   sudo systemctl restart ssh
   ```

3. **Verifica la connessione SSH dal Controller alla Target:**
   ```bash
   ssh root@<indirizzo ip>
   ```

- Se tutto è configurato correttamente, l'accesso non richiederà la password.

---

### **4. Creare una Credential di tipo "Machine" su AAP**

1. Accedi alla dashboard di Ansible Automation Platform sul Controller (RHEL 9).
2. Dal menu laterale, vai su **Resources** → **Credentials**.
3. Clicca su **Add (+)** per creare una nuova credenziale.
4. Compila i campi come segue:
   - **Name**: `Ubuntu Target Credential`.
   - **Description**: Facoltativo.
   - **Organization**: Seleziona `Default`.
   - **Credential Type**: Seleziona `Machine`.
   - **Inputs**:
     - **Username**: `root`.
     - **Private Key**: Incolla il contenuto del file `/root/.ssh/id_rsa` (chiave privata).
     - **Privilege Escalation Method**: Lascia vuoto per accesso root diretto.

5. Clicca su **Save**.

---

## **Risultato**
- Il Controller può ora accedere alla macchina target tramite chiave SSH senza richiedere la password.
- Una credenziale di tipo "Machine" è stata configurata su Ansible Automation Platform, pronta per essere utilizzata nei progetti futuri.

---

## **Risorse Utili**
- [Ansible Documentation: Credential Types](https://docs.ansible.com/automation-controller/latest/html/userguide/credentials.html)
- [SSH Keygen Manual](https://linux.die.net/man/1/ssh-keygen)
