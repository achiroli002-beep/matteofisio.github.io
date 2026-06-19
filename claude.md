# Studio Fisioterapico — Landing page single-page

Documentazione del progetto. Permette a chiunque (umano o LLM) di riprendere,
capire e modificare il sito rapidamente.

---

## 1. Panoramica

- **Cos'è:** una landing page single-page (one-page) per un **fisioterapista**, con
  forte effetto "wow" (micro-interazioni, animazioni allo scroll, cursore custom),
  ma estetica pulita e medicale.
- **Per chi:** professionista che vuole presentarsi, mostrare cosa tratta e
  raccogliere richieste di appuntamento.
- **Tipo di sito:** **100% statico**, nessun backend.
- **URL GitHub Pages previsto:** `https://matteofisio.github.io/`
  (il nome della repo è `matteofisio.github.io`, quindi è un *user/organization site*
  servito dalla root).

---

## 2. Stack & scelte

| Ambito | Scelta | Perché |
|---|---|---|
| Markup | HTML5 semantico (`header/nav/main/section/footer`) | accessibilità + SEO |
| Stile | **CSS custom moderno** (Flexbox/Grid + custom properties), niente Tailwind | zero dipendenze di build, palette ri-temizzabile da `:root` |
| Logica | **JavaScript ES6+ vanilla** | nessun framework, nessun bundler |
| Animazioni | **GSAP 3** + `ScrollTrigger` + `MorphSVGPlugin` (oggi gratuiti) via CDN jsDelivr | timeline, scroll e morph SVG |
| Form | **Formspree** (solo attributi HTML + handler `fetch`) | invio senza backend, successo in-pagina |
| Font | **Google Fonts** — Poppins (titoli) + Inter (testo) | coppia leggibile e professionale |

**Perché un singolo file statico (no build):** tutto (HTML + CSS + JS) vive in
`index.html`. Si apre nel browser e funziona; si carica su GitHub Pages senza
trasformazioni. Nessun `npm install`, nessun step di compilazione, nessun rischio
di "funziona in locale ma non in produzione".

> Nota sulla palette: come accento primario delle CTA è stato scelto un **verde teal
> AA-safe (`#2E7D6B`)** invece del verde salvia chiaro `#6FAE9B`, perché il testo
> bianco sul salvia chiaro **non** raggiunge il contrasto WCAG AA. Il salvia resta
> come accento brand/decorativo (`--sage`). Tutto è modificabile da `:root`.

---

## 3. Come usarlo in locale

È un file statico: ci sono due strade.

1. **Doppio click** su `index.html` → si apre nel browser. Funziona tutto
   (GSAP e i font vengono dal CDN, quindi serve la connessione a Internet).
2. **Server statico locale** (consigliato, evita eventuali limiti del protocollo `file://`):

   ```bash
   # con Python 3
   python3 -m http.server 8000
   # poi apri http://localhost:8000

   # oppure con Node
   npx serve .
   ```

---

## 4. Deploy su GitHub Pages

1. Fai il **commit** e il **push** di `index.html` (e `claude.md`) sul branch **`main`**.
   ```bash
   git add index.html claude.md
   git commit -m "Landing page fisioterapista"
   git push origin main
   ```
2. Su GitHub: **Settings → Pages**.
3. In **Build and deployment → Source** scegli **Deploy from a branch**.
4. **Branch:** `main` — **Folder:** `/ (root)` → **Save**.
5. Dopo 1–2 minuti il sito è online su `https://matteofisio.github.io/`.

> Essendo una repo `*.github.io`, il sito è pubblicato dalla root del branch `main`.
> Nessuna cartella `/docs` necessaria.

---

## 5. Mappa delle sezioni (àncore e funzione)

Ordine rispettato come da specifica. Le àncore sono gli `id` usati dal menu.

| # | Àncora | Sezione | Funzione |
|---|---|---|---|
| — | `#top` | Header/Nav | Navbar fissa trasparente → opaca allo scroll; menu + CTA "Prenota Ora" magnetica; hamburger su mobile |
| 1 | (hero) | **Hero** | Titolo ad alto impatto, sottotitolo, CTA magnetiche, foto; reveal animato delle righe |
| 2 | `#trattamenti` | **Cosa tratto** | SVG corpo umano interattivo (hover/tap/tastiera) + pannello condizioni da dati JSON |
| 3 | `#metodo` | **Il metodo** | Scrollytelling: colonna vertebrale che si raddrizza con lo scroll (pin + scrub) |
| 4 | `#aiuto` | **Lascia che ti aiuti** | Manichino diagnostico cliccabile + form richiesta (zona dolore) |
| 5 | `#chi-sono` | **Chi sono** | Foto, biografia, valori, loghi certificazioni; entrata con stagger |
| 6 | `#contatti` | **Contatti** | Form completo (nome/email/tel/messaggio) + info + mappa |
| 7 | (footer) | **Footer** | Social, P.IVA, copyright, orari, link rapidi |

