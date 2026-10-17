# Evidenze — Knight to Sky

## Metodo e limiti

Analisi statica di sorgenti, configurazione CMake, README e inventario degli asset, con conferme del proprietario raccolte in conversazione. Nessuna compilazione, esecuzione o sessione di gioco effettuata. “Implementato” indica logica presente e collegata nel codice, non verifica del comportamento in gioco. AGENTS.md e docs/DESIGN_BRIEF.md non risultano presenti nel checkout; sono state applicate le istruzioni AGENTS.md fornite in chat.

ONE_PAGER.md è stato adattato per il README GitHub e successivamente tradotto in inglese su richiesta esplicita del 17 ottobre 2026. La lunghezza è verificata come testo Markdown; non è stata verificata un'impaginazione effettiva su una pagina.

## Conferme fornite dall'utente

- Titolo: **Knight to Sky**. “JUMP!” indica l'azione, non il titolo.
- Gioco volutamente infinito, senza vittoria finale; difficoltà costante.
- Partenza a mezz'aria e attivazione dello scorrimento con il primo salto intenzionali.
- Valori attuali approvati.
- Flusso intenzionale: game over → Enter → schermata iniziale → Enter → nuova partita.
- Contatori di salti e velocità da considerare assenti nella presentazione richiesta.
- Contributo personale dichiarato: “Designer e implementazione del character e degli assets”. Non sono stati confermati la creazione originale degli asset, specifiche decisioni di design individuali o la paternità degli altri sistemi.

Queste informazioni costituiscono conferme di design e attribuzione, non prove di esecuzione. Non sono state confermate ulteriori funzionalità pianificate.

## Sistemi e riferimenti tecnici

I percorsi sono relativi a questo documento. I dettagli numerici descrivono i valori del codice esaminato.

