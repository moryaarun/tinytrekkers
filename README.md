# Tiny Trekkers `index.html` — Code Guide

A single-file website (HTML + CSS + JS all in one file). This guide explains what each part does and — more importantly — **where to look when you want to change something**.

---

## 1. Overall structure

```
<head>
  - SEO meta tags
  - Open Graph / Twitter cards
  - Schema.org JSON-LD (structured data for Google)
  - Google Fonts link
  - <style> ... all CSS ... </style>
</head>
<body>
  - WhatsApp floating button
  - Callback floating button + popup
  - Nav bar + mobile menu
  - Hero section
  - Upcoming Events (trek cards)
  - Schedule on Demand
  - Trek Places
  - Gallery
  - About Us
  - Support
  - Terms & Conditions (tabbed)
  - Contact
  - Footer
  - Booking Modal (3-step booking flow + Razorpay)
  - <script> ... all JavaScript ... </script>
</body>
```

Everything lives in one file, so CSS is in the `<head>` (lines 46–436) and JavaScript is at the very bottom (lines 1254–1513).

---

## 2. `<head>` section (lines 1–45)

| Lines | What it is | When to touch it |
|---|---|---|
| 6–10 | `<title>`, meta description, keywords, author, robots | Update if you rename the business or want different SEO keywords |
| 11 | Canonical URL | Change if the domain changes |
| 12–21 | Open Graph & Twitter card tags | Controls the preview when the link is shared on WhatsApp/Facebook/Twitter |
| 22 | Geo meta tag | Location for local SEO |
| 23–24 | Google Fonts (`Fraunces` for headings, `Nunito` for body text) | Change fonts here |
| 25–45 | JSON-LD schema (`TravelAgency`) — name, description, address, phone, Instagram link | **Update this whenever your address/phone/Instagram changes** — Google reads this for search snippets |

---

## 3. CSS (lines 46–436)

### 3.1 Design tokens (lines 47–52)
```css
:root{
  --forest:#1B2F1A; --moss:#2D4829; --sage:#4E7249; --fern:#6B9464;
  --mist:#B5CEAD; --cream:#F6F1E7; --parch:#EDE3CC; --amber:#C07A28;
  --gold:#DFA53A; --stone:#7A6347; --bark:#4A3728; --white:#FDFAF4;
  --text:#1B2F1A; --sub:#5A6B52;
}
```
**This is the entire color palette.** Every color used across the site is one of these variables. To re-theme the whole site (e.g. change the primary green or the amber accent), edit values here instead of hunting through the CSS.

> ⚠️ Note: the booking modal CSS (from ~line 360 onward) uses **hardcoded hex codes** instead of these variables (e.g. `#1B2F1A` typed directly). If you change a color in `:root`, also search-and-replace the matching hex code in the modal styles to keep them in sync.

### 3.2 Global resets (lines 53–58)
- `*{box-sizing:border-box;margin:0;padding:0}` — removes default browser spacing
- `html{scroll-behavior:smooth}` — smooth scroll for the `#anchor` nav links
- `body{...}` — base font, background, text color, size
- Scrollbar styling for WebKit browsers

### 3.3 Navigation (lines 61–75)
- `nav` — the sticky dark-green top bar
- `.brand` — "TinyTrekkers" logo text
- `.nav-links` — desktop menu items
- `.nav-wa` — the amber "WhatsApp Us" pill button in the nav
- `.hamburger` / `.mob-menu` — mobile menu (shown only on small screens, toggled by JS)

### 3.4 Hero (lines 78–94)
- `.hero` — the big dark green banner at the top
- `.hero::before` and `.hero-dots` — decorative background gradients/dots (pure CSS, no images)
- `.hero-title`, `.hero-sub` — headline and subtext
- `.btn-hero`, `.btn-forest`, `.btn-ghost` — the two call-to-action buttons
- `.hero-chips` — the "500+ Trekkers / 18+ Routes / 4.9★ / 0 Safety Issues" stat row

### 3.5 Section commons (lines 96–107)
Reusable classes used by **every** content section:
- `.section` — standard padding wrapper
- `.section-alt` — cream background variant
- `.section-dark` — dark green background variant
- `.sec-label`, `.sec-title`, `.sec-sub` — the small uppercase label / big heading / paragraph pattern repeated at the top of each section
- `.inner` — centers content with a 1060px max width

There are two comments here (lines 97, 105) noting padding was previously reduced — safe to adjust further if you want more/less breathing room between sections.