---

## 6. Punti di personalizzazione

Tutto è dentro `index.html`. Cerca i commenti `TODO` e `[...]`.

- **Colori:** blocco `:root` (sezione CSS *1. DESIGN TOKENS*). Cambia `--accent`,
  `--sage`, `--bg`, ecc. e tutto il sito si ri-tematizza.
- **Font:** cambia il `<link>` di Google Fonts nel `<head>` e le variabili
  `--font-head` / `--font-body`.
- **Testi:** direttamente nell'HTML. I segnaposto sono tra parentesi quadre:
  `[Nome]`, `[Nome Cognome]`, `[Città]`, `[X] anni`.
- **Immagini:** gli `<img>` usano placeholder SVG inline (`src="data:image/svg+xml,…"`).
  Sostituisci `src` con i percorsi reali (es. `foto/hero.jpg`) mantenendo `alt` e
  `loading="lazy"`. Riguarda: hero, foto "Chi sono", loghi certificazioni, `og:image`.
- **Formspree (`TUO_ID`):** in **due** form (`#diagForm` e `#contactForm`) sostituisci
  `action="https://formspree.io/f/TUO_ID"` con il tuo endpoint. Finché c'è `TUO_ID`
  i form girano in **modalità demo** (mostrano il successo senza inviare).
- **Mappa:** `<iframe class="map-embed">` in *Contatti*. Cambia il parametro `q=` con
  le tue coordinate o indirizzo (oppure sostituisci con l'embed ufficiale di Google Maps).
- **Dati di contatto:** indirizzo, telefono (`tel:`), email (`mailto:`) nella sezione
  *Contatti*; orari e **P.IVA** nel footer.
- **Social:** link `href="#"` nel footer (Instagram, Facebook, LinkedIn).
- **SEO / dati struttura:** `<title>`, meta description, Open Graph e il blocco
  **JSON-LD** `Physiotherapy` nel `<head>` (nome, telefono, indirizzo, `geo`, orari).
- **Condizioni trattate (anatomia):** oggetto JS `ANATOMIA` (chiave = `id` dell'area SVG).
- **Zone del manichino:** attributi `data-value` / `data-label` sui gruppi `.zone`
  e relative `<option>` del `<select id="zonaSelect">` (tieni allineati i valori).

---

## 7. Inventario animazioni / interazioni (dove vivono nel JS)

Tutto il JS è in fondo al `<body>`, dentro un unico `DOMContentLoaded`. Blocchi
etichettati con lettere nei commenti:

- **A) Navbar opaca allo scroll** → `ScrollTrigger.create({ onUpdate … })`
  (toggle classe `.scrolled` sull'header).
- **B) Hamburger** (fuori da GSAP) → funzione `setMenu()` con `aria-expanded`,
  chiusura con `Esc`, backdrop e click sui link.
- **C) Cursore-osso** → dentro `gsap.matchMedia()`, ramo `isDesktop && !reduceMotion`.
  SVG osso inline in cima al `<body>` (`.cursor`). Movimento con **lerp su
  `gsap.ticker` (rAF)**; classe `.is-active` su elementi interattivi.
- **D) Pulsanti magnetici** → stesso ramo desktop; selettore `.magnetic`,
  `mousemove`/`mouseleave` con `gsap.to` (ritorno elastico).
- **E) Reveal allo scroll** → selettore generico `.reveal`, animati con
  `ScrollTrigger` (`once: true`). Con reduced-motion: comparsa immediata.
- **F) Hero timeline** → `gsap.timeline()` con `stagger` sulle righe `.hero-line`.
- **G) Anatomia interattiva** → oggetto `ANATOMIA`, funzioni `renderPanel()` /
  `activateRegion()`; fade del pannello con GSAP (`panelAnim`).
- **H) Scrollytelling colonna** → `buildSpine()` genera tracciato + vertebre (con
  `smoothPath()`, `curvedD`/`straightD`) a misura: grande su desktop, sottile e alta
  quanto il testo su mobile (viewBox in px → niente deformazione). La funzione
  `straighten()` aggiunge a una timeline **scrub** il morph di `#spineLine`
  (**MorphSVGPlugin**) e il rientro delle vertebre (`x → 0`). Desktop: `pin` sulla
  sezione; mobile: scrub senza pin (reversibile).
