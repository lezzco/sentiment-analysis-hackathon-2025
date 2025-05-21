# 🧠 Hackathon Big Data & AI  
## Challenge Ufficiale 🚀  
**"F1 Social Analytics Engine: estrazione, integrazione e sentiment analysis dal GP di Monaco 2025"**

---
## Indice

- [Obiettivo 🏁](#obiettivo-)
- [📥 Fase 1 — Data Ingestion and Dataset Building](#fase-1---data-ingestion-and-dataset-building)
    - [1.1 Schema dataset finale](#1-.-1-schema-dataset-finale)
- [📥 Fase 2 — Sentiment Analysis](#fase-2---sentiment-analysis)
    - [🎯 Obiettivo della parte di Sentiment Analysis](#obiettivo-della-parte-di-sentiment-analysis)
    - [✨ Bonus: Estensione LLM-based - Creazione automatica dei report](#bonus-estensione-llm-based---generatione-automatica-dei-report)
- [📦 Cosa Consegnare](#cosa-consegnare)
- [📜 Metriche di Valutazione e Punteggi](#-metriche-di-valutazione-e-punteggi)
- [1️⃣ Inizializzazione progetto](#1-inizializzazione-progetto)
- [🕒 Timeline](#-timeline)

---

## Obiettivo 🏁 

In occasione del Gran Premio di Monaco 2025, uno degli eventi più iconici e seguiti della Formula 1, il vostro compito è progettare e realizzare un sistema intelligente che recupera, integra e analizza post e commenti provenienti dai social media, per comprendere come gli utenti vivono l’evento prima, durante e dopo la gara, con un focus particolare sul sentiment e sulla geolocalizzazione.

L'obiettivo è cogliere le emozioni, le opinioni e le reazioni degli appassionati di motori sparsi in tutto il mondo, offrendo un quadro dinamico e aggiornato in tempo reale su come il pubblico percepisce i piloti, le scuderie, i momenti salienti e gli episodi controversi della gara.

Questa sfida ha due anime principali:

- **Data ingestion & dataset building (fondamentale!)**
- **Sentiment analysis avanzata (con o senza LLM)**

---


## Fase 1 - Data Ingestion and Dataset Building

Progettare e implementare una pipeline di data ingestion che:

- 🔍 Recupera in modo automatico (o semi-automatico) contenuti testuali da diversi social media (es. Instagram, Facebook, Twitter/X, Reddit, YouTube…).

- 🧹 Pulisce, filtra e normalizza i dati testuali, le interazioni e i metadati.

- 📦 Produce un dataset finale conforme a uno schema comune condiviso da tutti i gruppi (vedi sotto).

### NOTE:
- Non devono essere inclusi dati personali identificabili  
- Non violare i ToS dei social: scraping etico o dataset già disponibili

---

### 1.1 Schema dataset finale:

| Feature           | Descrizione                                                                 | Datatype             | Nullable | Note                                        | Esempio                  |
|-------------------|-----------------------------------------------------------------------------|----------------------|----------|---------------------------------------------|--------------------------|
| content_id        | ID univoco del post o commento                                              | stringa              | No       | es. ID nativo social o hash generato        | fb_post_001              |
| observation_time  | Timestamp di un’osservazione di uno specifico contenuto                     | datetime (ISO 8601)  | No       | formato: YYYY-MM-DDTHH:MM:SSZ                | 2025-05-15T10:00:00Z     |
| user              | ID o nome utente (anonimizzato se necessario)                              | stringa              | No       |                                             | giulia_snap              |
| user_location     | Città/Stato indicati nel profilo dell’utente (se presente)                  | stringa              | Si       |                                             | Nizza                    |
| social_media      | Nome della piattaforma (es. Twitter, Reddit, etc.)                          | stringa              | No       |                                             | Instagram                |
| publish_date      | Timestamp UTC del post/commento                                             | datetime (ISO 8601)  | No       | formato: YYYY-MM-DDTHH:MM:SSZ                | 2025-05-25T19:45:10Z     |
| geo_location      | Coordinate GPS (latitudine, longitudine) del luogo dell’interazione         | tuple(float, float)  | Si       |                                             | (45.4642, 9.1900)        |
| comment_raw_text  | Testo originale del contenuto                                               | stringa              | No       | Se non c’è testo, mettere stringa vuota ""  | Corsa Pazzesca 😬🏎️       |
| emoji             | Emoji utilizzate nel testo                                                  | Lista[stringa]       | Si       |                                             | 😬🏎️                      |
| reference_post_url| Link al post originale (se è una risposta/commento)                        | stringa              | Si       | Deve essere un URL                           | facebook.com/post/123456789 |
| like_count        | Numero di like/upvote                                                       | Intero               | No       | Default 0, non può essere negativo          | 112                      |
| reply_count       | Numero di commenti/risposte ricevute                                       | Intero               | No       | Default 0, non può essere negativo          | 17                       |
| repost_count      | Numero di condivisioni/retweet                                             | Intero               | No       | Default 0, non può essere negativo          | 1                        |
| quote_count       | Quote tweet o repost con commento (se disponibile)                         | Intero               | No       | Default 0, non può essere negativo          | 0                        |
| bookmark_count    | Numero di salvataggi (se disponibile)                                     | Intero               | No       | Default 0, non può essere negativo          | 0                        |
| content_type      | Tipo di contenuto analizzato                                               | stringa              | No       | Valore categorico: "post" o "commento"      | post                     |

---

## Fase 2 - Sentiment Analysis

Una volta creato un dataset coerente, la sfida si sposta sull'analisi del sentiment. Ecco cosa includere, come strutturare la consegna e cosa valutare.

### Obiettivo della parte di Sentiment Analysis

Analizzare i contenuti raccolti (post/commenti) per comprendere come evolve l’umore e la percezione degli utenti nel tempo e nello spazio in relazione al Gran Premio di Monaco 2025, con un focus sulle fasi prima, durante e dopo la gara.

L’analisi può mettere in luce emozioni, attese, reazioni e controversie legate ai piloti, ai team e possibili correlazioni con gli eventi chiave della gara.

---

### ⚙️ Approcci suggeriti

#### 1. Approccio classico (NLP tradizionale)

Utilizzo di tecniche di Natural Language Processing tradizionali, quali:  
- Pre-elaborazione del testo (tokenizzazione, rimozione stopwords, stemming)  
- Rappresentazione con tecniche tipo TF-IDF  
- Classificatori supervisionati (SVM, Naive Bayes, Random Forest)  
- Analizzatori di sentiment già pronti come TextBlob o VAD  

#### 2. Approccio con LLM (Large Language Models)

Sfruttare modelli linguistici di nuova generazione (GPT, LLaMA, Claude, Mistral, ecc.) per:  
- Classificare direttamente il sentiment di ogni contenuto  
- Estendere l’analisi con rilevazione di emozioni, argomenti principali, intensità del sentimento  
- Arricchire dati mancanti (es. dedurre location, riconoscere sarcasmo, ecc.)  

Questi modelli possono essere interrogati tramite prompt ben costruiti per analisi dettagliate su testi brevi, rumorosi o ambigui.

---

### Bonus: Estensione LLM-based - Generatione automatica dei report

Oltre alla classificazione del sentiment, è incoraggiato l’uso di LLM e modelli multimodali per:  

- 📊 Generare automaticamente grafici o visualizzazioni dai dati analizzati  
- 🧠 Riassumere in linguaggio naturale i risultati (es. "Nei commenti dalla Francia, il sentiment era positivo prima della gara, ma è calato dopo l’incidente al 35° giro")  

Esempi di strumenti/approcci:  
- GPT-4-Vision, Gemini, Claude 3 per generare visualizzazioni via prompt  
- Plotly + LLM per generare codice Python per grafici  
- Diagrammi, mappe e infografiche sintetiche generate da modelli come DALL·E o Midjourney 


**⚠️ Questa parte è opzionale ma valorizzata nella valutazione, dimostrando originalità e padronanza delle tecnologie LLM.**

---

## Cosa Consegnare

Lista degli artefatti da consegnare:

- Codice sorgente su Repository GitHub (link da condividere con gli organizzatori prima della scadenza)
- Dataset generato dall’ingestion (**)
- Report PDF con risultati analisi del sentiment dell’evento (inclusi eventuali grafici Bonus) all’interno della repository GitHub sotto la folder reports

 📥 **Consegna dei dataset post-ingestion**

Se il dataset finale supera i limiti di GitHub (es. >80MB per file o >1GB totali) sarà necessario utilizzare il link che vi verrò passato **privatavamente** al quale caricare il file.

---

## 1️⃣ Inizializzazione progetto

Ogni team deve creare una propria repository GitHub a partire da un template ufficiale fornito dall’organizzazione. Tutti i team lavoreranno sulla stessa struttura di base per garantire ordine, coerenza e facilità di valutazione.

### 🚀 Come iniziare

Installare copier (solo la prima volta):

```bash
pip install copier
```




## 📜 Metriche di Valutazione e Punteggi

La vostra soluzione verrà valutata secondo cinque macro-criteri. Ogni aspetto tiene conto non solo della qualità tecnica, ma anche della creatività, dell’efficacia dell’analisi e della chiarezza nella comunicazione dei risultati. Di seguito la griglia di valutazione dettagliata:

| Macro-metrica                        | Punteggio massimo | Dettaglio                                                                 |
|-------------------------------------|-------------------|--------------------------------------------------------------------------|
| 1. Qualità della soluzione          | 30 pts            | Codice pulito, automazione, modularità, riusabilità                      |
| 2. Creatività della soluzione       | 20 pts            | Idee originali, approcci inediti, uso creativo di tool/LLM              |
| 3. Completezza del dato             | 20 pts            | Aderenza allo schema, qualità e copertura dei dati                      |
| 4. Efficacia dell’analisi di sentiment | 15 pts         | Accuratezza, rilevanza, originalità delle intuizioni                    |
| 5. Output e comunicazione (report, grafici, doc) | 15 pts | Chiarezza, presentazione, comprensibilità per non esperti               |


## 🕒 Timeline

| Evento                            | Data e Ora                      |
|----------------------------------|----------------------------------|
| ⏳ **Chiusura iscrizioni**        | 🗓️ Venerdì 23 maggio, ore 20:45     |
| 📤 **Consegna progetto**          | 🗓️ Lunedì 26 maggio, ore 21:30      |
| 🎤 **Presentazione risultati**    | 🗓️ Martedì 27 maggio, ore 08:30      |

