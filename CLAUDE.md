# CPS / BWEV Lunch & Learn Slide Decks

## What This Repo Is

HTML-based slide decks for Critical Peake Services + Blue Whale EV lunch & learn presentations. Each client gets their own folder. The root `index.html` redirects to the active deck. Each deck is deployed as a GitHub Pages site.

---

## Repo Structure

```
/
├── index.html                        ← redirects to active deck
├── CLAUDE.md
├── Content/
│   ├── Logos/                        ← shared logos (CPS + BWEV)
│   ├── headshots/                    ← team headshot images
│   └── Brand Guidelines/             ← brand docs
├── federal-reit/                     ← FRIT deck (reference/archived)
│   ├── federal-reit-deck.html        ← controller
│   ├── cps-theme.css
│   ├── bwev-theme.css
│   └── slide-01.html … slide-18.html
└── legum-and-norman/                 ← active deck
    ├── legum-and-norman-deck.html
    ├── cps-theme.css
    ├── bwev-theme.css
    └── slide-01.html … slide-19.html
```

---

## Architecture

### Controller (`<deck>-deck.html`)
Loads all slides as `<iframe>` elements, handles keyboard/click navigation, slide transitions, and fullscreen. The iframe `src` order in the HTML determines presentation order (not the slide file numbers).

**Key controller elements:**
- `<div id="deck">` containing `<iframe class="slide-frame">` elements
- `#slide-counter` — displays "N / Total"
- `#prev-btn`, `#next-btn`, `#fullscreen-btn` — nav controls
- postMessage `play`/`freeze` to each iframe on transition

### Slides (`slide-NN.html`)
Each slide is a self-contained HTML file rendered at **3840×2160px** then scaled to fit the viewport. File numbers are for organization; display order is set by the controller.

---

## Required Slide Boilerplate

Every slide needs `<div class="slide-canvas" id="slide">` as the root, then **two script blocks** at the bottom of `<body>`:

**Rescale script:**
```html
<script>
(function () {
  var s = document.getElementById('slide');
  function rescale() {
    var r = Math.min(window.innerWidth / 3840, window.innerHeight / 2160);
    s.style.transform = 'scale(' + r + ')';
    s.style.left = ((window.innerWidth  - 3840 * r) / 2) + 'px';
    s.style.top  = ((window.innerHeight - 2160 * r) / 2) + 'px';
  }
  rescale();
  window.addEventListener('resize', rescale);
}());
</script>
```

**Freeze/play script (required for transitions):**
```html
<script>
(function(){
  if(window.parent===window)return;
  var fs=document.createElement("style");
  fs.id="deck-freeze";
  fs.textContent="[class*=anim-]{animation:none!important;opacity:0!important}";
  document.head.appendChild(fs);
  window.addEventListener("message",function(e){
    if(e.data==="play"){
      var f=document.getElementById("deck-freeze");
      if(f)f.remove();
      document.querySelectorAll("[class*=anim-]").forEach(function(el){
        el.style.animation="none";
        void el.offsetHeight;
        el.style.animation="";
      });
    }else if(e.data==="freeze"){
      if(!document.getElementById("deck-freeze")){
        var s=document.createElement("style");
        s.id="deck-freeze";
        s.textContent="[class*=anim-]{animation:none!important;opacity:0!important}";
        document.head.appendChild(s);
      }
    }
  });
})();
</script>
```

---

## Design System

### CPS Theme (`cps-theme.css`)

**Brand colors:**
- `--cp-red: #C31F2F` — primary accent
- `--cp-navy: #25388D` — deep blue
- `--cp-dark: #131419` — background
- `--cp-white: #FFFFFF`

**Background decorators (dark slide pattern):**
```html
<div class="cps-tri-navy"></div>
<div class="cps-tri-red"></div>
<div class="cps-bar-bottom"></div>
<!-- Most slides also add this gradient overlay: -->
<div style="position:absolute;inset:0;background:linear-gradient(108deg,rgba(19,20,25,0.98) 0%,rgba(19,20,25,0.92) 55%,rgba(19,20,25,0.55) 72%,transparent 85%);z-index:1;"></div>
```

**Type scale (px at 3840px canvas — looks correct via scale transform):**
`--fs-xs: 44px` / `--fs-sm: 52px` / `--fs-base: 60px` / `--fs-md: 72px` / `--fs-lg: 88px` / `--fs-xl: 112px` / `--fs-2xl: 144px` / `--fs-3xl: 184px` / `--fs-hero: 312px`

**Key classes:** `.t-hero` `.t-h2` `.t-h3` `.t-h4` `.t-eyebrow` `.t-body` `.t-body-sm` `.t-subhead` `.t-label` `.t-muted`
**Layout:** `.slide-inner` `.slide-inner.full-pad` `.flex-row` `.flex-col` `.flex-1` `.gap-1`–`.gap-6`
**Components:** `.divider-red` `.check-icon` + `<svg class="check-svg">` `.avatar-card` `.logo-lockup`

### BWEV Theme (`bwev-theme.css`)

**Brand colors:**
- `--bw-green: #8DC63F` — primary accent
- `--bw-blue: #0072B4` — brand blue
- `--bw-dark-blue: #0b2a43` — background

**Background decorators:**
```html
<div class="bwev-glow-right anim-glow-pulse"></div>
<div class="bwev-glow-bottom"></div>
<div class="bwev-wave-bg anim-wave-drift">
  <svg viewBox="0 0 3840 900" ...><!-- wave paths --></svg>
</div>
<div class="bwev-accent-top"></div>
<div class="bwev-accent-bottom"></div>
```

