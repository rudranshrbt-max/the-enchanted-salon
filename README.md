# The Enchanted Salon — Website Documentation

**File:** `the-enchanted-salon.html`  
**Type:** Single-file static website  
**Stack:** HTML · CSS · Vanilla JS · Three.js r128  
**Business:** The Enchanted Salon, 20B NEMA COMPLEX, Indrapuri, Bhopal 462022  
**Contact:** 074150 28439

---

## Quick Start

No build step. No dependencies to install. Open the file directly in a browser or upload it to any web host.

```bash
# Local preview
open the-enchanted-salon.html

# Or serve with any static server
npx serve .
python3 -m http.server 8080
```

---

## File Structure

Everything lives in one file: `the-enchanted-salon.html`

```
the-enchanted-salon.html
├── <head>          → SEO meta, OG tags, Google Fonts, Three.js CDN
├── <style>         → All CSS (CSS variables, layout, animations, responsive)
├── <body>
│   ├── Scroll progress bar
│   ├── Custom cursor
│   ├── Nav (sticky, scrolled state)
│   ├── Mobile menu
│   ├── Hero section (Three.js canvas)
│   ├── Marquee strip
│   ├── About section
│   ├── Services section
│   ├── Testimonials section
│   ├── Booking section + time slot grid
│   ├── Contact section + map embed
│   ├── Footer
│   ├── Floating buttons (AI chat, WhatsApp, Call)
│   ├── Booking confirmation modal
│   └── Admin dashboard panel
└── <script>        → Three.js scene, cursor, booking logic, AI chatbot, admin
```

---

## Sections

| Section | ID | Description |
|---|---|---|
| Hero | `#hero` | Full-screen 3D background, tagline, CTAs |
| Marquee | — | Animated service ticker |
| About | `#about` | Story, stats, decorative frame |
| Services | `#services` | 6 service cards with pricing |
| Testimonials | `#testimonials` | Horizontal scroll, 4 dummy reviews |
| Booking | `#booking` | Form + real-time slot grid |
| Contact | `#contact` | Info, map embed, action buttons |
| Footer | — | Links, social, hours, admin trigger |

---

## Features

### 3D Hero Background
Built with Three.js r128. Renders floating geometric shapes (octahedrons, tetrahedrons, icosahedrons) in gold metallic and wireframe materials. Reacts to mouse movement via camera lerp. Particle field of 200 points overlaid. Automatically resizes on window resize.

**To disable on mobile** (already limited by `cursor: auto` override — Three.js still runs):
```js
// Add inside the animate() function
if (window.innerWidth < 768) { renderer.setPixelRatio(1); }
```

### Booking System
- Time slots rendered from a hardcoded array of 16 slots (10:00–17:30, 30-min intervals)
- Three slots pre-marked as booked for demo realism
- On date change, checks `bookedSlots` object and marks conflicts
- On submit, saves booking to `localStorage` under key `enchanted_bookings`
- Fires confirmation modal with client's booking summary
- Admin dashboard reads from the same `localStorage`

