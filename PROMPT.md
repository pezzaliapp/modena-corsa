# MODENA · CORSA — Prompt per Claude Code (build autonoma)

Questo file contiene **(1)** i comandi da terminale per preparare la repo `modena-corsa`
e lanciare Claude Code, e **(2)** il prompt completo da dare a Claude Code perché
migliori il gioco in piena autonomia.

---

## 1) Comandi da terminale (copia–incolla)

### Setup repo + base esistente

```bash
# crea la cartella di progetto e entra
mkdir modena-corsa && cd modena-corsa

# copia qui dentro il file di partenza (rinominalo index.html)
# es. se è in Download:
cp ~/Downloads/modena-corsa.html ./index.html

git init
git branch -M main
echo "node_modules/" > .gitignore
git add . && git commit -m "base: arcade racer Modena (v1)"
```

### Crea la repo su GitHub (serve la GitHub CLI `gh`)

```bash
# autentica una sola volta
gh auth login

# crea la repo pubblica e collega il remote
gh repo create modena-corsa --public --source=. --remote=origin --push
```

> Senza `gh`? In alternativa: crea la repo dal sito GitHub, poi
> `git remote add origin https://github.com/TUO-UTENTE/modena-corsa.git`
> e `git push -u origin main`.

### Installa e avvia Claude Code

```bash
# richiede Node.js 18+ (verifica con: node -v)
npm install -g @anthropic-ai/claude-code

# entra nel progetto e avvia (autenticati al primo lancio)
cd modena-corsa
claude
```

### Dai il prompt a Claude Code

A `claude` avviato, incolla il prompt della sezione 2 (o salvalo come
`PROMPT.md` e scrivi: `Leggi PROMPT.md ed eseguilo per intero in autonomia`).

### Commit + push del lavoro finito

```bash
git add -A
git commit -m "v2: comandi avanzati, grafica migliorata, 5 livelli"
git push
```

---

## 2) Prompt per Claude Code

> Incolla tutto ciò che segue dentro Claude Code.

