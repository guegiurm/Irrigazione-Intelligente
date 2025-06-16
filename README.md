# 🌱 Irrigazione Intelligente con Node-RED e Home Assistant

Sistema avanzato di irrigazione automatica basato su **Node-RED** e **Home Assistant**, che utilizza sensori, previsioni meteo via API, soglie dinamiche e automazioni per ottimizzare il consumo d'acqua e la salute delle piante.

## 🚀 Caratteristiche principali

- **Gestione multi-zona**: ogni zona è gestita tramite subflow indipendenti (es. `switch.centralina_irrigazione`)
- **Integrazione sensori**:
  - Umidità del suolo (`sensor.*`)
  - Temperatura e umidità atmosferica (`Open-Meteo API`)
- **Previsioni meteo e storico pioggia**:
  - Ultimi 5 giorni + prossimi 3
  - Calcolo automatico della pioggia nelle prossime 6 ore
- **Controllo intelligente**:
  - Valuta soglie dinamiche configurabili: temperatura, umidità suolo, pioggia
  - Salta irrigazioni in caso di pioggia eccessiva o suolo già umido
- **Automazioni configurabili**:
  - Fino a 3 slot orari per zona
  - Possibilità di forzare manualmente l'irrigazione
  - Skip temporanei automatici
- **Pannello Home Assistant**:
  - Interfaccia utente con slider, toggle e log
  - Mostra la prossima irrigazione prevista

## 🛠 Setup

### 1. Importazione dei flussi

Importa il file `flusso.json` direttamente in Node-RED. Ogni zona è rappresentata da un **subflow** con i relativi nodi per la logica di controllo, sensori e attivazione.

### 2. Helpers in Home Assistant

Importa il file `irrigation_helpers.yaml` in Home Assistant, che include:

- `input_boolean.irrigation_enable`, `input_boolean.force_irrigation`
- `input_number.irrigation_duration_zone1`, `input_number.irrigation_soil_threshold`, `irrigation_temp_min`, `irrigation_temp_max`, ecc.
- `input_datetime.irrigation_slot1_time`
- `input_select.irrigation_slot1_days`
- `input_text.irrigation_log`, `irrigation_next_run`

Questi helper sono fondamentali per controllare logica, soglie e schedule.

### 3. Pannello Lovelace

Il file `card_irrigation.yaml` contiene una dashboard Lovelace pronta, che include:

- **Custom Cards richieste**:
  - [`button-card`](https://github.com/custom-cards/button-card)
  - [`auto-entities`](https://github.com/thomasloven/lovelace-auto-entities)
  - [`card-mod`](https://github.com/thomasloven/lovelace-card-mod)
  - [`slider-entity-row`](https://github.com/thomasloven/lovelace-slider-entity-row)

Per installarli puoi usare HACS (Home Assistant Community Store).

### 4. Dipendenze Node-RED

Assicurati di avere installati i seguenti nodi:

- `node-red-contrib-home-assistant-websocket`
- `node-red-contrib-cron-plus`
- `node-red-node-random` (se usi delay casuali)
- `node-red-node-fetch` (o `http request`)

## 📊 Come funziona

1. Un trigger `cron-plus` valuta se lo slot è attivo.
2. Il subflow acquisisce:
   - Previsioni meteo (pioggia, temperatura, umidità)
   - Umidità del terreno (se abilitato)
3. Vengono calcolate:
   - Medie giornaliere
   - Pioggia cumulata (6h e 6 giorni)
4. Il sistema decide:
   - Irrigare con logica avanzata (basata su soglie e meteo)
   - Saltare l'irrigazione con motivazione salvata nel log

## 📱 Notifiche e Log

- **Notifiche push** in caso di avvio irrigazione
- **Log skip o avvio** salvati in `input_text.irrigation_log`
- **Prossima irrigazione** visibile in `input_text.irrigation_next_run`

## 🌾 Sviluppi futuri

- Integrare suggerimenti su concimi o piante (in sviluppo)
- Estendere le API a più fonti meteo (es. DarkSky, WeatherBit) (in sviluppo)

## 📷 Screenshot

![UI Panel](./assets/ui-panel.png) <!-- Inserisci screenshot reale se disponibile -->

---

## 🧪 Debug

Ogni nodo ha indicatori di stato per aiutare nella diagnostica. Le motivazioni per lo skip vengono sempre loggate e possono essere consultate nel pannello.

## 📄 Licenza

MIT License — Open to contribution!

🧡 Supporta il progetto
Se questo progetto ti è utile e vuoi supportarne lo sviluppo, puoi offrirmi un caffè ☕ o fare una piccola donazione. Ogni contributo è molto apprezzato!

<p align="left"> <a href="https://www.paypal.me/guegiurm" target="_blank"> <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="Donate via PayPal"> </a> <a href="https://coff.ee/guegiurm" target="_blank"> <img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" height="41"> </a> </p>