| Sistema | Evidenza e simboli | Stato |
|---|---|---|
| Tecnologia | [CMakeLists.txt](../CMakeLists.txt): progetto C/C++, standard C++17, target eseguibile, collegamento raylib, copia asset. [main.cpp](../src/main.cpp): main avvia Game::Start. | Implementato; portabilità non verificata in esecuzione. |
| Flusso di partita | [Game.cpp](../src/Scene/Game.cpp): Game::Start e ChangeScene collegano Begin, Game, End. [BeginScene.cpp](../src/Scene/BeginScene.cpp): BeginScene::Update disegna “JUMP!” e invito a premere Enter. [EndScene.cpp](../src/Scene/EndScene.cpp): EndScene::Update mostra game over e punteggio finale. | Implementato; flusso confermato dall'utente. |
| Apertura | [GameScene.cpp](../src/Scene/GameScene.cpp): GameInit posiziona il personaggio al centro, non a terra, con velocità zero; azzera scrolling e punteggio. La gravità agisce subito. | Implementato; intenzione confermata. |
| Controlli e wrap | [Playable_character.cpp](../src/Character/Playable_character.cpp): PlayableCharacter::HandleInput legge A/D e frecce; Update sposta il personaggio al lato opposto dopo l'uscita completa. [Character.cpp](../src/Character/Character.cpp): Character::Update applica velocità e gravità, poi azzera la velocità orizzontale. | Implementato. |
| Doppio salto | [Playable_character.cpp](../src/Character/Playable_character.cpp): costruttore imposta due salti e forza -500; Jump sostituisce la velocità verticale e consuma un salto; OnLandOnPlatform ripristina due salti. | Implementato. |
| Attivazione e scrolling | PlayableCharacter::HandleInput, nello stesso file, imposta Platform::moveDownVelocity = 100, Pickup::moveDownVelocity = 125 e GameScene::gameStart al primo Space. [Platform.cpp](../src/World/Platform.cpp): UpdatePosition; [Pickup.cpp](../src/Pickups/Pickup.cpp): Update. | Implementato; valori e attivazione confermati. |
| Atterraggio | [GameScene.cpp](../src/Scene/GameScene.cpp): HandleCollisions accetta sovrapposizione durante discesa con posizione precedente sopra la superficie; ignora piattaforme rotte, riposiziona il personaggio e ripristina i salti. | Implementato; affidabilità collisioni da verificare in gioco. |
| Generazione continua | [PlatformsManager.cpp](../src/World/PlatformsManager.cpp): GeneratePlatforms, GenerateOne, UpdatePlatformsPosition, CheckDelete generano e rimuovono piattaforme. GenerateOne impedisce piattaforme breakable consecutive. [PlatformsManager.h](../src/World/PlatformsManager.h): intervallo verticale 80–circa 114,8 pixel. Nessuna escalation temporale nei parametri esaminati. | Implementato; difficoltà costante confermata; raggiungibilità da verificare. |
| Piattaforme mobili | [MovingPlatform.cpp](../src/World/MovingPlatform.cpp): MovingPlatform::UpdatePosition aggiorna x con cos(theta), oltre alla discesa base. HandleCollisions non trasferisce esplicitamente il movimento orizzontale al personaggio. | Implementato; comportamento percepito da verificare. |
| Piattaforme fragili | [BreakablePlatform.cpp](../src/World/BreakablePlatform.cpp): TriggerBreak avvia un timer di 50; UpdatePosition lo decrementa con dt × velocità di scrolling; Draw applica dissolvenza. [BreakablePlatform.h](../src/World/BreakablePlatform.h): stato e valori iniziali. | Implementato; circa 0,5 secondi a scrolling 100, timer fermo a scrolling zero. |
| Generazione e raccolta pickup | [PickupManager.cpp](../src/Pickups/PickupManager.cpp): GeneratePickups seleziona uniformemente uno dei cinque tipi, con intervallo verticale 200–400 pixel e posizione indipendente dalle piattaforme; Update gestisce raccolta e rimozione. [Pickup.cpp](../src/Pickups/Pickup.cpp): CheckCollisionPlayer usa cerchio-rettangolo. | Implementato; accessibilità delle posizioni da verificare. |
| Punteggio | [GameScene.cpp](../src/Scene/GameScene.cpp): Updates assegna punti temporali soltanto con isRunning e gameStart; UpdateScore applica il moltiplicatore. [GameScene.h](../src/Scene/GameScene.h): scoreIncrement = 10. PickupManager::Update aggiunge valore moneta × moltiplicatore; [PickupManager.h](../src/Pickups/PickupManager.h): Defaults.coinValue = 100. | Implementato; valori approvati. |
| Spring | [Spring.cpp](../src/Pickups/Spring.cpp): Spring::OnCollect imposta il moltiplicatore del prossimo salto. [Character.cpp](../src/Character/Character.cpp): SetNextJumpMultiplier; PlayableCharacter::Jump lo consuma e ripristina 1. Defaults.jumpMultiplier = 1,6. | Implementato; non è un lancio automatico né un salto aggiuntivo. |
| Effetti temporanei | [ScoreMultiplier.cpp](../src/Pickups/ScoreMultiplier.cpp), [Slow.cpp](../src/Pickups/Slow.cpp), [ReverseControl.cpp](../src/Pickups/ReverseControl.cpp): OnCollect chiama AddEffect. PickupManager::Defaults stabilisce ×2 per 15 s, velocità ×0,6 per 12 s, inversione per 5 s. [EffectSystem.cpp](../src/Pickups/EffectSystem.cpp): AddEffect aggiorna durata e valore di un effetto già presente; Update rimuove gli scaduti; getter applicano gli effetti. | Implementato; valori approvati. |
| Sconfitta | GameScene::CheckGameOver richiede velocità verticale almeno 1000 e bordo superiore del personaggio sotto Game::height. Disattiva la partita, riproduce il suono e chiude la musica; Game::Start passa a End. | Implementato; il semplice superamento del bordo inferiore non basta. |
| HUD | GameScene::DrawUI e DrawScore mostrano punteggio e istruzioni per cinque secondi con dissolvenza negli ultimi due. [UIStatusEffects.h](../src/UI/UIStatusEffects.h): Draw mostra durata degli effetti; [UIPickMessage.h](../src/UI/UIPickMessage.h): PushAt, Update, Draw gestiscono messaggi di raccolta. | Implementato; leggibilità e sovrapposizioni da verificare. |
| Aspetto e audio | PlayableCharacter::Draw e Update usano knight.png; Platform::LoadTextures carica tre texture; Pickup::LoadSpriteSheet carica coin.png condiviso dai pickup, distinti da tinte e simboli. PickupManager::InitSounds e Update caricano e riproducono suoni; [BackgroundMusic.cpp](../src/Audio/BackgroundMusic.cpp): Init, Play, Close. | Implementato; resa e riproduzione non verificate. |
| Progressione e vittoria | Generazione e velocità non hanno una crescita temporale; le scene esaminate non espongono campagna, sblocchi, persistenza del record o vittoria. L'utente conferma struttura infinita e difficoltà costante. | Modello infinito confermato; nessuna ulteriore funzionalità pianificata attestata. |