```
RUOLO
Sei un game developer senior front-end. Lavora in piena autonomia: pianifica,
implementa, testa nel codice e itera fino al risultato. Non chiedere conferme
intermedie. Alla fine fai un riepilogo delle modifiche.

CONTESTO
Nella repo c'è `index.html`: un arcade racer pseudo-3D stile OutRun ambientato a
Modena (Ghirlandina, Duomo, Motor Valley, rossa di Maranello, tramonto). È un
singolo file HTML senza dipendenze esterne, canvas 2D puro. DEVE restare un
unico file HTML autosufficiente (apribile con doppio click, niente build, niente
server, niente npm a runtime). Font da Google Fonts via <link> è ok.

OBIETTIVO
Portarlo alla versione 2: comandi molto migliori, grafica notevolmente più
ricca, e 5 livelli con difficoltà crescente. Mantieni 60 FPS su un laptop medio
e su mobile recente. Non rompere la compatibilità "apri e gioca".

REQUISITI — COMANDI
1. Sterzo analogico con accelerazione/ritorno morbido (non on/off): pressione
   prolungata aumenta l'angolo, rilascio rientra dolce. Sensibilità che scala
   con la velocità.
2. Freno a mano / drift: tasto Spazio (o tasto dedicato su mobile). In drift
   l'auto scivola, lascia segni di gomma e particelle di fumo; uscita pulita dà
   un piccolo boost. Aderenza distinta asfalto vs. fuori strada.
3. Supporto Gamepad API: stick sinistro = sterzo analogico, RT/grilletto = gas,
   LT = freno, A = freno a mano. Rilevamento e hot-plug del controller.
4. Mobile rifatto: sterzo a due pulsanti grandi con risposta analogica OPPURE
   modalità "inclina il telefono" (deviceorientation) selezionabile; pulsanti
   gas/freno/handbrake chiari, area touch ampia, nessuno scroll/zoom accidentale.
5. Tasti: ↑/W gas, ↓/S freno, ←→/A D sterzo, Spazio handbrake, R restart,
   P pausa, M audio. Schermata "Comandi" accessibile dal menu e in pausa.
6. Pausa vera (P) con overlay e ripresa; il tempo di gara non scorre in pausa.

REQUISITI — GRAFICA
1. Ciclo luce per livello: alba, mezzogiorno, tramonto, blu serale, notte. In
   notte: fari dell'auto che illuminano la strada, lampioni, riflessi.
2. Meteo per livello: sereno, foschia, pioggia (gocce + spray dietro le ruote +
   asfalto più scivoloso e riflettente). Tutto proceduralmente disegnato.
3. Effetti: motion blur a velocità alta, scia/boost, scintille a contatto,
   fumo del drift, polvere fuori strada, sfarfallio calore sull'orizzonte,
   leggero camera shake su urti e cordoli, grana/vignettatura.
4. Parallasse multi-livello sullo sfondo (colline, città, alberi vicini) con
   profondità coerente alla curvatura della strada.
5. Auto del giocatore ridisegnata meglio (carrozzeria con riflessi, sospensioni
   che reagiscono a dossi e sterzo, fari/stop funzionali di notte). Traffico
   con 4–5 modelli distinti e luci posteriori.
6. Landmark di Modena più curati e contestuali al livello (Ghirlandina, Duomo,
   piazza, autodromo/box, cipressi, casolari, filari di vite). HUD ridisegnato
   coerente, mini-mappa del tracciato opzionale, popup "GIRO/PUNTI/DRIFT".
7. Niente estetica "AI generica": tipografia e palette curate, coerenti col
   tema motorsport italiano.

REQUISITI — LIVELLI E DIFFICOLTÀ
Crea 5 circuiti selezionabili con difficoltà crescente. Si sbloccano in
sequenza (completa il precedente per aprire il successivo); stato di sblocco
salvato in localStorage. Per ciascuno: nome, ora del giorno, meteo, tracciato
con curvatura/dislivello propri, numero giri, traffico, e soglia tempo per
medaglia (Bronzo/Argento/Oro).
  L1 "Centro Storico — Alba": facile, curve dolci, poco traffico.
  L2 "Viali di Modena — Mezzogiorno": medio, esse, traffico moderato.
  L3 "Colline & Vigneti — Tramonto": dislivelli forti, curve cieche.
  L4 "Autodromo — Sera": tecnico, traffico denso, chicane.
  L5 "Passo Appenninico — Notte+Pioggia": difficile, buio, asfalto scivoloso,
     tornanti stretti, tempo limite.
Difficoltà progressiva: più traffico e più veloce, curve più strette, presa
ridotta, limite di tempo negli ultimi livelli, IA rivale opzionale che gareggia.
Schermata di selezione livello con anteprima, medaglie e miglior tempo.

QUALITÀ E PROCESSO
- Codice pulito e organizzato dentro l'unico file (sezioni commentate: input,
  fisica, rendering, livelli, audio, UI). Niente librerie esterne JS.
- Game loop a delta-time stabile, niente memory leak, gestione resize/rotate.
- Audio sintetizzato (WebAudio): motore che scala coi giri, stridio gomme in
  drift, urti; toggle e volume.
- Salva miglior tempo, medaglie e sblocchi in localStorage; pulsante reset.
- Testa la logica con verifiche/console quando utile; correggi i bug che trovi.
- Performance: punta a 60 FPS; degrada effetti se il device è lento.
- Mantieni l'unico file < ~250 KB se possibile.

CONSEGNA
1. Aggiorna `index.html` con tutto quanto sopra.
2. Crea/aggiorna `README.md`: descrizione, comandi, lista livelli, come giocare,
   come pubblicarlo con GitHub Pages.
3. Verifica che si apra a doppio click e funzioni offline.
4. Esegui i commit con messaggi chiari (puoi fare più commit logici).
5. Stampa un riepilogo finale: cosa hai cambiato, scelte di design, eventuali
   limiti noti.

Procedi ora dall'inizio alla fine senza fermarti a chiedere.
```

---

## 3) Suggerimenti rapidi

- Per pubblicarlo online gratis dopo la build: `Settings → Pages → Deploy from
  branch → main / root`, e in pochi minuti sarà su
  `https://TUO-UTENTE.github.io/modena-corsa/`.
- Se Claude Code chiede permessi su comandi/file, autorizza pure: il prompt è
  pensato per un lavoro autonomo end-to-end.
- Vuoi farlo continuare in più sessioni? Aggiungi in coda al prompt:
  `Tieni un TODO.md aggiornato con stato e prossimi passi`.
