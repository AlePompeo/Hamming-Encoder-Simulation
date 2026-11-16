# 🔢 Codifica/Decodifica Hamming (12, 8) — circuito digitale

> **Hamming (12, 8) encoder/decoder** as a digital circuit in the [Digital](https://github.com/hneemann/Digital) logic simulator: single-error correction, double-error detection, shared XOR networks. University project (Computer Architecture course), Italian documentation.

![Digital](https://img.shields.io/badge/Simulator-Digital-1f6feb) ![Hamming](https://img.shields.io/badge/Code-Hamming%20(12%2C8)-8a2be2) ![License](https://img.shields.io/badge/License-MPL--2.0-orange)


Progetto universitario per il corso di **Calcolatori Elettronici** — progettazione e realizzazione di un circuito digitale per la **codifica e decodifica del Codice di Hamming (12, 8)** con distanza minima di Hamming pari a 3, realizzato con il simulatore [Digital](https://github.com/hneemann/Digital).

<img width="852" height="904" alt="image" src="https://github.com/user-attachments/assets/20ab08b4-a925-4c0c-af43-ce498b152e0c" />

---

## 📌 Descrizione

Il circuito implementa un encoder/decoder per il **Codice di Hamming (12, 8)**: prende in ingresso una parola a 8 bit di dati e 4 bit di parità, e opera in due modalità selezionabili tramite il segnale di controllo `d`:

- **Codifica** (`d=0`): calcola i bit di parità e produce la parola codificata a 12 bit nelle posizioni corrette dello schema di Hamming.
- **Decodifica** (`d=1`): verifica la sindrome di errore, segnala la presenza di errori tramite il bit `e`, e **corregge automaticamente** eventuali errori singoli invertendo il bit errato.

---

## 🧠 Struttura del Codice di Hamming (12, 8)

La parola codificata segue il posizionamento standard, dove i bit di parità occupano le posizioni che sono potenze di 2:

| Posizione | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   |
|-----------|------|------|------|------|------|------|------|------|------|------|------|------|
| Tipo      | `p1` | `p2` | `d1` | `p3` | `d2` | `d3` | `d4` | `p4` | `d5` | `d6` | `d7` | `d8` |

Ogni bit di parità copre le posizioni il cui indice binario ha un `1` nel bit corrispondente:

| Parità | Posizioni coperte   |
|--------|---------------------|
| `p1`   | 1, 3, 5, 7, 9, 11   |
| `p2`   | 2, 3, 6, 7, 10, 11  |
| `p3`   | 4, 5, 6, 7, 12      |
| `p4`   | 8, 9, 10, 11, 12    |

---

## ⚙️ Interfaccia del Circuito

### Ingressi (13 bit)

| Segnale | Descrizione |
|---------|-------------|
| `d`     | Modalità: `0` = codifica, `1` = decodifica |
| `p1`, `p2`, `p3`, `p4` | Bit di parità (don't care in codifica) |
| `d1` … `d8` | Bit di dati |

### Uscite (13 bit)

| Modalità | `y1`–`y12` | `e` |
|----------|------------|-----|
| Codifica (`d=0`) | Parola codificata a 12 bit | `0` (forzato) |
| Decodifica (`d=1`) | `d1`–`d8` corretti nelle posizioni `y1`–`y8`, `y9`–`y12` = `0` | `0` = nessun errore, `1` = errore rilevato |

---

## 🔧 Architettura del Circuito

Il circuito è composto da tre blocchi funzionali principali:

### 1. Calcolo dei bit di parità (encoder)
Quattro reti di porte **XOR** calcolano i bit di parità `p1`–`p4` in funzione dei bit di dati `d1`–`d8`, seguendo la copertura definita dallo schema di Hamming.

### 2. Calcolo della sindrome (decoder)
Quattro **XOR** a più ingressi confrontano i bit di parità ricevuti con quelli ricalcolati sulla parola in ingresso, producendo una sindrome a 4 bit. Se la sindrome è `0000`, la parola è corretta; altrimenti il suo valore indica l'indice del bit errato.

### 3. Correzione dell'errore
Un **decoder 4→16** converte la sindrome nell'indice del bit errato. Ogni bit della parola viene passato attraverso un **multiplexer** che, se selezionato dalla sindrome, lo inverte tramite una porta **NOT**, correggendo l'errore singolo.

### 4. Selezione modalità
Un banco di **multiplexer** controllato dal segnale `d` smista le uscite tra la parola codificata (modalità encoder) e i soli bit di dati corretti (modalità decoder), forzando a zero le uscite non necessarie.

---

## 🛠️ Come aprire e simulare il circuito

### Requisiti
- [Java Runtime Environment (JRE)](https://www.java.com/)
- [Digital — Logic Circuit Simulator](https://github.com/hneemann/Digital/releases)

### Apertura
1. Scaricare e avviare `Digital.jar`
2. Aprire il file `circuito.dig` tramite **File → Apri**

### Simulazione
- Avviare la simulazione con **Simulazione → Avvia**
- Modificare gli ingressi e osservare le uscite in tempo reale
- Eseguire i test automatici con **Analisi → Test**

---

## 📁 Struttura del Repository

```
.
└── circuito.dig    # Circuito Digital con encoder/decoder Hamming (12,8)
```

---

## ✅ Funzionalità Implementate

- Codifica corretta di qualsiasi parola a 8 bit in Hamming (12, 8)
- Decodifica corretta di parole senza errori
- Rilevamento di errori singoli e doppi tramite sindrome
- Correzione automatica di errori singoli
- Critical path minimizzato tramite calcolo parallelo della sindrome
- Numero di componenti ridotto con condivisione delle reti XOR tra encoder e decoder

---

## 📄 Licenza

Mozilla Public License 2.0, vedi [`LICENSE`](LICENSE).
---

*Keywords: Hamming code, Hamming (12,8), error correction, ECC, digital logic, Digital simulator, encoder, decoder, computer architecture, university project.*
