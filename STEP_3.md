# Step 3: Creare un Workflow e lanciare in serie più playbook

## Obiettivo
Creare un Workflow che esegua in sequenza due playbook:
1. Il primo playbook effettua un ping alla macchina target.
2. Il secondo playbook verifica l'uso del disco sulla macchina target.

## Procedura

### 1. Creare un Job Template per il secondo playbook
1. Vai su **Templates** dal menu principale.
2. Clicca su **Add (+)** e seleziona **Job Template**.
3. Compila i campi richiesti:
   - **Name**: `Disk Check Template`
   - **Job Type**: Run
   - **Inventory**: Seleziona l'inventory creato precedentemente (es. `Ubuntu Inventory`).
   - **Project**: Seleziona il progetto `Formazione AAP`.
   - **Playbook**: Scegli il playbook `check_disk.yml` dalla lista.
   - **Credentials**: Seleziona la credential di tipo `Machine` (es. `Ubuntu Credential`).
4. Clicca su **Save**.

### 2. Creare un Workflow Template
1. Vai su **Templates** dal menu principale.
2. Clicca su **Add (+)** e seleziona **Workflow Template**.
3. Compila i campi richiesti:
   - **Name**: `Workflow Template`
   - **Description**: Facoltativo.
   - **Organization**: Seleziona l'organizzazione appropriata.
4. Clicca su **Save**.

### 3. Configurare i nodi del Workflow
1. Una volta salvato il Workflow Template, clicca su **Edit Workflow Visualizer**.
2. Aggiungi il primo nodo:
   - Clicca sul nodo centrale (Start).
   - Seleziona il Job Template `Ping Test Template`.
3. Aggiungi il secondo nodo:
   - Clicca sull'icona **+** sotto il nodo del primo Job Template.
   - Seleziona il Job Template `Disk Check Template`.
4. Clicca su **Save** per salvare il Workflow.

### 4. Lanciare il Workflow
1. Torna su **Templates**.
2. Clicca sull'icona del razzo accanto al Workflow Template.
3. Verifica i risultati al completamento del Workflow:
   - Il primo playbook confermerà la connettività con il target.
   - Il secondo playbook mostrerà l'uso del disco del target.

## Esempio di Output
Ecco un esempio dell'output generato dal secondo playbook, che mostra l'utilizzo del disco sulla macchina target:

```plaintext
PLAY [Check disk usage] ********************************************************

TASK [Gathering Facts] *********************************************************
ok: [<indirizzo ip>]

TASK [Display disk usage] ******************************************************
changed: [<indirizzo ip>]

TASK [Show disk usage output] **************************************************
ok: [<indirizzo ip>] => {
    "msg": [
        "File system     Dim. Usati Dispon. Uso% Montato su",
        "tmpfs           387M  2,0M    385M   1% /run",
        "/dev/sda2        20G   15G    3,8G  80% /",
        "tmpfs           1,9G     0    1,9G   0% /dev/shm",
        "tmpfs           5,0M  8,0K    5,0M   1% /run/lock",
        "tmpfs           387M  120K    387M   1% /run/user/1000",
        "/dev/sr0         88M   88M       0 100% /media/luca-cisotto/CDROM",
        "tmpfs           387M   84K    387M   1% /run/user/0"
    ]
}

PLAY RECAP *********************************************************************
<indirizzo ip>            : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Conclusioni
Con questo workflow, abbiamo integrato due task essenziali in un'unica esecuzione automatizzata:
1. La verifica della connettività al target.
2. Il controllo dell'uso del disco per monitorare lo stato della macchina.

Questo approccio dimostra l'efficacia dell'uso di Workflow Templates su Ansible Automation Platform per coordinare più playbook in maniera sequenziale.
