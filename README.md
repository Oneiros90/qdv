# QDV

Pagina statica che aiuta a trovare la citta italiana piu adatta alle preferenze dell'utente usando i dataset annuali della Qualita della Vita del Sole 24 Ore.

## Cosa fa

- Scarica dinamicamente il CSV piu recente disponibile dal repository GitHub annuale del progetto QDV.
- Se l'anno corrente non esiste ancora, scala all'indietro fino a trovare il repository pubblicato piu recente.
- Legge il nome reale del CSV dal repository GitHub, invece di assumere una data fissa nel filename.
- Costruisce un questionario con tutti gli indicatori disponibili.
- Permette di assegnare un peso a ogni indicatore.
- Permette di marcare indicatori preferiti con una stella, forzandoli a priorita massima.
- Calcola una classifica personalizzata delle citta in base all'ordine migliore-peggiore gia presente nel CSV per ogni indicatore.

## Interfaccia

La pagina e composta da due colonne.

- A sinistra ci sono il pannello dati, i controlli e la lista completa degli indicatori.
- A destra ci sono la citta consigliata, i motivi principali e la classifica completa.

Funzionalita principali:

- Ricerca testuale degli indicatori.
- Preset rapidi per impostare tutti i pesi a `0`, `5` o `10`.
- Stato aperto/chiuso delle sezioni salvato nel browser.
- Preferenze dei pesi salvate nel browser.
- Evidenziazione visuale degli indicatori marcati come prioritari.

## Come funziona il ranking

Per ogni indicatore, il dataset ordina gia le citta dalla migliore alla peggiore. La pagina usa direttamente quell'ordine per costruire un punteggio normalizzato:

- la prima citta ottiene il punteggio piu alto;
- l'ultima ottiene il punteggio piu basso;
- il contributo di ogni indicatore viene moltiplicato per il peso scelto dall'utente;
- gli indicatori con stella vengono forzati a peso `20`.

Il risultato finale e una media pesata che genera la classifica personalizzata.

## Sorgente dati

I dati arrivano dai repository pubblici di Il Sole 24 Ore, per esempio:

- `https://github.com/IlSole24ORE/QDV2025`
- `https://github.com/IlSole24ORE/QDV2024`

La pagina prova i repository annuali in ordine decrescente e interroga l'indice GitHub del repository per trovare il filename CSV effettivo del dataset pubblicato.

## Struttura del progetto

- [index.html](/Users/oneiros/Projects/qdv/index.html): pagina unica con markup, stile e logica client-side.
- [.github/workflows/deploy-pages.yml](/Users/oneiros/Projects/qdv/.github/workflows/deploy-pages.yml): workflow GitHub Actions per il deploy su GitHub Pages.

## Sviluppo locale

Essendo una pagina statica, puoi aprire direttamente `index.html` nel browser.

Se preferisci servirla in locale con un server HTTP semplice:

```bash
python3 -m http.server 8000
```

Poi apri:

```text
http://localhost:8000
```

## Deploy su GitHub Pages

Il repository include un workflow GitHub Actions che pubblica il contenuto della root del progetto su GitHub Pages a ogni push su `main`.

Passi richiesti su GitHub:

1. Apri le impostazioni del repository.
2. Vai in `Settings > Pages`.
3. In `Build and deployment`, imposta `Source` su `GitHub Actions`.

Nota importante:

- Il workflow puo fare il deploy del sito.
- L'abilitazione iniziale di GitHub Pages spesso richiede pero un'impostazione manuale lato repository.
- Se Pages non e ancora abilitato, GitHub Actions fallisce nello step `configure-pages` con errori come `Get Pages site failed` o `Resource not accessible by integration`.

Una volta attivato correttamente, il sito sara disponibile su:

```text
https://oneiros90.github.io/qdv/
```

## Note tecniche

- Tutta la logica gira nel browser.
- Non ci sono dipendenze di build.
- I dati e le preferenze vengono memorizzati localmente per migliorare l'esperienza d'uso tra un refresh e l'altro.
