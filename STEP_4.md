# Step 4

## Obiettivo
L'obiettivo di questo step è creare un Workflow che utilizzi il modulo `set_stats` per definire una variabile globale in un playbook e riutilizzarla in un altro playbook nello stesso Workflow.

---

## Procedura

### 1. Creare il playbook `set_variable.yml`
Questo playbook definisce una variabile globale utilizzando il modulo `set_stats`.

```yaml
---
- name: Set a global variable
  hosts: all
  tasks:
    - name: Define a variable with set_stats
      set_stats:
        data:
          my_custom_variable: "Hello from the first playbook"
```

Salva questo file nella tua repository GitHub associata al progetto.

---

### 2. Creare il playbook `use_variable.yml`
Questo playbook utilizza la variabile globale definita nel playbook precedente e la mostra nell'output.

```yaml
---
- name: Use a global variable
  hosts: all
  tasks:
    - name: Display the variable
      debug:
        msg: "{{ my_custom_variable }}"
```

Salva anche questo file nella stessa repository GitHub.

---

### 3. Aggiornare il Project
1. Vai su **Projects** nella dashboard di Ansible Automation Platform.
2. Seleziona il progetto `Formazione AAP`.
3. Clicca su **Sync** per aggiornare il progetto con i nuovi playbook dalla repository.

---

### 4. Creare due Job Templates

**Template 1: Set Variable**
1. Vai su **Templates → Add (+) → Job Template**.
2. Compila i campi richiesti:
   - **Name**: `Set Variable`
   - **Job Type**: Run
   - **Inventory**: `Ubuntu Inventory`
   - **Project**: `Formazione AAP`
   - **Playbook**: `set_variable.yml`
   - **Credentials**: `Ubuntu Credential`
3. Salva il template.

**Template 2: Use Variable**
1. Vai su **Templates → Add (+) → Job Template**.
2. Compila i campi richiesti:
   - **Name**: `Use Variable`
   - **Job Type**: Run
   - **Inventory**: `Ubuntu Inventory`
   - **Project**: `Formazione AAP`
   - **Playbook**: `use_variable.yml`
   - **Credentials**: `Ubuntu Credential`
3. Salva il template.

---

### 5. Creare un Workflow Template
1. Vai su **Templates → Add (+) → Workflow Template**.
2. Compila i campi richiesti:
   - **Name**: `Set and Use Variable`
   - **Description**: Facoltativo.
   - **Organization**: Seleziona la tua organizzazione.
3. Salva il template.

---

### 6. Configurare i nodi del Workflow
1. Una volta salvato il Workflow Template, clicca su **Edit Workflow Visualizer**.
2. Aggiungi il primo nodo:
   - Clicca sul nodo centrale (Start).
   - Seleziona il Job Template `Set Variable`.
3. Aggiungi il secondo nodo:
   - Clicca sull'icona + sotto il nodo del primo Job Template.
   - Seleziona il Job Template `Use Variable`.
4. Salva il Workflow.

---

### 7. Lanciare il Workflow
1. Vai su **Templates** e clicca sull'icona del razzo accanto al Workflow Template `Set and Use Variable`.
2. Osserva l'output dei due playbook:
   - Il primo playbook definirà la variabile.
   - Il secondo playbook utilizzerà la variabile e la mostrerà nel suo output.

---

## Esempio di Output

### Output del primo playbook (`set_variable.yml`):
```plaintext
TASK [Define a variable with set_stats] ****************************************
ok: [indirizzo ip]
```

### Output del secondo playbook (`use_variable.yml`):
```plaintext
TASK [Display the variable] ****************************************************
ok: [indirizzo ip] => {
    "msg": [
        "Hello from the first playbook"
    ]
}
```

---

## Risultato Atteso

Il Workflow eseguirà con successo entrambi i playbook, mostrando la variabile definita nel primo playbook (`set_variable.yml`) durante l'esecuzione del secondo playbook (`use_variable.yml`).