### 3.6 Component-specific styles (lines 109–436)
Each block is labeled with a comment header like `/* ── EVENTS ── */`. Roughly:

| Comment header | Lines | Styles for |
|---|---|---|
| UPCOMING EVENTS | 109–142 | Trek cards (`.event-card`, `.event-header`, `.event-diff`, `.event-price`, etc.) |
| ADVANCE PAYMENT BOX | 126–133 | The "Pay Just ₹99" highlighted box inside each event card |
| ON DEMAND | 144–157 | The "Schedule on Demand" trek cards |
| TREK PLACES | 159–172 | The list-style trek destination items |
| INQUIRY | 174–193 | The enquiry form section |
| ABOUT | 195–210 | About Us layout, stat cards |
| SUPPORT | 212–222 | Support contact cards |
| T&C | 224–240 | Tabbed terms & conditions |
| (truncated section ~242–393) | | Gallery, contact, footer, callback popup, booking modal overlay styles |
| Booking modal — page 2/3 fields | 394–423 | Form inputs, phone row, payment method buttons, Razorpay button |
| Nav buttons / success screens | 424–435 | Back/Next buttons, success confirmation styling |

**To restyle a specific component**, search for its comment header (the `/* ── NAME ── */` lines) and edit within that block only.

---

## 4. Body content (lines 438–1250)

### 4.1 Floating buttons (lines 441–450)
- `.wa-float` — floating WhatsApp button (bottom corner)
- `.cb-float` — floating "Request Callback" button

### 4.2 Callback popup (lines 452–510)
A simple modal with a form (name, phone, trek, date, group size) that, on submit, builds a WhatsApp message and opens `wa.me` with it pre-filled (see `submitCallback()` in JS).

### 4.3 Nav + mobile menu (lines 512–542)
Plain anchor links (`href="#events"` etc.) to each section's `id`. **If you rename a section's `id`, update the matching `href` here too**, in both the desktop nav and the mobile menu.

### 4.4 Hero (lines 544–560)
Headline, subtext, two buttons, and the stat chips. Edit the numbers here (500+, 18+, 4.9★, etc.) directly in the HTML text.

### 4.5 Upcoming Events (lines 567–770ish)
Each trek is a `.event-card` block, structured like:
```html
<div class="event-card">
  <div class="event-header easy"></div>      <!-- colored strip: easy/moderate/hard -->
  <div class="event-body">
    <div class="event-diff d-easy">...Easy</div>
    <div class="event-name">Vetal Tekadi Trek</div>
    <div class="event-meta">... location, date, time, group size ...</div>
    <div class="advance-box">... "Pay Just ₹99" ...</div>
    <div class="event-footer">
      <div class="event-price">₹499 <span>/ person</span></div>
      <button onclick="openBookingModal('Trek Name','Date','Location',99,499)">Book Seat</button>
      <a href="https://wa.me/...">Full Booking</a>
    </div>
  </div>
</div>
```
**To add a new trek event**, copy one whole `.event-card` block and change:
- The name, date, location, difficulty class (`d-easy` / `d-moderate` / `d-hard`, and `event-header` class `easy`/`hard`/none)
- The `advance-box` price
- The final price and slots-left text
- The `openBookingModal(...)` arguments — **this is what actually drives the booking modal**, so keep the 5 arguments (trek name, date, location, advance ₹, full ₹) in sync with what's displayed

### 4.6 Gallery (lines 775–802)
Emoji-based placeholder cards (no real images) plus an Instagram follow CTA. Replace `.gallery-card` content with real `<img>` tags if you want actual photos instead of emoji icons.

### 4.7 About (lines 807–832)
Two-column layout: story text on the left (`.about-text`), stat cards on the right (`.about-stats`).

### 4.8 Support (lines 834–875)
Four cards: Phone, WhatsApp, Email, Support Hours. Email is obfuscated with Cloudflare's email-protection encoding (the `__cf_email__` spans) — this is normal, it's anti-spam obfuscation, not broken code.

### 4.9 Terms & Conditions (lines 877–973)
Tab-based: four tabs (Refund, Agreement, Privacy, Consent), each is a `.tnc-panel`. JS function `showTnc(id, el)` toggles which panel is visible.

### 4.10 Contact (lines 975–1038)
Address, phone, email, WhatsApp, plus a static "map card" (not an actual embedded Google Map — just a styled placeholder with a link out to Google Maps).

### 4.11 Footer (lines 1040–1084)
Brand blurb, quick links, policy links, copyright line. Update the `© 2025` year and any links here.

