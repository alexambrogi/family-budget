# 💰 FamilyBudget

> **WebApp per la gestione delle spese di famiglia** — dark, minimal, mobile-first.

FamilyBudget è una single-page application che permette a una famiglia di tenere traccia delle proprie spese, visualizzare report grafici e confrontare i dati anno per anno. Non richiede installazione: basta aprire `index.html` nel browser.

---

## ✨ Funzionalità

| Sezione | Descrizione |
|---|---|
| **Spese** | Aggiungi, modifica ed elimina spese mensili per categoria e persona |
| **Report** | Grafici a torta, a barre e line chart filtrabili per mese e categoria |
| **Storico** | Confronto anno su anno con grafico a barre impilate |
| **KPI** | Totali mensili e annuali a colpo d'occhio |
| **Admin** | Gestione categorie (colore + emoji), import/export JSON e CSV |

---

## 🏗️ Come funziona

```
Browser (index.html)
       │
       ├─ localStorage   ← dati offline, accesso immediato
       │
       └─ Google Apps Script (GAS)  ← sync con Google Sheets
```

- **Offline-first:** tutti i dati vengono salvati in `localStorage`.
- **Sync remoto:** le operazioni vengono sincronizzate con un backend Google Apps Script (Google Sheets come database).
- **Sessione:** autenticazione con password, valida 24 ore via `sessionStorage`.

---

## 🚀 Come avviare

Nessuna dipendenza da installare.

```bash
# Apri semplicemente il file nel browser
open index.html
```

Tutte le librerie (Bootstrap 5, Chart.js 4, Google Fonts) vengono caricate via CDN.

---

## 🗂️ Struttura del progetto

```
family-budget/
└── index.html   # intera applicazione (HTML + CSS + JS)
```

---

## 🎨 Stack tecnico

- **HTML5 / Vanilla JavaScript** — nessun framework frontend
- **Bootstrap 5.3** — layout responsive e componenti UI
- **Chart.js 4.4** — visualizzazioni grafiche
- **Google Apps Script** — API backend + persistenza su Google Sheets
- **Dark theme** — palette personalizzata con variabili CSS

---

## 📦 Formato dati

Le spese sono salvate come oggetti JSON:

```json
{
  "id": "abc123",
  "mese": 3,
  "tipoId": "cat_01",
  "desc": "Spesa supermercato",
  "importo": 150.00,
  "periodo": "mensile",
  "persona": "Mario",
  "ts": 1710000000000
}
```

Per esportare o importare i dati vai nella sezione **Admin → Esporta / Importa**.
