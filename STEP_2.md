# Step 2

## Obiettivo
Creare un Job Template basato sul progetto sincronizzato nello Step 1 e lanciare un playbook utilizzando il modulo `ping` per testare la connessione tra il Controller e la macchina target.

---

## Procedura

### 1. Verifica del Progetto
Assicurarsi che il progetto `Formazione AAP` creato nello Step 1 sia sincronizzato e che il playbook `test_ping.yml` sia disponibile nella propria repository configurata.

---

### 2. Configurare l'Inventory
1. Vai su **"Inventories"**:
   - Dal menu laterale, clicca su **Resources → Inventories**.
2. Aggiungi un Nuovo Inventory:
   - Clicca su **Add (+)** per creare un nuovo inventario.
   - Compila i campi:
     - **Name**: Ad esempio, `Ubuntu Inventory`.
     - **Description**: Facoltativo.
     - **Organization**: Seleziona l'organizzazione appropriata.
   - Salva l'inventario.
3. Aggiungi un Host:
   - Dopo aver salvato l'inventario, clicca su **Hosts** in alto.
   - Clicca su **Add (+)** e aggiungi un host:
     - **Name**: L'indirizzo IP della tua macchina Ubuntu target.
     - **Description**: Facoltativo.
     - **Variables**: Non è necessario aggiungere variabili specifiche in questo caso.
   - Salva il nuovo host.
4. Test della connettività (facoltativo):
   - Dopo aver aggiunto l'host, clicca su **Run Command → Test Host Connectivity** per verificare che il Controller possa raggiungere la macchina target.

---

### 3. Creazione del Job Template
1. Accedere alla dashboard dell'Automation Controller.
2. Andare su **Templates** → **Add (+)** → **Job Template**.
3. Compilare i campi come segue:
   - **Name**: `Ping Test Template`
   - **Job Type**: `Run`
   - **Inventory**: Selezionare l’inventario configurato nel passaggio precedente (`Ubuntu Inventory`).
   - **Project**: `Formazione AAP`
   - **Playbook**: `test_ping.yml`
   - **Credentials**: Selezionare la credenziale di tipo `Machine` creata nello Step 0.
4. Salvare il Job Template.

---

### 4. Esecuzione del Job Template
1. Tornare alla lista dei **Templates**.
2. Individuare il template `Ping Test Template` e cliccare su **Launch**.
3. Osservare l’esecuzione del Job e verificare che non siano presenti errori.

---

## Output Atteso
Durante l’esecuzione del Job Template, l’output dovrebbe mostrare che la connessione alla macchina target è riuscita. Un esempio di output positivo è il seguente:
```plaintext
Identity added: /runner/artifacts/2/ssh_key_data (root@aap-luca)

PLAY [Test connectivity to the target] *****************************************

TASK [Gathering Facts] *********************************************************
ok: [<indirizzo ip>]

TASK [Ping the target machine] *************************************************
ok: [<indirizzo ip>]

PLAY RECAP *********************************************************************
<indirizzo ip>            : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

---

## Note
- Il playbook `test_ping.yml` utilizza il modulo `ping` per verificare la raggiungibilità della macchina target.
- Assicurarsi che le credenziali, il progetto e l’inventario siano configurati correttamente per evitare errori durante l'esecuzione.

---

## Prossimi Passi
Proseguire con lo **Step 3**, dove creeremo un Workflow per eseguire più playbook in serie.
