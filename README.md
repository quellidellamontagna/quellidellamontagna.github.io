# Quelli della Montagna

Diario di gruppo per le cime dell'Appennino sopra i 2000 m (272 vette).

🗺 **Demo live:** [quellidellamontagna.github.io](https://quellidellamontagna.github.io/)

---

## Funzionalità

| Sezione | Descrizione |
|---|---|
| **Mappa** | Tutte le 272 cime su mappa topografica (Esri World Topo Map), con marker colorati in base al numero di alpinisti che le hanno salite (0–5). Filtri per alpinista e gruppo montuoso. |
| **Cime** | Elenco completo delle vette con quota, gruppo e numero di salite registrate. Ricerca e ordinamento per colonna. |
| **Diario** | Uscite raggruppate per persona e data, con conteggio cumulativo delle cime uniche raggiunte. |
| **Alpinisti** | Profilo di ogni alpinista: cime salite, gruppi toccati, prima e ultima uscita. |
| **Confronta** | Confronto fianco a fianco tra due alpinisti: cime in comune, cime esclusive, statistiche. |
| **Statistiche** | Panoramica del gruppo: cime salite, copertura per gruppo, classifica alpinisti. |

## Dati

I dati delle ascensioni vengono letti da un **Google Sheet** pubblico con due fogli:

### Foglio `Alpinisti`

| id | nome |
|---|---|
| 1 | Mario Rossi |
| 2 | Anna Bianchi |

### Foglio `Ascese`

| cima_id | alpinista | data |
|---|---|---|
| 1 | 1 | 2024-08-12 |
| 3 | 2 | 2024-07-19 |

- `cima_id` corrisponde all'ID numerico nell'elenco delle cime (embedded in `cime.js`).
- `alpinista` è l'`id` numerico dal foglio Alpinisti.
- `data` in formato `YYYY-MM-DD`.

Se non viene configurato alcun `spreadsheetId` in `app.js`, l'app usa dati di esempio integrati.

## Configurazione

In testa a `app.js`:

```js
const CONFIG = {
  spreadsheetId: "",   // ID del Google Sheet (la parte tra /d/ e /edit nell'URL)
  sheets: { ascese: "Ascese", alpinisti: "Alpinisti" },
  cacheTtlMs: 60 * 60 * 1000,  // cache sessionStorage: 1 ora
};
```

1. Crea un Google Sheet con i due fogli descritti sopra.
2. Pubblicalo: **File → Condividi → Pubblica sul Web** (oppure rendi il foglio accessibile a "Chiunque abbia il link").
3. Copia l'ID dall'URL e incollalo in `spreadsheetId`.

## Stack tecnico

- **HTML/CSS/JS vanilla** — nessun framework, nessun bundler.
- **[Leaflet](https://leafletjs.com/)** (vendored) per la mappa interattiva.
- **Google Sheets** come database leggero, letto tramite l'endpoint `gviz` (JSON) con fallback JSONP per l'uso da `file://`.
- **`sessionStorage`** per cache dei dati con TTL simulato (1 h).
- Routing SPA con hash (`#/mappa`, `#/cime`, ecc.).

## Sviluppo locale

Apri direttamente `index.html` nel browser — funziona anche da `file://` grazie al fallback JSONP.  
In alternativa, con un server locale:

```sh
# Python
python3 -m http.server 8000

# Node
npx serve .
```

## Licenza

Uso personale / gruppo privato.