## Discrepanze e verifiche aperte

1. **HUD richiesto contro codice:** l'utente chiede di considerare rimossi i contatori. PlayableCharacter::Draw contiene ancora “Jumps” e, nel ramo con texture, “Vel Y”. Su richiesta dell'utente, il one-pager non include più la sezione dei limiti e delle verifiche; gli indicatori di debug restano documentati qui; nessuna modifica al gioco è stata effettuata.
2. **Titolo e testi UI:** il titolo confermato è Knight to Sky; BeginScene mostra “JUMP!” e Game::Game usa “Game” come titolo finestra. “PRESS ENTER TO RESTART!” porta alla schermata iniziale: il flusso è intenzionale, ma il testo non esplicita il secondo Enter.
3. **Durata del moltiplicatore:** il commento in [ScoreMultiplier.h](../src/Pickups/ScoreMultiplier.h) parla di 10 secondi, mentre PickupManager::Defaults passa 15 secondi. Il documento usa il valore eseguibile approvato.
4. **Raggiungibilità:** PlatformsManager::GenerateOne contiene un commento sulla garanzia di salti possibili, ma playerMaxJumpHorizontal deriva da una velocità verticale negativa. Il calcolo usa anche moveSpeed = 150, contro 200 in Character. Non si assume quindi che le sequenze siano sempre percorribili; occorre verificarle considerando scrolling, doppio salto, wrap e piattaforme mobili.
5. **Dipendenza dai frame:** MovingPlatform::UpdatePosition aggiunge cos(theta) alla posizione orizzontale ogni frame senza scalare lo spostamento per dt. Va verificato a frame rate differenti.
6. **Percorso musica:** [GameScene.h](../src/Scene/GameScene.h) usa assets/audio/background.mp3; il percorso presente è assets/Audio/background.mp3. Possibile problema su filesystem sensibili alle maiuscole.
7. **Limiti dell'analisi:** nessuna prova di bilanciamento, durata media delle run, divertimento, accessibilità, stabilità o compatibilità multipiattaforma. Non sono disponibili risultati di playtest, target audience, design pillars o milestone di rilascio; non vengono inventati.

## Revisione per uso README — 17 ottobre 2026

- Riletti il one-pager esistente, README, CMakeLists.txt e i sorgenti pertinenti a controlli, scene, generazione, raccolta ed effetti. `.codex/AGENTS.md` non è presente nel checkout; applicate le istruzioni AGENTS.md fornite in chat.
- Le conferme storiche sopra riportate provengono dal registro preesistente e non sono state raccolte nuovamente in questa revisione.
- Istruzioni di compilazione ricavate da [CMakeLists.txt](../CMakeLists.txt): `cmake_minimum_required`, `CMAKE_CXX_STANDARD`, `find_package`, `pkg_check_modules`, fallback `RAYLIB_DIR`, `add_executable` e copia `POST_BUILD`. Il percorso Windows è quello configurato in [.vscode/settings.json](../.vscode/settings.json), proprietà `cmake.configureSettings.RAYLIB_DIR`; va adattato alla macchina dell'utente. I comandi non sono stati eseguiti e non costituiscono una compilazione verificata.
- Le istruzioni di avvio richiedono come directory corrente quella dell'eseguibile perché i caricamenti degli asset usano percorsi relativi. Per generatori con più configurazioni la cartella effettiva può essere una sottocartella della build; l'esempio fornito usa Ninja.
- Il documento rimane in `docs/ONE_PAGER.md`; il README principale e il codice del gioco non vengono modificati. Il testo non contiene link relativi dipendenti dalla sua posizione, per consentirne il riuso nella radice della repository.
- Nessuna compilazione o esecuzione del gioco effettuata. Nessuna verifica di impaginazione su una pagina fisica.

## Traduzione inglese e contesto del progetto — 17 ottobre 2026

- Su richiesta esplicita dell'utente, tradotto `docs/ONE_PAGER.md` in inglese e rimosse le sezioni sullo stato da verificare e sulle funzionalità pianificate. Le discrepanze e i limiti dell'analisi restano conservati in questo registro.
- L'utente conferma che si tratta di un progetto di gruppo e del primo approccio allo sviluppo di un gioco interamente in C++. Il documento riporta queste informazioni nella sezione “About the project”, senza attribuire contributi individuali o risultati formativi non dichiarati.
- Mantenute le meccaniche e le istruzioni di compilazione già documentate. Nessuna modifica al gioco e nessuna nuova verifica in esecuzione.
