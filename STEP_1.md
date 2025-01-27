# Step 1

## Obiettivo
In questo step, il nostro obiettivo è creare un nuovo **Project** su Ansible Automation Platform (AAP) e sincronizzarlo con un repository GitHub contenente dei playbook. Inoltre, useremo un **Personal Access Token (PAT)** per accedere al repository privato e configureremo una **Credential di tipo GitHub PAT**.

## Passaggi

### 1. Configurare il repository GitHub
   - Assicurati che il tuo repository su GitHub sia **privato**.
   - Genera un **Personal Access Token (PAT)** per il tuo account GitHub:
     1. Accedi al tuo profilo GitHub.
     2. Vai su **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**.
     3. Clicca su **Generate new token** e dai il nome `Ansible PAT`.
     4. Seleziona le seguenti autorizzazioni:
        - `repo`: per consentire l'accesso completo ai repository privati.
     5. Clicca su **Generate token** e copia il token generato (sarà visibile solo una volta).

### 2. Creare una Credential di tipo "GitHub Personal Access Token"
   - Accedi alla dashboard dell'Automation Controller.
   - Vai su **Resources** → **Credentials**.
   - Clicca su **Add (+)** per creare una nuova credenziale.
   - Compila i seguenti campi:
     - **Name**: `GitHub Token`
     - **Credential Type**: Seleziona `Source Control`.
     - **Source Control Credential Type**: Seleziona `Personal Access Token`.
     - **Token**: Incolla il PAT generato in precedenza.
   - Salva la credenziale.

### 3. Creare un nuovo Project
   - Vai su **Resources** → **Projects**.
   - Clicca su **Add (+)** per creare un nuovo progetto.
   - Compila i seguenti campi:
     - **Name**: `Formazione AAP`.
     - **Source Control Type**: `Git`.
     - **Source Control URL**: Incolla l'URL del tuo repository privato.
     - **Source Control Credential**: Seleziona la credenziale `GitHub Token`.
   - Salva il progetto.

### 4. Sincronizzare il Project
   - Una volta creato il progetto, clicca sul pulsante **Sync** per sincronizzare il progetto con il repository GitHub.
   - Verifica che i file del repository (incluso il playbook) siano stati correttamente scaricati e resi disponibili per l'uso in AAP.

## Completato!
Il tuo **Project** è ora sincronizzato con il repository GitHub contenente i playbook. La configurazione del **GitHub PAT** e la sincronizzazione sono state completate con successo, consentendo ad AAP di accedere al repository privato per eseguire i playbook.

---

### Risorse Utilizzate:
- **GitHub Personal Access Token (PAT)**
- **Ansible Automation Platform Project**
