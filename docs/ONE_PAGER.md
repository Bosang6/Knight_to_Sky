# Knight to Sky

Platformer 2D infinito sviluppato in **C++17 con raylib**: guida un cavaliere tra piattaforme che scorrono verso il basso, sfrutta il doppio salto e raccogli bonus per aumentare il punteggio. Ogni partita propone nuove disposizioni di piattaforme e oggetti, con difficoltà costante e senza un traguardo finale.

## Come si gioca

La partita comincia a mezz'aria: il primo salto attiva lo scorrimento e il punteggio legato alla sopravvivenza. Osserva gli appoggi, correggi la traiettoria anche in volo e scegli quando deviare per raccogliere un oggetto. Atterrare ripristina entrambi i salti; il secondo può prolungare la salita o interrompere una caduta. Uscendo completamente da un bordo laterale ricompari dal lato opposto.

Puoi attraversare le piattaforme dal basso e atterrarci durante la discesa. Le piattaforme **normali** offrono un appoggio, quelle **mobili** si spostano orizzontalmente e quelle **fragili** svaniscono dopo il contatto, una volta avviato lo scorrimento. Nuove piattaforme vengono generate mentre quelle precedenti escono dall'area di gioco.

Quando precipiti sotto lo schermo la partita termina e viene mostrato il punteggio finale. Premi Invio per tornare alla schermata iniziale, poi nuovamente Invio per ripartire.

## Comandi

| Tasto | Azione |
|---|---|
| A / D oppure frecce sinistra / destra | Movimento orizzontale |
| Spazio | Salto, fino a due prima di atterrare |
| Invio | Avvio dalla schermata iniziale; ritorno al menu dopo il game over |

## Punteggio e oggetti

La sopravvivenza assegna **10 punti al secondo** dal primo salto. Gli oggetti raccolti possono aiutarti oppure rendere più difficile il controllo del cavaliere.

| Oggetto | Effetto |
|---|---|
| Moneta | +100 punti |
| Moltiplicatore | Punti da tempo e monete ×2 per 15 secondi |
| Molla | Velocità iniziale verso l'alto del prossimo salto aumentata del 60%; si consuma all'uso |
| Rallentamento | Velocità orizzontale ridotta al 60% per 12 secondi |
| Inversione | Comandi sinistra e destra scambiati per 5 secondi |

Effetti temporanei diversi possono coesistere. Raccogliere nuovamente lo stesso effetto ne ripristina la durata senza aumentarne l'intensità. L'interfaccia mostra punteggio, durate residue e messaggi di raccolta.

## Compilazione e avvio

Servono **CMake 3.20 o superiore**, un compilatore compatibile con C++17, **Ninja** per i comandi seguenti e un'installazione di **raylib** compatibile con il compilatore. Esegui dalla cartella principale della repository:

```powershell
cmake -S . -B build -G Ninja -DRAYLIB_DIR="C:/raylib/w64devkit/x86_64-w64-mingw32"
cmake --build build
cd build
.\cpp-raylib-platformer.exe
```

L'esempio è per Windows: adatta il percorso di raylib alla tua installazione e rendi disponibili gli strumenti nel PATH. Se CMake trova già raylib, puoi omettere `-DRAYLIB_DIR=...`. La compilazione copia gli asset accanto all'eseguibile: avvia il gioco da quella cartella. Su altri sistemi il nome dell'eseguibile e la configurazione degli strumenti possono variare.

## Stato e limiti

**Implementato nel codice:** movimento, generazione continua, tre tipi di piattaforme, cinque oggetti, punteggio, schermate iniziale e finale, animazioni ed effetti audio.

**Da verificare in gioco:** compilazione sulle diverse piattaforme, collisioni, raggiungibilità dei percorsi e resa audiovisiva. Il caricamento della musica usa un percorso con maiuscole diverse dalla cartella presente; sono inoltre ancora disegnati indicatori di debug. Titolo finestra e schermata iniziale differiscono dal titolo del progetto.

**Pianificato:** nessuna funzionalità futura confermata. Analisi statica e discrepanze sono documentate nel registro delle evidenze.
