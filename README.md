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

Su touchscreen compaiono i pulsanti grandi: sterzo L/R, gas, freno, drift, oltre
al tasto pausa in alto a sinistra.

Dalla schermata **Comandi** puoi scegliere fra:
- **Pulsanti** (default) — sterzo a due pulsanti
- **Inclina** — sterzo via `deviceorientation` (su iOS verrà richiesto il
  permesso al primo uso)

Lo zoom e lo scroll sono disabilitati per evitare azioni accidentali durante la
guida.

## Drift e boost

Tieni premuto **Spazio** (o A su gamepad / DRIFT su mobile) mentre sei in curva
a velocità sostenuta. L'auto perde aderenza, lascia segni di gomma e fumo,
mentre la barra **BOOST DRIFT** si riempie. Quando rilasci il freno a mano con
boost sufficiente, ottieni una breve accelerazione extra.

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
- Mobile: iOS Safari, Chrome Android
- Gamepad: tutti i browser desktop moderni
- DeviceOrientation: richiede permesso su iOS 13+

## Reset progressi

Dalla schermata **Circuiti**, pulsante "AZZERA PROGRESSI" — chiede conferma e
ricomincia da zero (sblocca solo L1).

## Crediti

Programmazione: Alessandro Pezzali · Sviluppato con Claude Code · Font: Bebas
Neue e Saira da Google Fonts.