**Current limitation:** `localStorage` is per-device. Two different users on two different devices won't see each other's bookings. See [Backend Upgrade](#backend-upgrade) section.

### Admin Dashboard
Triggered by clicking `⚙ Admin` in the footer. No password protection in this version.

Shows:
- Total bookings today
- Confirmed count
- Pending count
- Full table of all bookings with click-to-call phone numbers

**To add a password:**
```js
function toggleAdmin() {
  const pass = prompt('Enter admin password:');
  if (pass !== 'YOUR_PASSWORD') return;
  // rest of toggle logic
}
```

### AI Chatbot
Keyword-matching assistant. Recognises intent for: booking, appointment, price, service, location, hours, call, WhatsApp, bridal, keratin, hello, thank you. Falls back to a call-us prompt for anything unrecognised.

**Not a real AI.** It does not use an API. It does not attend calls, emails, or SMS. See [Backend Upgrade](#backend-upgrade) for that.

Quick-action buttons: Book · Services · Pricing · Location

### Floating UI
| Element | Position | Action |
|---|---|---|
| AI Chat button | Bottom right | Toggles chat panel |
| WhatsApp button | Bottom right (below AI) | Opens wa.me link with pre-filled message |
| Call button | Bottom left | `tel:` link, click-to-call |

---

## Customisation

### Business Details
All contact info is hardcoded. Search and replace:

| Find | Replace with |
|---|---|
| `07415028439` | Your phone number |
| `917415028439` | Your WhatsApp number (country code + number) |
| `https://maps.app.goo.gl/VHo4XZzuUCERJ9ZN7` | Your Google Maps link |
| `20B NEMA COMPLEX...` | Your address |

### Google Maps Embed
The current map iframe uses a placeholder coordinate. Replace the `src` with your actual embed URL:

1. Go to [maps.google.com](https://maps.google.com)
2. Search your business
3. Click Share → Embed a map → Copy HTML
4. Paste the `src` value into the iframe in the `#contact` section

### Colors
All colors are CSS variables in `:root`. Change these to rebrand completely:

```css
:root {
  --gold: #C9A96E;        /* Primary accent */
  --gold-light: #E8D5A8;  /* Hover state */
  --gold-dark: #8B6914;   /* Deep accent */
  --obsidian: #0A0A0A;    /* Primary background */
  --obsidian-mid: #111111;
  --obsidian-soft: #1A1A1A;
  --cream: #F5F0E8;       /* Primary text */
  --text-muted: #9A8F82;  /* Secondary text */
}
```

### Fonts
Loaded from Google Fonts:
- `Cormorant Garamond` — headings, logo, display text
- `Jost` — body, UI, buttons

Change in the `<link>` tag in `<head>` and update `font-family` references in CSS.

### Services
Edit the service cards in the `#services` section. Each card follows this structure:

```html
<div class="service-card reveal">
  <span class="service-icon">✂</span>
  <h3 class="service-name">Service Name</h3>
  <p class="service-desc">Description text.</p>
  <div class="service-price">From ₹XXX · Duration</div>
</div>
```

Also update the `<select>` dropdown in the booking form to match.

### Testimonials
Replace dummy content in the `#testimonials` section. Each card:

```html
<div class="testimonial-card">
  <div class="testimonial-quote">"</div>
  <p class="testimonial-text">Review text here.</p>
  <div class="testimonial-author">
    <div class="author-avatar">A</div>  <!-- First initial -->
    <div>
      <div class="author-name">Client Name</div>
      <div class="author-role">Context / Date</div>
      <div class="stars">★★★★★</div>
    </div>
  </div>
</div>
```

### Adding Real Photos
The about section has a placeholder div. Replace it with an `<img>`:

```html
<!-- Find this: -->
<div class="about-img-placeholder"> ... </div>

<!-- Replace with: -->
<img src="your-salon-photo.jpg" alt="The Enchanted Salon interior" 
     style="width:100%;height:100%;object-fit:cover;">
```

---

## SEO

Meta tags already set in `<head>`:
- `<title>` — includes business name, category, city
- `<meta name="description">` — compelling, keyword-rich
- `<meta name="keywords">` — local SEO terms
- `<meta property="og:title">` and `og:description` — social sharing

**To improve further:**
- Add `og:image` with a high-quality salon photo (1200×630px recommended)
- Add `<link rel="canonical" href="https://yourdomain.com">`
- Submit to Google Search Console once deployed
- Register and optimise Google Business Profile at [business.google.com](https://business.google.com)

---

## Deployment

### Option 1 — Free (Netlify)
1. Go to [netlify.com](https://netlify.com)
2. Drag and drop the HTML file onto the deploy zone
3. Get a live URL instantly (e.g. `enchanted-salon.netlify.app`)
4. Add a custom domain in settings

### Option 2 — Free (GitHub Pages)
```bash
git init
git add the-enchanted-salon.html
git commit -m "initial"
git push origin main
# Enable Pages in repo Settings → Pages → main branch
```
Rename file to `index.html` first.

### Option 3 — Paid Hosting (cPanel)
Upload `the-enchanted-salon.html` renamed as `index.html` to the `public_html` folder via File Manager or FTP.

---

## Backend Upgrade

The current build is frontend-only. To unlock real functionality:

### Real Booking Storage
Replace `localStorage` with a database call:

```js
// Replace the localStorage block in submitBooking() with:
async function submitBooking() {
  const res = await fetch('/api/bookings', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name, phone, service, date, time: selectedTime })
  });
  const data = await res.json();
  if (data.success) { /* show modal */ }
}
```

Recommended: Firebase Firestore (free tier), Supabase, or a Node/Express + MongoDB backend.

### SMS/WhatsApp Confirmations
Use [MSG91](https://msg91.com) (India-based, affordable) or Twilio:

```js
// Server-side (Node.js)
const msg91 = require('msg91');
await msg91.sendSMS({
  to: booking.phone,
  message: `Hi ${booking.name}, your appointment at The Enchanted is confirmed for ${booking.date} at ${booking.time}. See you soon!`
});
```

### Real AI Chatbot
Replace the keyword matcher with Claude API:

```js
async function getAIResponse(msg) {
  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': 'YOUR_KEY', // server-side only, never in frontend
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 300,
      system: 'You are the AI assistant for The Enchanted Salon in Bhopal. Answer questions about services, pricing, booking, and location. Be warm, concise, and professional.',
      messages: [{ role: 'user', content: msg }]
    })
  });
  const data = await res.json();
  return data.content[0].text;
}
```

**Never expose your API key in frontend code.** Route this through a server endpoint.

### Admin Password Protection
For production, replace the frontend admin panel with a proper authenticated dashboard (Next.js + Auth.js, or a simple password-protected route).

---

## Known Limitations

| Limitation | Impact | Fix |
|---|---|---|
| `localStorage` for bookings | Per-device only, no shared state | Firebase / server DB |
| No real SMS confirmation | Manual follow-up needed | MSG91 / Twilio |
| AI is keyword-matching only | Can't handle complex queries | Claude API integration |
| Admin has no password | Anyone can open it | Auth layer |
| Map embed is placeholder coordinates | May show wrong location | Replace iframe src |
| No real photos | Generic placeholder | Add actual salon images |

---

## Browser Support

| Browser | Support |
|---|---|
| Chrome 90+ | ✓ Full |
| Firefox 88+ | ✓ Full |
| Safari 14+ | ✓ Full |
| Edge 90+ | ✓ Full |
| Mobile Chrome/Safari | ✓ Full (cursor disabled on touch) |

Three.js requires WebGL. All modern browsers support it. Falls back gracefully — the rest of the page functions without the 3D hero.

---

## Performance Notes

- Three.js scene is the heaviest asset. Pixel ratio capped at 2 via `Math.min(devicePixelRatio, 2)`
- Google Fonts loaded with `preconnect` for faster DNS resolution
- Scroll-reveal uses IntersectionObserver-style threshold (`getBoundingClientRect`) — no library needed
- No images loaded by default — zero image HTTP requests in base state
- `localStorage` reads/writes are synchronous — keep booking data small

---

## License

Built for The Enchanted Salon, Bhopal. All rights reserved.  
Not for redistribution without permission.
