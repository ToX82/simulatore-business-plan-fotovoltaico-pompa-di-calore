# Simulatore Fotovoltaico + Accumulo

Business plan interattivo a 25 anni per impianti fotovoltaici domestici con sistema di accumulo e pompa di calore. Calcola payback, ROI, flussi di cassa annuali e cumulati a partire da pochi parametri.

Tutto in **un singolo file HTML**, senza build, senza dipendenze, senza server. Apri e usa.

## Caratteristiche

- **Distribuzione automatica** della produzione tra autoconsumo istantaneo, carica batteria e immissione in rete (RID), simulata giorno per giorno su un profilo orario annuale di riferimento. Override manuale tramite slider.
- **Modello di degrado** separato per pannelli (lineare) e batteria (a tre nodi: anno 10, 20, 25).
- **Inflazione** indipendente per il prezzo dell'energia in acquisto e per il valore RID.
- **Incentivi**: detrazione fiscale 50% pluriennale, Conto Termico una-tantum, incentivo Conto Energia revamping (opzionale).
- **Finanziamento** con rata calcolata o inserita manualmente.
- **Manutenzione straordinaria** (es. sostituzione inverter) parametrizzabile.
- **KPI**: payback con e senza finanziamento, vantaggio netto cumulato a 25 anni, rendimento medio annuo.
- **Grafico flusso di cassa** (cumulato e annuale) e **piano analitico** anno per anno.
- **Salvataggio simulazioni** in `localStorage` + esportazione/importazione JSON per condividere scenari.
- **Mobile-ready**: bottom navigation con swipe tra Risultati / Parametri / Piano, bottom sheet per le simulazioni salvate, touch target ≥44px.

## Uso

```bash
# clone e apri
open business-plan.html         # macOS
xdg-open business-plan.html     # Linux
start business-plan.html        # Windows
```

Oppure servilo localmente:

```bash
python3 -m http.server 8000
# poi http://localhost:8000/business-plan.html
```

## Cosa simula

Per ogni anno fra 1 e 25 il modello calcola:

| Voce | Criterio |
|---|---|
| Produzione FV | `prod₁ × (1 − degrado)^(n−1)` |
| Autoconsumo istantaneo | quota di FV consumata in tempo reale (diurna) |
| Autoconsumo batteria | quota caricata di giorno e usata di sera/notte, decurtata dal degrado batteria |
| RID | eccedenza immessa in rete, valorizzata al prezzo PUN-like |
| Risparmio GPL | costo combustibile fossile evitato dalla pompa di calore |
| Costo EE residuo | kWh mancanti acquistati dalla rete al prezzo retail |
| Detrazioni | rate annuali fisse della detrazione 50% |
| Conto Termico | una-tantum nell'anno 1 |
| Manutenzione | uscita una-tantum nell'anno specificato |
| Rata finanziamento | costante per la durata del piano (se attivo) |

Tutti i prezzi crescono con l'inflazione configurata (energia EE e RID separate).

## Distribuzione automatica della produzione

Il toggle "Calcolo automatico da profilo orario" simula 364 giorni del profilo annuale di riferimento, riscalando produzione e consumo sui valori dell'utente. Ogni giorno viene incrociato:

1. quanta produzione coincide temporalmente con i consumi diurni (autoconsumo istantaneo)
2. l'eccedenza disponibile per la batteria, limitata dalla capacità nominale
3. il residuo che va in rete come RID

Le percentuali risultanti popolano gli slider; disattivare il toggle permette di forzare manualmente la ripartizione.

## Salvataggio e condivisione

- **Salva**: dai un nome alla simulazione, viene memorizzata in `localStorage`.
- **Carica**: clicca un chip o digita il nome.
- **Esporta JSON**: scarica tutti gli scenari salvati in un file portatile.
- **Importa JSON**: ricarica scenari da file (con gestione conflitti: sovrascrivi o rinomina).

## Stack

Vanilla HTML + CSS + JavaScript. Nessuna libreria, nessun framework, nessun build step. Font Inter e JetBrains Mono via Google Fonts. Grafico disegnato con Canvas API.

## Limitazioni

- I valori di default sono indicativi: vanno ricalibrati sulla propria utenza, geografia e contratto.
- La stima di produzione FV non sostituisce un report PVGIS o quello dell'installatore.
- Il modello non include: variazione di prezzo dell'autoconsumo per fascia oraria, accise specifiche, oneri di rete dettagliati, eventi straordinari (guasti, sostituzione batteria).
- Non è una proiezione finanziaria certificata: usalo come strumento di confronto fra scenari, non come consulenza.

## Licenza

MIT — usa, modifica, condividi.
