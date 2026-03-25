# UpVentures — Sito Web Istituzionale
**Tech Venture Building Studio · Torino**
> Sito statico single-page per UpVentures. Apple-inspired, light theme, full responsive.

---

## Anteprima

```
┌─────────────────────────────────────────────────────┐
│  ▲ UpVentures        [Chi Siamo] [Percorsi] [AI]   ✉ │
├─────────────────────────────────────────────────────┤
│                                                     │
│          • TECH VENTURE STUDIO — TORINO             │
│                                                     │
│              Scala                                  │
│           la Vetta.                                 │
│        Build the Future.                            │
│                                                     │
│   Il tuo partner operativo per trasformare          │
│   un'idea in un business che scala. Con l'AI.       │
│                                                     │
│      [ Scopri i Percorsi ↗ ]  [ Prenota Call ]     │
│   +24 Imprese · MVP 6-8W · AI Tools · Automazioni  │
│                                                     │
│  ◆ Venture Far  ◆ AI Automation  ◆ MVP in 6–8W     │
├─────────────────────────────────────────────────────┤
│  Il Problema Reale → I Percorsi → Metodo → AI →    │
│  Qualità → Manifesto → Contatti                    │
└─────────────────────────────────────────────────────┘
```

---

## Architettura

### Filosofia: Single-File Site
Il sito è contenuto interamente in **`index.html`** (~1.6MB).
CSS, HTML e JavaScript sono tutti inline — zero dipendenze runtime, zero bundler, zero build step.

```
Project_upventures/
├── index.html              ← Tutto il sito (CSS + HTML + JS inline)
├── brand/
│   ├── images/
│   │   └── manaslu-expedition.jpg   ← Unica immagine esterna (hero bg)
│   └── logo/                        ← Logo SVG (inline nel HTML)
├── Uplogo.svg              ← Logo standalone
├── vercel.json             ← Config deploy Vercel (static, no build)
├── .vercelignore           ← Esclude file inutili dal deploy
├── .gitignore
└── package.json            ← Solo dev script (npx serve)
```

### Stack Tecnico
| Layer | Tecnologia |
|---|---|
| HTML | Semantico, single-file |
| CSS | Custom Properties, Grid, Flexbox, `clamp()`, `backdrop-filter` |
| JS | Vanilla ES5+ (no framework, no librerie) |
| Font | Google Fonts — Pathway Extreme + DM Mono |
| Icone | SVG inline |
| Deploy | Vercel (static) |
| Dev | `npx serve . -p 3000` |

---

## Design System

### Colori
```css
--orange:  #ff5900   /* Accento primario — CTA, numeri, highlights */
--teal:    #1a50e0   /* Accento secondario — gradiente, link attivi */
--yellow:  #f2b705   /* Accento terziario — card The Flow */
--ink:     #1d1d1f   /* Testo principale (Apple black) */
--off:     #f5f5f7   /* Sfondo sezioni alternate (Apple off-white) */
--white:   #ffffff   /* Sfondo sezioni principali */
--muted:   #6e6e73   /* Testo secondario */
```

### Tipografia
```css
--ffp: 'Pathway Extreme'   /* Headline, titoli — display */
--ffm: 'DM Mono'           /* Label, tag, eyebrow — monospace */

Hero H1:  clamp(40px, 6vw, 82px) · weight 800 · tracking -0.038em
Section H2: clamp(32px, 4vw, 52px)
Body:     15–18px · weight 300 · line-height 1.72
```

### Spaziature (8-point system)
```css
--sp-xl: 120px   /* Padding sezioni desktop */
--sp-lg: 80px    /* Gap tra blocchi */
--sp-md: 48px    /* Spaziatura componenti */
--px:    52px    /* Padding orizzontale layout */
--inner: 1160px  /* Max-width contenuto */
```

### Breakpoint Responsive
| Breakpoint | Contesto |
|---|---|
| Default | Desktop (>1024px) |
| `≤1024px` | Laptop / Tablet landscape |
| `≤768px` | Tablet portrait — hamburger menu attivo |
| `≤480px` | Mobile — layout singola colonna |

---

## Struttura delle Sezioni

```
[HERO]
  └── Eyebrow pill "Tech Venture Studio — Torino"
  └── Headline tri-linea (Scala / la Vetta. / Build the Future.)
  └── Sottotitolo
  └── CTA buttons
  └── Proof strip (+24 Imprese · MVP 6-8W · AI Tools · Automazioni)

[TICKER]
  └── Striscia animata continua (orange bg)

[IL PROBLEMA REALE]  #sec-prob
  └── 4 problem card con "UV →" solution

[CASI STUDIO]  #sec-cases
  └── 3 case row con statistiche

[CHI SIAMO]  #about
  └── Foto team + badge "+24 progetti lanciati"
  └── Testo con border-left accent

[PERCORSI / SERVIZI]  #sec-services
  └── 3 card (The Ascent / The Flow / The Depth)
  └── Ogni card apre subpage dedicata

[IL METODO ALPINE]  #sec-process
  └── 3 step (Base Camp / La Rotta / Summit Push)
  └── Connector line + accent color per step

[AI & AUTOMATION]  #sec-ai
  └── Lista feature + Terminal UI animato

[QUALITÀ]  #sec-qualita
  └── 4 stat countUp animate (Apple quartic ease)
  └── 6 quality card

[MANIFESTO]  #sec-manifesto
  └── Dark section (sfondo #1d1d1f)
  └── Quote + statistiche

[CONTATTI]  #sec-contact
  └── CTA band dark con orange glow

[FOOTER]
  └── Link social + info legali
```

---

## Animazioni e Interazioni

