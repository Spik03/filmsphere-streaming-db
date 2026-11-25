# 🎬 FilmSphere — Database Relazionale & Motore di Analytics CDN

Progettazione completa e implementazione su MySQL di una base di dati relazionale per **FilmSphere**, una piattaforma globale di video streaming on-demand (VOD) supportata da una Content Delivery Network (CDN) distribuita.

Sviluppato per il corso di **Basi di Dati** presso l'**Università di Pisa** (Corso di Laurea Triennale in Ingegneria Informatica).

## 📌 Panoramica

FilmSphere modella l'intero livello dati di un servizio di streaming internazionale attivo in **193 nazioni** e dimensionato per oltre **240.000 utenti**, **3.000 titoli** e **48 milioni di visualizzazioni in cronologia**. Oltre alle operazioni transazionali standard (abbonamenti, fatturazione, verifica di compatibilità dei dispositivi e recensioni), il database implementa:

- **Schema Relazionale Normalizzato in BCNF**, derivato dalla progettazione concettuale E-R e relativa ristrutturazione (eliminazione delle generalizzazioni, reificazione di attributi e scomposizione di attributi multivalore).
- **Ridondanze Giustificate da Analisi Costi-Benefici**, validate tramite tavole dei volumi e degli accessi e mantenute automaticamente da **25 Trigger MySQL**.
- **Materialized View con Aggiornamento Settimanale** (`RatingFilm`, `RatingUtente`, `PoP`), gestite tramite `EVENT` schedulati in MySQL per alimentare il sistema di raccomandazione personalizzata e il caching proattivo sui server edge.
- **Routing CDN & Bilanciamento del Carico**, basati sulla formula dell'emisenoverso (Haversine) per instradare gli utenti verso il server Point of Presence (PoP) più vicino con il contenuto in cache e larghezza di banda disponibile.
- **Data Analytics Lato Server**, con classifiche multi-criterio delle visualizzazioni e analisi dell'impatto delle restrizioni geografiche.

## 🗂️ Struttura della Repository

```text
filmsphere-streaming-db/
├── Diagrams/
│   ├── ER Non Ristrutturato.png    # Diagramma E-R non ristrutturato
│   └── ER Ristrutturato.png        # Diagramma E-R ristrutturato
├── Docs/
│   ├── Project report IT.pdf       # Documentazione completa di progetto (89 pagine)
│   ├── ER diagrams IT.pdf          # PDF unico con diagrammi E-R concettuale e ristrutturato
│   └── Progetto A.A. 22-23.pdf     # Specifiche ufficiali del progetto d'esame
├── Sql/
│   ├── 00_full_script/             # Script completo standalone (FilmSphere.sql)
│   ├── 00_test/                    # Query di test ed esempi di chiamata alle procedure
│   ├── 01_schema/                  # DDL: tabelle, vincoli PK/FK, CHECK, Trigger ed Event
│   ├── 02_materialized_views/      # Tabelle RatingFilm, RatingUtente, PoP e procedure di refresh
│   ├── 03_analytics/               # Procedure per classifiche e analisi restrizioni geografiche
│   ├── 04_operations/              # Stored procedure applicative principali (streaming, abbonamenti, ecc.)
│   └── 05_populate/                # Script di popolamento del dataset (DML)
└── README.md
```

## 🚀 Come Eseguirlo

1. Assicurati di avere **MySQL Server 8.0+** installato e avviato.
2. Abilita l'Event Scheduler di MySQL (opzionale, necessario per l'aggiornamento settimanale automatico delle Materialized View):
   ```sql
   SET GLOBAL event_scheduler = ON;
   ```
3. Esegui lo script completo di creazione e popolamento da terminale MySQL o MySQL Workbench:
   ```sql
   SOURCE Sql/00_full_script/FilmSphere.sql;
   ```
   *(In alternativa, esegui in ordine sequenziale gli script contenuti nelle cartelle da `Sql/01_schema/` a `Sql/05_populate/`.)*
4. Esegui le query di esempio in `Sql/00_test/` per testare raccomandazioni, routing dello streaming e classifiche analytics.

## 📄 Documentazione

La documentazione completa del progetto è disponibile in italiano:

- 📘 **[Relazione Completa di Progetto (PDF)](<Docs/Project report IT.pdf>)**
- 📐 **[Diagrammi E-R Concettuale e Ristrutturato (PDF)](<Docs/ER diagrams IT.pdf>)**
- 📋 **[Specifiche del Progetto d'Esame (PDF)](<Docs/Progetto A.A. 22-23.pdf>)**