**Key difference from CPS:** uses `.divider-green`, green accents, ocean-wave aesthetic. `.t-eyebrow` renders green not red.

### Animation Classes (both themes)

| Class | Effect |
|---|---|
| `anim-fade-up` | Fade in + rise |
| `anim-fade-right` | Fade in + slide from left |
| `anim-fade-left` | Fade in + slide from right |
| `anim-scale-in` | Fade in + scale from 80% |
| `anim-glow-pulse` | BWEV: radial glow pulse |
| `anim-wave-drift` | BWEV: slow wave sway |

Delays: `.delay-1` (0.08s) through `.delay-9` (0.88s)

Animations are frozen by default (via the freeze/play postMessage system). They replay each time a slide is navigated to.

---

## Content Assets

All paths relative to the deck folder:

| Asset | Path |
|---|---|
| CPS Logo (white) | `../Content/Logos/Critical-Peake-Logo_FA White.png` |
| CPS Logo (color) | `../Content/Logos/Critical-Peake-Logo_FA.png` |
| BWEV Logo (white) | `../Content/Logos/BlueWhale_LogoCMYK_white.png` |
| Scott Schankweiler | `../Content/headshots/Scott S.png` |
| Zach Hogan | `../Content/headshots/Zach headshot.jpg` |
| Larry Cartee | `../Content/headshots/Larry Cartee Square.png` |
| Jack Wilson | `../Content/headshots/Jack Wilson.jpeg` |

---

## Static Slide Templates

The following slides are nearly identical across all decks — only eyebrow text / client name changes:

| Slide | Title | What changes |
|---|---|---|
| slide-06 | Who's In The Room? | Client abbreviation in prompt pill |
| slide-07 | What's Working / Not? | Nothing (generic) |
| slide-08 | What Makes Us Different | Nothing |
| slide-09 | Testimonials | Nothing |
| slide-10 | Our Services | Eyebrow subtitle |
| slide-13 | What Can We Do To Earn Your Call? | Nothing |
| slide-14 | Let's Stay Connected | Nothing |
| slide-17 | Any Questions? (split) | Nothing |
| slide-18 | Message from Our President | Client abbreviation in eyebrow |

---

## Per-Deck Variables

When creating a new deck, define these values first:

```
CLIENT_NAME:        Full client name (e.g., "Legum & Norman")
CLIENT_SHORT:       Abbreviation for eyebrows (e.g., "L&N")
CLIENT_DIVISION:    Division / subtitle (e.g., "MD & DE Resorts Division")
PROPERTY_TYPE:      What kind of properties (e.g., "resort communities", "retail centers")
DATE:               Presentation date (e.g., "April 9, 2026")
CPS_TEAM:           Array of {name, role, email, headshot_path or initials}
EXISTING_CLIENT:    true/false — have we worked with them before?
CLIENT_LIST:        Array of reference clients for "We've Worked in Your World"
EV_SITUATION:       Known EV charger details (count, issues) — or "unknown"
RAFFLE_PRIZE:       Prize type and quantity (e.g., "3× $100 Amazon Gift Card")
CUSTOM_SLIDES:      Any client-specific slides to add (e.g., EV challenge data)
SLIDES_TO_REMOVE:   Any standard slides to drop
```

---

## Standard Slide Order

Default deck order (adjust per client needs):

1. `slide-01` — Cover
2. `slide-02` — Agenda
3. `slide-03` — Meet Your Team
4. `slide-18` — Message from Our President (video)
5. `slide-04` — We've Worked in Your World
6. `slide-05` — A Perfect Match
7. `slide-06` — Who's In The Room? *(discussion)*
8. `slide-07` — What's Working / Not With Your Vendors? *(discussion)*
9. `slide-13` — What Can We Do To Earn Your Call? *(discussion)*
10. `slide-08` — What Makes Us Different
11. `slide-09` — Testimonials
12. `slide-10` — Our Services
13. `slide-11` — Blue Whale EV
14. *(client-specific EV slides or generic EV education)*
15. `slide-17` — Any Questions?
16. `slide-15` — Raffle Time!
17. `slide-14` — Let's Stay Connected

---

## CPS Company Info (Static)

- **Phone:** 877-247-9767
- **Email:** request@criticalpeake.com
- **Website:** criticalpeake.com
- **HQ:** Hanover, MD (second location: Richmond, VA)
- **Licensed:** MD, VA, DC, DE
- **Experience:** 25+ years
- **Google:** 4.9★ / 35 reviews
- **Differentiators:** Answers within 3 rings 24/7 · same-day/next-day at standard rates · 100% self-performed · transparent pricing · no subcontracting

## Blue Whale EV Info (Static)

- **Website:** bluewhaleev.com
- **Role:** Vendor-agnostic EV charging advisory & installation services
- **Process:** Site Audit → Product Selection → Remove & Replace → Rebates/ROI
- **Featured product:** Xeal (Bluetooth/NFC, no WiFi needed, no monthly fees, tap-to-pay, revenue-ready)

---

## Creating a New Deck

1. `mkdir <client-slug>/`
2. Copy `cps-theme.css` and `bwev-theme.css` into the new folder
3. Copy slides from `federal-reit/` as starting points
4. Fill in the Per-Deck Variables above
5. Adapt each slide to the client (see which are static vs. per-deck above)
6. Create `<client-slug>/<client-slug>-deck.html` controller
7. Update root `index.html` redirect
8. Push — GitHub Pages auto-deploys
