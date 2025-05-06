# Permessi Warehouse
| Ruolo | Descrizione | Permesso WS | Sharable | Note |
| ----- | ----------- | ----------- | -------- | ---- |
| Read | Permesso di connessione. Come il permesso `CONNECT` su SQL | Viewer+ | Y | Da solo non serve a nulla. Bisogna almeno dare un `GRANT`. Non è necessario fare `CREATE USER`. Documentazione sui permessi SQL [qui](https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-granular-permissions?source=recommendations) |
| ReadData | Come `db_datareader` su SQL, può vedere *tutte* le tabelle e viste | Viewer+ | Y | E' possibile usare i permessi `GRANT`/`REWOKE`/`DENY` |
| ReadAll | Accesso in lettura completo, anche ai file e ai path di OneLake da usare su Spark. | Contributor+ | Y | |
| Build | Creare report usando il dataset PBI associato di default | Contributor+ | Y | |
| Write & Monitor | | Contributor+ | N | Funzionalità di [Query Insights](https://learn.microsoft.com/en-us/fabric/data-warehouse/query-insights)|
| Creation e Reshare | Creare i warehouse. Condidivere e permettere a persone non Admin/Member di condividere | Member+ | N | |
| Restore e Audit | Possibilità di monitorare le sessioni e fare `KILL` di sessioni | Admin | N | Audit tramite [DMV](https://learn.microsoft.com/en-us/fabric/data-warehouse/monitor-using-dmv) e [Query Activity](https://learn.microsoft.com/en-us/fabric/data-warehouse/query-activity). I [`RESTORE`](https://learn.microsoft.com/en-us/fabric/data-warehouse/restore-in-place) possono essere di due tipi: system-defined oppure user-defined. I primi sono fatti ogni 8 ore e hanno un periodo di retention di 30 giorni.

E' possibile fare Object-level security (**OLS**), Column-level security (**CLS**), Row-level security (**RLS**) e **Dynamic Data Masking**. Link [qui](https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-granular-permissions?source=recommendations#data-protection-features).

- https://learn.microsoft.com/en-us/fabric/data-warehouse/share-warehouse-manage-permissions
- Monitor e audit: https://learn.microsoft.com/en-us/fabric/data-warehouse/monitoring-overview
- https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-granular-permissions
- https://learn.microsoft.com/en-us/fabric/data-warehouse/security

- https://learn.microsoft.com/en-us/fabric/fundamentals/roles-workspaces
- https://learn.microsoft.com/en-us/fabric/data-warehouse/workspace-roles

| Artefatto | Attività | Admin | Developer backend full | Developer backend one-feature | Developer backend PBI full | Developer backend PBI one-model | Developer backend one-feature and one-model | 
| --------- | -------- | ----- | ---------------------- | ----------------------------- | -------------------------- | ------------------------------- | ------------------------------------------- |
Warehouse | Creare i warehouse e gestire le condivisioni | Admin WS |
Warehouse | Monitorare i warehouse | Admin WS |
Warehouse | Sviluppo | | Contributor WS
Warehouse | Sviluppo di un filone singolo | | | *Reader* e `GRANT` dedicati | | | *Reader* e `GRANT` dedicati
Power BI model | Impostare le connessioni e gli aggiornamenti dati | | | | Contributor WS
Power BI model | Sviluppo | | | | Contributor WS
Power BI model | Sviluppo di un modello singolo | | | | | *Write* | *Write*

# Permessi Power BI semantic model
| Ruolo | Descrizione | Permesso WS | Sharable | Note |
| ----  | ----------- | ----------- | -------- | ---- |
Read | Accesso ai dati | Viewer+ | Y | Subisce eventuali RLS
Build | Possibilità di creare nuovi report Power BI | Contributor+ | Subisce eventuali RLS
Reshare | Condidivere e permettere a persone non Admin/Member di condividere | Member+ | Y |
Write | | Contributor+ | Y |
Owner | Può impostare le connessioni dati e gli aggiornamenti schedulati | Contributor+ | N | Può essere una *persona singola*.

E' possibile fare OLS e RLS. Da studiare: Dynamic Data Masking in caso di Direct Query verso il warehouse
https://learn.microsoft.com/en-us/power-bi/connect-data/service-datasets-permissions
