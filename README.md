# Installazione e utilizzo di Ansible Automation Platform

## Panoramica
Questo repository fornisce una guida pratica per configurare e utilizzare **Ansible Automation Platform (AAP)**, automatizzando attività su macchine Linux. I passaggi includono la configurazione di AAP, la gestione di playbook e la creazione di Workflow per eseguire attività in sequenza o dinamicamente.

## Installazione di Ansible Automation Platform
Per configurare AAP, segui questi passaggi generali:

1. **Requisiti di Sistema**:
   - Sistema operativo compatibile, come RHEL o una distribuzione Linux simile.
   - Database compatibile per l'installazione (ad esempio, PostgreSQL).
   - Accesso alle macchine target per eseguire i playbook.

2. **Procedura di Installazione**:
   - **Configurazione di Sistema**: Assicurati che il sistema sia aggiornato e che siano installati i pacchetti necessari.
   - **Installazione di AAP**: Segui le istruzioni ufficiali per scaricare e configurare AAP nel tuo ambiente.
   - **Post-Installazione**: Accedi al Controller tramite l'interfaccia web per configurare il sistema e il tuo primo utente.

## Obiettivi e Funzionalità
Il progetto si concentra sui seguenti obiettivi:
- **Automazione di Base**: Esegui test di connettività e verifica dello spazio disco.
- **Workflow Avanzati**: Crea e gestisci Workflow per eseguire playbook in sequenza e condividere variabili globali.

## Struttura del Repository
Il repository include i seguenti playbook:

- `test_ping.yml`: Verifica la connettività con la macchina target.
- `check_disk.yml`: Controlla lo stato del disco sulla macchina target.
- `set_variable.yml`: Definisce una variabile globale.
- `use_variable.yml`: Usa la variabile globale definita nel playbook precedente.

## Come Iniziare
1. **Configurazione iniziale**: Installa AAP e configura le macchine target.
2. **Progetto GitHub**: Sincronizza il tuo progetto con AAP utilizzando il repository GitHub contenente i playbook.
3. **Creazione di Job Templates**: Crea Job Templates per eseguire i playbook (`test_ping.yml`, `check_disk.yml`).
4. **Creazione di Workflow**: Configura Workflow Templates per eseguire i playbook in sequenza, condividendo variabili tramite `set_stats`.
