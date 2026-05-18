# MODENA · CORSA — v2

Un arcade racer pseudo-3D ambientato a Modena e dintorni: dalla Ghirlandina ai
passi appenninici. Un solo file HTML, niente dipendenze, apri e gioca.

![v2](https://img.shields.io/badge/version-2.0-rosso) ![single-file](https://img.shields.io/badge/single--file-HTML-orange)

## Come si gioca

Apri `index.html` con doppio click nel browser. Niente installazione, niente
server, niente connessione (le sole risorse esterne sono i Google Fonts, ma il
gioco funziona anche senza).

Dalla schermata titolo:
- **Scegli circuito** — selezione dei 5 tracciati con anteprima e medaglie
- **Corsa rapida** — entra nell'ultimo livello sbloccato
- **Comandi** — riepilogo input e opzioni mobile/audio

## Comandi

### Tastiera

| Tasto | Azione |
|---|---|
| `↑` / `W` | Gas |
| `↓` / `S` | Freno |
| `← →` / `A D` | Sterzo analogico |
| `Spazio` | Freno a mano (drift) |
| `P` | Pausa |
| `R` | Restart livello |
| `M` | Audio on/off |
| `Esc` | Torna al menu |

Lo sterzo è **analogico con smoothing**: la pressione prolungata aumenta
gradualmente l'angolo, il rilascio rientra dolcemente. La sensibilità scala con
la velocità (più reattivo a bassa velocità).

### Gamepad

Supporto Gamepad API con hot-plug: collega un controller anche a gioco già
avviato e verrai notificato.

| Comando | Azione |
|---|---|
| Stick L (asse X) | Sterzo analogico |
| RT / R2 | Gas |
| LT / L2 | Freno |
| A / Cross | Freno a mano |
| Start | Pausa |
| B / Cerchio | Restart |

### Mobile / Touch

Su smartphone l'esperienza è stata ripensata da zero per essere comoda con
una sola mano per lato, senza ostruire la visuale.

**Tocca per attivare i sensori, poi gioca inclinando.** Quando avvii un
livello su telefono compare la schermata **"GIOCA INCLINANDO"**:

1. Tieni il telefono in **orizzontale**, comodo nelle mani come vuoi giocare.
2. Premi il pulsante grande. Su iPhone iOS richiede in quel momento
   l'autorizzazione ai sensori — è un gesto utente reale, quindi il prompt
   compare correttamente.
3. L'inclinazione attuale viene presa come **zero di calibrazione**: da lì
   inclini per sterzare.

In gara su mobile ci sono **solo 4 controlli** a schermo, fissi:

| Controllo | Posizione | Funzione |
|---|---|---|
| **GAS** | basso-destra, 128px | tieni premuto per accelerare |
| **FRENO** | basso-sinistra, 128px | tieni premuto per frenare; **tieni a fondo in curva → drift** |
| **ESCI** | alto-destra | conferma e torna alla selezione livello |
| **CENTRA** | alto-centro | ricalibra lo zero del tilt in qualsiasi momento |

Lo sterzo è **solo tilt** (asse gamma): nessun pulsante L/R che occupi
l'area di guida. Risposta analogica con dead-zone di 2°, clamp a 25°.

**Drift implicito**: tenendo il freno a fondo in curva ad alta velocità l'auto
entra in drift automaticamente, carica il boost e — al rilascio del freno —
spara una breve accelerazione extra. Il pulsante freno si illumina quando
sei in drift attivo.

**Fallback senza sensori**: se l'utente nega il permesso o il dispositivo non
ha l'accelerometro (in-app browser, alcuni emulatori), la schermata avvisa
chiaramente e si passa automaticamente a **due zone-touch laterali invisibili**
(metà sinistra/destra dello schermo) come ripiego.

**Orientamento**: il gioco forza il landscape: in portrait compare un overlay
"Ruota il telefono" — l'esperienza è progettata per il formato orizzontale.

**Requisiti**:
- iOS 13+ (Safari): serve `https://` per la richiesta sensori — il file
  aperto via `file://` non riceverà il permesso. Usa GitHub Pages, Netlify,
  o un server statico locale via `python3 -m http.server`.
- Android: `https://` è raccomandato; alcuni dispositivi accettano anche
  `http://` su rete locale.
- Touch-action e user-select sono disabilitati: nessun pull-to-refresh,
  doppio-tap-zoom o selezione testo accidentale durante la guida.

## Drift e boost

- Tastiera: **Spazio**
- Gamepad: **A / Cross**
- Mobile: **FRENO tenuto a fondo in curva** (drift implicito — l'icona si
  illumina quando è attivo)

Mentre sei in drift l'auto perde aderenza, lascia segni di gomma e fumo, e la
barra **BOOST DRIFT** si riempie. Al rilascio (o quando finisci la curva) con
boost sufficiente parte una breve accelerazione extra con kick di camera, boom
audio e vignettatura calda. Un drift lungo e pulito (in strada) conta come
**PERFECT DRIFT** e alimenta la combo.

L'aderenza è ridotta fuori strada e sotto la pioggia (livello 5).

## Circuiti

I livelli si sbloccano in sequenza: completa il precedente per accedere al
successivo. Tempo migliore, medaglie e sblocchi sono salvati in localStorage.

| # | Nome | Atmosfera | Meteo | Difficoltà |
|---|---|---|---|---|
| 1 | Centro Storico | Alba | Sereno | Facile |
| 2 | Viali di Modena | Mezzogiorno | Sereno | Medio |
| 3 | Colline & Vigneti | Tramonto | Foschia | Medio-alto |
| 4 | Autodromo | Sera blu | Sereno | Tecnico |
| 5 | Passo Appenninico | Notte | Pioggia | Difficile (tempo limite) |

Le medaglie sono assegnate sul **tempo totale di gara**:
- 🥇 ORO — tempo soglia oro
- 🥈 ARGENTO
- 🥉 BRONZO

## Sistema combo e rank

Ogni gara è valutata con un **rank S/A/B/C/D**:
- **S** — oro + alto stile (combo, perfect drift, near miss)
- **A** — oro a cronometro
- **B** — argento
- **C** — bronzo
- **D** — completato senza medaglia

Lo **stile** si guadagna con:
- **PERFECT DRIFT** — chiudere un drift lungo e pulito (in strada) — bonus + combo
- **NEAR MISS** — passare vicinissimo a un'altra auto ad alta velocità —
  bonus + slow-mo brevissimo + scintille
- **COMBO** — concatena drift puliti e near miss entro 3.2s: il moltiplicatore
  sale, il bonus per ogni azione cresce con la combo

La combo si **azzera al crash** o allo scadere del timer. A fine gara la
schermata risultati ti mostra di quanto hai mancato il rank superiore
("hai mancato per 0.4s" / "ti serve più stile") per invogliarti a rigiocare.

## Caratteristiche v2

- **Ciclo luce** per livello (alba → notte) con tinta del cielo, sole/luna,
  illuminazione ambientale e fari dell'auto attivi di notte.
- **Meteo dinamico** per livello: sereno, foschia, pioggia (gocce animate,
  spray dietro le ruote, asfalto più scuro e scivoloso).
- **Effetti grafici**: motion blur ad alta velocità, scia/boost,
  scintille a contatto, fumo del drift, polvere fuori strada, camera shake,
  vignettatura e grana sottile.
- **Parallasse multi-livello** sulle colline e città lontane.
- **Mini-mappa** del tracciato in HUD con posizione corrente.
- **Audio sintetizzato** via WebAudio: motore che scala col regime, stridio
  gomme in drift, urti, vento ad alta velocità.
- **Auto giocatore ridisegnata** con riflessi sulla carrozzeria, sospensioni
  reattive, fari/stop e fiamma di boost.
- **Traffico vario** con 5 modelli distinti (berlina, city car, SUV, sport,
  furgone) e luci posteriori funzionali di notte.
- **Landmark contestuali**: Ghirlandina e Duomo, palazzi storici, casolari
  emiliani, filari di vite, cipressi, box e tribune all'autodromo,
  lampioni di notte.

## Pubblicare su GitHub Pages

```bash
git push -u origin main
```

Poi dalle impostazioni del repo:

`Settings → Pages → Source: Deploy from a branch → Branch: main / root → Save`

In pochi minuti il gioco sarà disponibile su:
`https://<TUO-UTENTE>.github.io/modena-corsa/`

Essendo un singolo file HTML statico, GitHub Pages è la via più semplice.
Funziona ugualmente su Netlify, Vercel, Cloudflare Pages, S3 con object
hosting, o qualsiasi server statico (anche `python3 -m http.server`).

## Sviluppo locale

```bash
# basta aprire il file
open index.html

# oppure server statico per evitare CORS su alcune feature browser
python3 -m http.server 8000
# poi vai su http://localhost:8000
```

## Struttura

Tutto in `index.html`. Le sezioni dello script sono commentate:

1. Canvas / setup
2. Costanti
3. Definizione livelli
4. Storage (localStorage)
5. Input (tastiera, touch, gamepad, deviceorientation)
6. Audio sintetizzato (WebAudio)
7. Track builder
8. Stato gara + fisica
9. Particelle / tracce gomma
10. Render (cielo, strada, sprite, traffico, auto, meteo)
11. HUD / mini-mappa / UI
12. Flow (menu, livelli, pausa, finish)

## Browser supportati

- Chrome / Edge / Safari / Firefox recenti
- Mobile: iOS Safari 13+, Chrome Android — il tilt **richiede HTTPS** per
  ottenere il permesso sensori su iOS
- Gamepad: tutti i browser desktop moderni
- **In-app browser** (Instagram, Facebook, Telegram, ecc.): possono bloccare
  l'accesso ai sensori; in quel caso si attiva automaticamente il fallback
  zone-touch laterali invisibili
- **Auto-degrader**: se il framerate medio scende sotto 46 fps, il gioco
  riduce a runtime le particelle e le gocce di pioggia per restare fluido —
  l'utente vede solo un breve toast "QUALITÀ ↓"

## Reset progressi

Dalla schermata **Circuiti**, pulsante "AZZERA PROGRESSI" — chiede conferma e
ricomincia da zero (sblocca solo L1).

## Crediti

Programmazione: Alessandro Pezzali · Font: Bebas
Neue e Saira da Google Fonts.