### Scroll Reveal
Ogni blocco ha classe `.rv` — appare con `fadeup` quando entra nel viewport:
```js
// IntersectionObserver con threshold:0.12
// Le classi .d1/.d2/.d3 aggiungono delay (0.1s / 0.2s / 0.3s)
el.classList.add('in') // → opacity:1, translateY:0
```

### CountUp (Sezione Qualità)
Numeri animati all'entrata — quartic ease-out, 1400ms:
```js
function ease(t){ return 1 - Math.pow(1 - t, 4) }
// Triggerato da IntersectionObserver al 50% visibilità
// Solo elementi con data-target numerico puro
```

### Navbar
- Fixed sticky con `backdrop-filter: blur(20px) saturate(180%)`
- Layout 3 colonne CSS Grid (`1fr auto 1fr`) per centratura perfetta
- Active link tracking via `scrollY + innerHeight/2`
- Su mobile: hamburger drawer con `max-height` animation + body scroll lock

### Altre
- Card lift: `translateY(-6px)` + box-shadow al hover
- Button press: `scale(0.96)` su click (`spring` cubic-bezier)
- Heading underline reveal: `::after` con `scaleX` da 0→1
- Parallax hero: `translateY` via rAF sull'immagine di sfondo
- Ticker pause: `animation-play-state:paused` al hover

---

## Deploy su Vercel

Il sito è pronto. Nessun build step richiesto.

### Opzione A — Vercel CLI (rapida)
```bash
npm i -g vercel
cd Project_upventures
vercel
```

### Opzione B — GitHub (consigliata, autodeploy)
```bash
git init
git add index.html brand/images/manaslu-expedition.jpg Uplogo.svg vercel.json .vercelignore
git commit -m "feat: initial deploy"
git remote add origin https://github.com/TUO-USER/upventures.git
git push -u origin main
```
Poi su vercel.com → Import Repository → Deploy.
Ogni `git push` aggiorna il sito automaticamente.

---

## Cosa Abbiamo Fatto (Changelog)

| Area | Intervento |
|---|---|
| **Hero** | Headline cinematica 3 linee, background montagna con parallax e overlay Apple-light |
| **Nav** | Fixed sticky, 3-column CSS Grid per centratura perfetta, blur glassmorphism, hamburger mobile con drawer |
| **Theme** | Passaggio dark → light (Apple off-white #f5f5f7), testo nero su bianco |
| **Brand colors** | Sostituito teal #32aeae con royal blue #1a50e0, mantenuto orange #ff5900 |
| **Contatti** | Rimosso floating social (FB+IG), aggiunta icona busta email singola + in navbar |
| **AI & Automation** | Nuova sezione con terminal UI animato |
| **Qualità** | Nuova sezione con countUp stats + 6 quality card |
| **Animazioni** | Scroll reveal, countUp quartic ease, card lift, button press, active nav tracking, ticker pause |
| **Responsive** | 4 breakpoint (1024/768/480), hamburger drawer, layout adattivo per ogni sezione |
| **Personalità sezioni** | Manifesto dark, step accent colors, case numbers colorati, about border-left, CTA dark glow |
| **Proof strip** | "+24 Imprese · MVP 6-8W · AI Tools · Automazioni" sotto i bottoni hero |
| **Deploy** | `vercel.json` semplificato, `.vercelignore`, rinomina immagine (no spazi) |
| **Overflow fix** | `overflow-x:hidden` su `html` + `body` |

---

## Stato Attuale

**Score UX cold visitor: 6.5 / 10**

| Cosa funziona | Cosa manca |
|---|---|
| Visivamente premium e coerente | L'headline è evocativa ma non spiega cosa facciamo |
| Animazioni fluide stile Apple | "Tech Venture Studio" è gergo non immediato per PMI |
| Proof strip con concretezza | Le metafore alpine richiedono decodifica cognitiva |
| Sezioni AI e Qualità complete | Nessuna pricing indicativa |
| Mobile responsive | SEO basica (solo meta description) |
| Deploy-ready su Vercel | Analytics non configurata |

---

## Cosa Migliorare (Priorità)

### Alta Priorità
- [ ] **Chiarezza hero** — Aggiungere una riga esplicita sotto il tagline: *"Costruiamo MVP, tool AI e automazioni per PMI e startup"*
- [ ] **Subtitle servizi** — Rendere più leggibili i subtitle delle card (già migliorato, ma valutare copia più diretta es. "Consulenza Strategica + Tech")
- [ ] **Form contatto** — Il bottone "Prenota una Call" apre solo un overlay, non un form o link Cal.com reale

### Media Priorità
- [ ] **SEO** — Aggiungere `og:image`, `og:title`, structured data (Organization schema)
- [ ] **Analytics** — Integrare Vercel Analytics o Plausible (privacy-first)
- [ ] **Performance** — L'immagine hero (98KB) può essere convertita in `.webp`
- [ ] **Pricing indicativa** — Una fascia di prezzo orientativa riduce le richieste di call non qualificate

### Bassa Priorità
- [ ] **Blog/Contenuti** — Una sezione insight o case study approfonditi per SEO long-tail
- [ ] **Testimonial** — Quote di founder/clienti con foto aumenterebbero la social proof
- [ ] **Splitting del file** — A lungo termine, separare CSS/JS in file distinti per caching browser
- [ ] **Lingua EN** — Toggle ITA/ENG se si punta a mercato internazionale

---

## Sviluppo Locale

```bash
# Avvia server locale su porta 3000
npm run dev
# oppure
npx serve . -p 3000
```
Apri `http://localhost:3000`

---

*Ultimo aggiornamento: Marzo 2026*
