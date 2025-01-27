# Installazione e utilizzo di Ansible Automation Platform

## Scopo
Questo esercizio ha come obiettivo fornire una conoscenza di base dell'Ansible Automation Platform (AAP), partendo dall'installazione e configurazione tramite CLI, fino all'utilizzo della GUI e delle sue componenti fondamentali.

### Prerequisiti
- Conoscenza di base di Ansible e della struttura di un playbook.
- Conoscenza di base dei sistemi RHEL.
- Una VM RHEL 9.4 già configurata (minimo 8 GB di RAM).

---

## Componenti di Ansible Automation Platform
L'Ansible Automation Platform si compone di:
1. **Controller**: Contiene la GUI e gestisce l'esecuzione dei playbook (precedentemente noto come Ansible Tower).
2. **Hub** (opzionale): Repository centrale per le collections scaricate da repository remoti.
3. **Database**: PostgreSQL, utilizzato per memorizzare i dati.

In questo esercizio, è stata effettuata un'installazione single-node, dove il Controller e il Database risiedono sullo stesso nodo.

---

## Installazione

### 1. Preparazione
Scaricare un’immagine ISO di RHEL 9.4 conforme alla propria architettura (x86_64 o aarch64) dal sito Red Hat (https://access.redhat.com/downloads) utilizzando credenziali aziendali. 

Dopo aver creato la VM e completato l'installazione del sistema operativo, procedere con l’installazione dell’Automation Platform.

---

### 2. Configurazione del repository
1. **Agganciare il pool per il repository (solo se necessario)**:
   ```bash
   sudo subscription-manager list --available --all
   sudo subscription-manager attach --pool=<pool_id>
   ```

2. **Verificare i repository disponibili**:
   ```bash
   sudo subscription-manager repos --list
   ```

3. **Abilitare il repository di AAP**:
   ```bash
   sudo subscription-manager repos --enable ansible-automation-platform-2.4-for-rhel-9-x86_64-rpms
   ```

4. **Verificare le versioni disponibili dei pacchetti AAP** (opzionale):
   ```bash
   sudo dnf list --showduplicates ansible-automation-platform-installer
   ```

   Installeremo la versione `2.4_7.1`.

6. **Installare il pacchetto richiesto**:
   ```bash
   sudo dnf -y install ansible-automation-platform-installer
   ```

---

### 3. Configurazione

**a. Preparazione dei file di configurazione**

1. Spostarsi nella directory di installazione:
   ```bash
   cd /opt/ansible-automation-platform/installer
   ```

2. Creare un file `credentials.yml` con le seguenti credenziali:
   ```yaml
   admin_password: <controller password>
   pg_password: <postgresql password>
   registry_password: <red hat registry password>
   ```

3. Criptare il file di credenziali:
   ```bash
   sudo ansible-vault encrypt credentials.yml
   ```

**b. Configurazione del file `/etc/hosts`**

1. Settare l'hostname della macchina per identificare il nodo. Per esempio, per impostare l'hostname come aap-luca, usare:
   ```bash
   sudo hostnamectl set-hostname aap-luca
   ```

2. Modificare il file `/etc/hosts` per aggiungere il nome della macchina e l'indirizzo IP statico. Ad esempio:
   ```
   <indirizzo ip> aap-luca
   ```

**c. Configurazione della chiave SSH**

1. Generare una chiave SSH, se non già presente:
   ```bash
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
   ```

2. Copiare la chiave pubblica sulla stessa macchina per consentire l'accesso senza password:
   ```bash
   ssh-copy-id -i ~/.ssh/id_rsa.pub root@aap-luca
   ```

**d. Configurazione dell'inventory**

Creare un file di inventory con il seguente contenuto:
```ini
[automationcontroller]
aap-luca ansible_ssh_user=root ansible_ssh_private_key_file=/home/lucacisotto/.ssh/id_rsa

[all:vars]
admin_password=<controller password>

registry_url='registry.redhat.io'
registry_username=<redhat-subscription-username>
registry_password=<red hat registry password>
```

---

### 4. Esecuzione dell'installer

Lanciare l'installer utilizzando il comando:
   ```bash
   sudo ANSIBLE_BECOME_METHOD='sudo' ANSIBLE_BECOME=True ANSIBLE_HOST_KEY_CHECKING=False ./setup.sh -e @credentials.yml -e receptor_datadir=/var/lib/awx/receptor -- --ask-vault-pass
   ```

---

## Accesso al Controller

Al termine dell’installazione:
1. Connettersi alla dashboard tramite un browser **dalla VM stessa**:
- Usare l'indirizzo `https://aap-luca` oppure l'indirizzo IP configurato in `/etc/hosts`.
2. Accedere con le credenziali dell’utente `admin` configurate.
3. Seguire le istruzioni per:
- Inserire le credenziali Red Hat.
- Accettare il license agreement.
