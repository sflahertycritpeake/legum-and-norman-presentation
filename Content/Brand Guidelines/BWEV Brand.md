---
name: bluewhale-brand
description: |
  Brand style guide for Blue Whale EV - an advisory and service organization for the EV charging community.
  MANDATORY TRIGGERS: Blue Whale, BlueWhale, BWEV, Blue Whale EV, EV charging
  Use this skill when creating any content, designs, documents, or materials for Blue Whale EV.
---

# Blue Whale EV Brand Style Guide

## Brand Overview

| | |
|---|---|
| **Company** | Blue Whale EV |
| **Tagline** | An Advisory and Service Organization to the EV Charging Community |
| **Website** | bluewhaleev.com |
| **Phone** | 855.293.8378 |
| **Audience** | B2B — Property managers, fleet managers, government, municipalities |
| **Positioning** | Advisory + service organization AND turnkey EV charging solutions provider (both are accurate) |

## Brand Voice

- **Professional** — Expert, knowledgeable, B2B focused
- **Innovative** — Tech-forward, modern solutions
- **Sustainable** — Eco-conscious, green energy
- **Trustworthy** — Reliable partner, proven results

## Color Palette (Official — source of truth)

All values sourced from `Blue Whale Branding/Brand Guidelines.md`. These are the only approved colors.

### Official Brand Colors
| Name | Hex | Role |
|------|-----|------|
| Green | `#8DC63F` | Logo EV badge, sustainability accents, primary CTAs, accent stripes |
| Blue | `#0072B4` | Wordmark, primary structural color, headings on light backgrounds |
| Dark Blue | `#0b2a43` | Dark backgrounds, pills, ticker bars, lower thirds |
| Orange | `#F7941D` | High-emphasis CTAs and urgency — use sparingly (max once per graphic) |
| Light Grey | `#EDEDED` | Light surface backgrounds, dividers |

### Supporting Neutrals (working palette)
| Name | Hex | Usage |
|------|-----|-------|
| White | `#ffffff` | Card backgrounds, text on dark |
| Gray 500 | `#757575` | Body text on light surfaces |
| Gray 600 | `#5d5d5d` | Muted/secondary text |

### CSS Variables
```css
:root {
  --bw-green: #8DC63F;
  --bw-blue: #0072B4;
  --bw-dark-blue: #0b2a43;
  --bw-orange: #F7941D;
  --bw-gray-light: #EDEDED;
  --bw-white: #ffffff;
  --bw-gray-500: #757575;
  --bw-gray-600: #5d5d5d;
}
```

### Colors to NEVER use
These were previously in the spec but are NOT official brand colors:
- `#141827` — not official (was a guess at dark background)
- `#334aff` — not official (was a guess at blue)
- `#7cb342` — not official (was a guess at green)
- `#00d084` — not official (teal, scraped from website)
- `#f7f6f6`, `#f0eeee`, `#e4e4e4` — not official grays

## Typography (Official Brand Fonts)

| Role | Font | Notes |
|------|------|-------|
| Headlines / Display | **Paralucent** | System-installed in `~/Library/Fonts/`; 32 variants available |
| Body / Supporting text | **Avenir Next** | System-installed in `/System/Library/Fonts/` |

### Paralucent Weight Reference
| Weight name | CSS `font-weight` |
|---|---|
| Extra Light | 200 |
| Light | 300 |
| Medium | 500 |
| Demi Bold | 600 |
| Bold | 700 |
| Heavy | 800 |

Condensed and Text subfamilies also available:
- `Paralucent Condensed` — all weights above in condensed width
- `Paralucent Text` — Book and Bold, optimized for smaller text sizes

### Font files reference location
`references/fonts/paralucent/` — all 32 Paralucent .otf files
`references/fonts/avenir-next/` — Avenir Next.ttc + Avenir Next Condensed.ttc

### For HTML/Puppeteer graphics
Both fonts are system-installed. Reference by name — no CDN or @font-face needed:
```css
font-family: 'Paralucent', sans-serif;       /* headlines */
font-family: 'Avenir Next', Avenir, sans-serif; /* body */
```
Do NOT load Roboto, Roboto Slab, Open Sans, or Inter — those are not brand fonts.

## Logo

- "BLUE WHALE" wordmark in `#0072B4` (brand blue)
- "EV" badge in `#8DC63F` (brand green) circle
- White version available for use on dark backgrounds (`BlueWhale_LogoCMYK_white.png`)
- Logo files: `Blue Whale Branding/BlueWhale_LogoCMYK.png` and `Blue Whale Branding/BlueWhale_LogoCMYK_white.png`
- Maintain clear space around logo

## Design Principles

### Color Meaning
- **Green** = Sustainability, EV, eco-friendly, brand identity
- **Blue** = Professionalism, trust, primary brand color
- **Dark Blue** = Authority, dark surfaces
- **Orange** = Urgency, emphasis, CTAs — use sparingly

### Color Usage Rules for Graphics
- `bw-green` is the accent stripe color — one structural accent stripe per graphic
- `bw-orange` is maximum one use per graphic, only for genuine urgency/CTA emphasis
- Do NOT mix green and orange as dual accent colors in the same graphic
- Ticker bars: left stripe = `bw-green`; no contrasting right stripe; max 2-3 segments; 60px+ horizontal padding

### Tailwind Config (copy into every graphic HTML)
```html
<script>
  tailwind.config = {
    theme: { extend: {
      colors: {
        'bw-green': '#8DC63F',
        'bw-blue': '#0072B4',
        'bw-dark-blue': '#0b2a43',
        'bw-orange': '#F7941D',
        'bw-gray-light': '#EDEDED',
        'bw-gray': { 500: '#757575', 600: '#5d5d5d' }
      },
      fontFamily: {
        display: ['Paralucent'],
        sans: ['Avenir Next', 'Avenir', 'sans-serif']
      }
    }}
  }
</script>
```

## Industries Served

- Apartments / Multifamily
- Fleet Management
- Government / Municipal
- Office / Commercial
- Parking Facilities
- Hospitality

## Key Messaging

- "An Advisory and Service Organization to the EV Charging Community"
- "Charging as a Service"
- "Turnkey EV Charging Solutions"
- "Email The EV Expert"
- Sourcewell contract partner