- **I) Manichino diagnostico** → `selectZone()` / `placeMarkerOnZone()`; sincronizza
  `<select>` ⇄ campo hidden `#zonaHidden` ⇄ marker SVG `#diagMarker`.
- **J) Form (Formspree)** → `wireForm()`: `preventDefault`, honeypot, validazione
  nativa, `fetch`, successo in-pagina (`revealSuccess` animata con GSAP).

L'orchestrazione motion è dentro **`gsap.matchMedia()`** con condizioni
`isDesktop` / `isMobile` / `reduceMotion`, con cleanup automatico al cambio di media.

---

## 8. Scelte di accessibilità

- **`prefers-reduced-motion`:** gestito sia in CSS (media query che azzera
  transizioni/animazioni e rende visibili i `.reveal`) sia in JS (ramo `reduceMotion`
  di `matchMedia`: niente pin/scrub, niente cursore custom, solo fade/stato finale).
- **Media query del puntatore:** cursore-osso e magnetici attivi **solo** con
  `@media (hover: hover) and (pointer: fine)` + guardia JS `isDesktop`. Su touch
  resta il cursore di sistema e il comportamento standard.
- **Niente scroll-trap su mobile:** il `pin` della sezione "Metodo" è creato solo su
  desktop. Su mobile la colonna è sottile e alta quanto il testo accanto e si raddrizza
  con uno **scrub SENZA pin** (reversibile: scrollando su si ri-scompone). Con
  reduced-motion resta dritta e ferma.
- **Tastiera:** aree anatomia e zone del manichino sono `tabindex="0"` + `role="button"`
  + `aria-label`, attivabili con Invio/Spazio; focus sempre visibile (`:focus-visible`).
- **Fallback `<select>` del manichino:** chi non usa l'SVG sceglie la zona dal menu;
  `<select>`, campo hidden e marker restano sincronizzati.
- **Semantica & ARIA:** `lang="it"`, landmark (`header/nav/main/section/footer`),
  heading gerarchici, `aria-labelledby` sulle sezioni, `aria-live` su pannello
  anatomia e messaggi dei form, skip-link "Salta al contenuto".
- **Immagini:** `alt` su tutte; `loading="lazy"` (tranne hero, caricata subito).
- **Contrasti:** testo e CTA scelti per rispettare WCAG 2.2 AA (vedi nota palette).

---

## 9. Limitazioni note e TODO

- [ ] **Configurare Formspree:** creare un form su formspree.io e sostituire `TUO_ID`
      nei due `action`. Finché non fatto, i form sono in **modalità demo**.
- [ ] **Sostituire i placeholder:** nome, città, biografia, anni di esperienza,
      P.IVA, telefono, email, indirizzo, social, coordinate mappa.
- [ ] **Immagini reali:** rimpiazzare i placeholder SVG (hero, ritratto, loghi cert.)
      e fornire una vera `og:image` 1200×630. Ottimizzare (formati moderni, dimensioni).
- [ ] **Dipendenza dal CDN:** GSAP e i font arrivano da CDN. Offline non funzionano
      le animazioni (la pagina resta comunque usabile: il JS rileva l'assenza di GSAP
      e mostra tutti i contenuti). Per massima resilienza si possono ospitare in locale.
- [ ] **Cookie/privacy:** la mappa Google embed può impostare cookie; valutare
      consenso/policy se necessario per il GDPR.

---

## 10. Convenzioni di codice

- **Nessun build step:** un solo file `index.html` autosufficiente. Niente bundler,
  niente transpiler, niente dipendenze npm.
- **CSS:** organizzato in sezioni numerate con commenti banner (1. Design tokens,
  2. Reset, …). Colori e misure **sempre** via custom properties in `:root`.
  Mobile-first; breakpoint principali 768px, 881px (collasso nav a 880px), 1280px.
- **JS:** vanilla ES6+, un solo `DOMContentLoaded`. Blocchi commentati con
  lettere (A–J) che corrispondono all'inventario del punto 7. La logica "motion"
  vive in `gsap.matchMedia()`; la logica essenziale (nav, form, anatomia, manichino)
  vive fuori così funziona anche senza GSAP.
- **Naming:** classi in stile BEM-light (`.blocco__elemento`, modificatori `--mod`);
  `id` in camelCase per gli hook JS, in kebab/italiano per le àncore di navigazione.
- **Commenti:** in italiano, presenti in corrispondenza di ogni animazione e
  interazione, come da specifica.
- **Accessibilità e performance** sono requisiti di prima classe: ogni nuova
  aggiunta deve preservare focus, contrasti, `prefers-reduced-motion`, `loading="lazy"`
  e l'uso prudente di `will-change`.