### 4.12 Booking Modal (lines 1086–1249)
The 3-step booking flow, shown as an overlay:
- **Step 1** (`#bk-page-1`): trek info recap + Terms checkbox
- **Step 2** (`#bk-page-2`, lines ~1122–1221 truncated above): parent/kid details form (name, email, phone, age, address, emergency contact) + disclaimer checkbox
- **Step 3** (`#bk-page-3`): order summary + payment method buttons (UPI/Card/Net Banking) + the Razorpay "Pay Now" button
- **Success screen** (`#bk-success`): shown after payment, displays a generated reference number

---

## 5. JavaScript (lines 1254–1513)

| Function | Lines | What it does |
|---|---|---|
| `toggleMenu()` | 1256–1258 | Opens/closes mobile nav menu |
| (click-outside listener) | 1259–1265 | Closes mobile menu if you click outside it |
| `showTnc(id, el)` | 1268–1278 | Switches the active Terms & Conditions tab |
| `openCallback()` / `closeCallback()` / `closeCbOnOverlay()` | 1281–1295 | Opens/closes the callback popup |
| `submitCallback()` | 1296–1313 | Validates the callback form, builds a WhatsApp message, opens `wa.me` with it, shows success state |
| `openBookingModal(trek, date, loc, advance, full)` | 1318–1337 | **Entry point for booking** — called from each event card's button. Stores trek details in global variables (`_bkTrek`, `_bkDate`, etc.), resets the modal to step 1, and opens it |
| `closeBookingModal()` / `bkOverlayClick()` | 1339–1345 | Closes the booking modal |
| `bkGoStep(n, init)` | 1347–1394 | Moves between steps 1→2→3. Runs form validation before advancing (e.g. checks T&C checkbox before step 2, checks all required fields before step 3) |
| `selectPayMethod(method)` | 1396–1401 | Highlights the chosen payment method button (UPI/Card/Net Banking) |
| `launchRazorpay()` | 1403–1471 | **The payment trigger.** If a real Razorpay key is set, opens the Razorpay checkout popup. Otherwise falls back to pre-made payment links, or (if nothing is configured) shows an alert and opens WhatsApp instead |
| `buildOwnerMessage(payId)` | 1473–1485 | Builds the WhatsApp message text sent to the business owner with all booking details |
| `sendOwnerWhatsApp(payId)` | 1487–1490 | Opens WhatsApp with the owner message pre-filled |
| `showBookingSuccess(payId)` | 1492–1498 | Displays the success screen with a random reference number |
| Active-nav-on-scroll listener | 1500–1512 | Highlights the current section's nav link as you scroll down the page |

### ⚠️ Important: Razorpay is NOT yet configured
Look at lines ~1403–1471. Right now:
```js
const RAZORPAY_KEY = 'YOUR_RAZORPAY_KEY_ID'
```
This is a placeholder. **Until you replace it with a real key** (from Razorpay Dashboard → Settings → API Keys), and replace the two payment link URLs:
```js
const paymentLinks = {
  99:  'https://rzp.io/l/YOUR_VETAL_TEKADI_LINK',
  149: 'https://rzp.io/l/YOUR_MALHARGAD_LINK'
}
```
online payment won't actually work — clicking "Pay" will just show an alert and redirect to WhatsApp instead. This is the #1 thing to fix before going live if you want real online payments.

---

## 6. Quick "how do I…" cheat sheet

| I want to... | Go to... |
|---|---|
| Change the phone/WhatsApp number everywhere | Search-and-replace `918310441503` (appears ~15+ times across nav, hero, floats, footer, modal, JS) |
| Change the color scheme | Edit the `:root` variables (lines 47–52), then check hardcoded hex codes in booking modal CSS (~360+) |
| Add a new trek to Upcoming Events | Copy a `.event-card` block (~lines 576–770) and edit text + `openBookingModal(...)` args |
| Add a trek to "Schedule on Demand" | Find the `.demand-card` blocks (search `on-demand` section) |
| Edit Terms & Conditions text | Find `#tnc-refund`, `#tnc-agreement`, `#tnc-privacy`, `#tnc-consent` panels (~890–970) |
| Turn on real Razorpay payments | Edit `RAZORPAY_KEY` and `paymentLinks` in `launchRazorpay()` (~1403–1458) |
| Change the address/map link | Contact section (~980–1034) and JSON-LD schema (~33–40) |
| Add/remove a nav menu item | Edit both `.nav-links` (~515–525) and `.mob-menu` (~531–542) — and add a matching `id="..."` on the target section |
