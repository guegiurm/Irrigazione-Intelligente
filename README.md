🌿 Progetto di Irrigazione Smart – Home Assistant + Node-RED + MQTT + DB
Sistema completo per l'automazione dell'irrigazione basato su:

Controllo intelligente con condizioni meteo e ambientali

UI evoluta con Lovelace e popup dinamici

Integrazione database per log e analisi

Raccolta dati storici per futuri algoritmi di machine learning

🧰 Stack Tecnologico
Componente	Funzione
Home Assistant	Interfaccia domotica e controllo logico
Node-RED	Automazioni e logica dati
MQTT	Bridge tra HA e Node-RED (dati meteo)
MariaDB/MySQL	Persistenza dati meteo/stato
HACS	UI personalizzate (cards Lovelace)

🔗 Dipendenze & Custom Cards
Installa tramite HACS:

swipe-card: swipe tra zone

button-card: pulsanti dinamici

timer-bar-card: visualizzazione countdown

mini-graph-card: dati meteo storici

bubble-card: popup responsive

Broker MQTT attivo (addon Mosquitto o esterno) necessario.

📂 Struttura del Progetto
📁 irrigation_helpers.yaml (Home Assistant)
Contiene tutto il necessario per la logica lato HA:

input_boolean per attivazione irrigazione, bypass, slot

input_number per soglie di temperatura, pioggia, durata

input_select per programmazione irrigazione (giorni/settimana)

input_text per log e prossima irrigazione

input_datetime per orari irrigazione slot

mqtt sensor:

sensor.pioggia_ultime_6_ore

sensor.pioggia_ultimi_6_giorni

history_stats sensor: numero irrigazioni nella settimana

timer: irrigazione in corso

template: calcolo tempo residuo

📁 card_irrigation.yaml (UI Lovelace)
Card principale:

custom:swipe-card: due zone irrigazione

custom:button-card: navigazione + log

timer-bar-card: appare se irrigazione in corso

Doppio tap per popup di zona

📁 card_irrigation_popup.yaml (UI Popup Dettaglio)
Popup avanzato per ogni zona:

Stato sensori, log, soglie

Grafici pioggia/temperatura/umidità

Visualizzazione pianificazione

Timer dinamico se attivo

📁 flusso.json (Node-RED)
Flusso completo che gestisce:

1. 📡 Recupero Dati Meteo (API esterne)
Pioggia ultime 6h

Pioggia ultimi 6gg

Temperatura, umidità media

2. 📤 Pubblicazione su MQTT
meteo/pioggia_6h

meteo/pioggia_6gg

3. 🗃️ Salvataggio su database (MariaDB/MySQL)
Tabella irrigation_data

Colonne:

timestamp

pioggia_6h

pioggia_6gg

temperatura

umidita

zona

durata_irrigazione

Query SQL automatiche per scrivere dati ogni 6 ore

4. 📊 Raccolta Dati Annuali
Aggregazione dati mensili/annuali

Costruzione dataset per algoritmi ML futuri (es. irrigazione predittiva)

🚀 Installazione
1. Includi irrigation_helpers.yaml in configuration.yaml
yaml
Copia
Modifica
homeassistant:
  packages: !include_dir_named packages
📁 Assicurati che il file sia in config/packages/

2. Importa i Flow in Node-RED
Vai su Node-RED

Menu → Importa → flusso.json

Associa eventuali nodi MQTT, MariaDB e timer

Salva & deploy

3. Aggiungi le Card Lovelace
Importa:

card_irrigation.yaml nella tua dashboard principale

card_irrigation_popup.yaml nelle viste #zona1, #zona2, ecc.

4. Assicurati che i seguenti servizi siano configurati:
Componente	Stato richiesto
MQTT broker	Installato + operativo
MariaDB	Accesso e credenziali validi
HACS	Installato con cards
timer.*	Attivati da automazioni o UI

📈 Esempi d’Uso
🌧 Stop irrigazione se pioggia > soglia

🌡 Irrigazione attivata solo se temperatura > soglia min

📅 Programmazione settimanale per zona

🧠 Dataset annuale per irrigazione ottimizzata

🧾 Log completo in input_text.irrigation_log (visibile in card)

🧪 Debug & Manutenzione
Verifica valori MQTT da Developer Tools → Entità

Log SQL nel DB: usa tool tipo phpMyAdmin o SELECT * FROM irrigation_data

Controlla che timer.irrigazione_* sia attivo per vedere barra

💡 Estensioni Future

Dashboard storiche Grafana via InfluxDB

Irrigazione predittiva con ML (es. scikit-learn)

Notifiche Telegram/Push in base a stato sistema

## 🧪 Debug

Ogni nodo ha indicatori di stato per aiutare nella diagnostica. Le motivazioni per lo skip vengono sempre loggate e possono essere consultate nel pannello.

## 📄 Licenza

MIT License — Open to contribution!

🧡 Supporta il progetto
Se questo progetto ti è utile e vuoi supportarne lo sviluppo, puoi offrirmi un caffè ☕ o fare una piccola donazione. Ogni contributo è molto apprezzato!

<p align="left"> <a href="https://www.paypal.me/guegiurm" target="_blank"> <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="Donate via PayPal"> </a> <a href="https://coff.ee/guegiurm" target="_blank"> <img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" height="41"> </a> </p>
