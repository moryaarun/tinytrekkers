The Big Picture
Your file is a single-page website for Tiny Trekkers, Pune. Everything — the design, content, and logic — lives in one file. It has three layers:

Laye             What it is                                             Where it lives
HTML    The actual content (text, buttons, sections)                 The <body> tag
CSS     How everything looks (colors, fonts, layout)                 The <style> block in <head>
Java    ScriptInteractive behavior (booking form, scroll effects)  The <script> block at the  bottom
===================================================================================================
PART 1 — Colors & Fonts (Lines 10–18)

css
:root {
  --forest:#1B2F1A;  --moss:#2D4829;  --sage:#4E7249;
  --amber:#C07A28;   --gold:#DFA53A;  --cream:#F6F1E7;
  ...
}
```

Think of `:root` as a **central settings panel** for all your colors. Instead of memorizing hex codes, the whole site uses names like `--amber` or `--forest`. **To change the site's color theme, you only edit here** — everything updates automatically.

**To change a color:** Replace the hex code (e.g., `#C07A28`) with a new one. You can pick colors from [coolors.co](https://coolors.co).

**Fonts used:**
- `Fraunces` — the elegant serif font for headings
- `Nunito` — the clean sans-serif for body text

Both are loaded from Google Fonts (line 8). To change fonts, swap the font names in the Google Fonts URL and in `font-family:` declarations.

---

## 🔝 PART 2 — Navigation Bar (Lines 23–38 CSS, ~line 420 HTML)

The nav bar is **sticky** (stays at the top as you scroll). It has three parts:

1. **`.brand`** — the "Tiny Trekkers" logo text on the left
2. **`.nav-links`** — the desktop menu links (Events, On Demand, etc.)
3. **`.nav-wa`** — the amber "WhatsApp Us" button on the right
4. **`.hamburger`** — the ☰ icon that appears on mobile
5. **`.mob-menu`** — the dropdown menu that opens on mobile

**To add a new nav link**, find the `<ul class="nav-links">` in the HTML and copy-paste one `<li>` item, updating the text and the `href="#section-id"`.

---

## 🏔️ PART 3 — Hero Section (Lines 41–57 CSS, ~line 440 HTML)

This is the **big banner at the top** of the page with the title, subtitle, and two buttons. Key parts:

- **`.hero-title`** — the main large heading. Change the text inside `<h1 class="hero-title">` to update it.
- **`.hero-sub`** — the smaller subtitle paragraph below the heading.
- **`.btn-hero`** — the two call-to-action buttons. The amber one links to `#events`, the ghost one opens WhatsApp.
- **`.hero-chips`** — the three stats at the bottom (e.g., "40+ Treks Done"). Find these `<div class="chip">` blocks in the HTML and update the numbers.
- **`::before` decorative gradients** — subtle background glows. You can ignore these safely.

---

## 📅 PART 4 — Upcoming Events Cards (Lines 73–105 CSS, ~line 470 HTML)

Each trek event is an **`.event-card`**. Here's the anatomy of one card:
```
[ colored top bar ]     ← .event-header (green = easy, brown = hard)
[ Difficulty label ]    ← .event-diff
[ Trek Name ]           ← .event-name
[ Date / Distance / Age ] ← .event-meta (.em-row items)
[ Advance Payment box ] ← .advance-box  (shows ₹99 advance, strikethrough full price)
[ Price | Book Button ] ← .event-footer

=============================================================================
To add a new event: 
Copy the entire <div class="event-card">...</div> block and paste it inside the <div class="events-grid">. Then update:

The trek name, date, distance, age group
The price and advance amount
The WhatsApp link in the href of the green button (the text after ?text= is the pre-filled WhatsApp message)

To change difficulty color on the top bar, use class easy or hard on .event-header:
html
<div class="event-header easy">    ← green bar
<div class="event-header hard">    ← brown bar

=================================================================================
PART 5 — On Demand Treks (Lines 107–120 CSS, ~line 590 HTML)
These are the private/group trek cards (Vasota, Lohgad, etc.). Each card (.demand-card) has:

- An emoji icon, trek name, region, tags, a short description
- A "Request Date" button linking directly to a WhatsApp message

To add a new on-demand trek: 
Copy a <div class="demand-card"> block and update the emoji, name, location, tags, description, and the href WhatsApp link.

===================================================================================
PART 6 — Trek Destinations List (Lines 122–135 CSS, ~line 690 HTML)
A compact grid showing all the places you trek to. Each row is a .place-item. The left border color indicates difficulty:

Green border = easy (default)
Amber border = hard (add class hard-place to the div)

To add a destination: 
Copy one <div class="place-item"> and update the emoji, name, location, and the tag badges (.pi-tag with classes t-easy, t-mod, t-hard, or t-pop).

====================================================================================
PART 7 — Inquiry / Contact Form (Lines 137–156 CSS, ~line 740 HTML)

A two-column layout:
- Left side — persuasive text and bullet points (.inq-perks)
- Right side — the actual form (.inquiry-form) with fields for name, phone, trek interest, and dates

The form submits via JavaScript (look for handleInquiry() in the <script> block). When submitted it opens a pre-filled WhatsApp message — so no backend/server is needed.

To add a new form field: Copy a <div class="form-group"> block inside the form, give the input a unique id, and reference that id in the handleInquiry() function in the JavaScript.

========================================================================
PART 8 — About Section (Lines 158–173 CSS, ~line 800 HTML)

Dark green background (section-dark), two columns:

Left — description text and 4 highlight boxes (safety, guides, etc.)
Right — 4 stat cards (treks done, kids, years, rating)

To update stats: Find <div class="as-num"> and change the number inside.
===============================================================================
PART 9 — Support & Contact (Lines 175–230 CSS, ~line 880 HTML)

Support section: Four cards showing phone, email, WhatsApp, and timings. 
Each .support-card has an icon, label, value, and a note. Just update the text inside.

Contact section: Two-column layout with contact details on the left and a map placeholder on the right. The "Open in Google Maps" link can be updated by changing the href of .map-btn.

=============================================================
PART 10 — Terms & Conditions (Lines 187–203 CSS, ~line 950 HTML)

A tabbed panel with 3 tabs: General, Safety, and Cancellation. Each tab corresponds to a .tnc-panel. The switchTnc() JavaScript function handles showing/hiding panels when you click a tab.

To edit T&C text: Find the relevant .tnc-panel and edit the <p> and <li> elements inside.

====================================================================
PART 11 — Booking Modal / Popup (Lines ~400–600 CSS, ~line 1050 HTML)

This is the 3-step booking popup that appears when you click "Book Seat":

Step 1 — Kid's name, age, parent name, phone, email
Step 2 — Address, emergency contact, and health note
Step 3 — Payment method selection (UPI / Card / Net Banking)

The flow is controlled by bookingStep(n) in JavaScript. On the final step, launchRazorpay() is called.

 Most Important Thing to Change Here:

 javascript
 const RAZORPAY_KEY = 'YOUR_RAZORPAY_KEY_ID'  // ← Replace this!

 And below it, update the payment link URLs:

 javascript

99:  'https://rzp.io/l/YOUR_VETAL_TEKADI_LINK',   // ← Replace
149: 'https://rzp.io/l/YOUR_MALHARGAD_LINK'        // ← Replace

Until you set these up, the fallback sends the customer directly to WhatsApp — so it works even without Razorpay.

======================================================================================
PART 12 — JavaScript Functions (Bottom of file)

Function                                         What it does  
openBooking(trek, date, price, advance)          Opens the booking popup and fills in trek details
closeBooking()                                   Closes the popup
bookingStep(n)                                   Moves between step 1 → 2 → 3 in the form
selectPayMethod(method)                          Highlights UPI/Card/NetBanking selection
launchRazorpay()                                 Initiates payment via Razorpay or falls back to WhatsApp
buildOwnerMessage(payId)                         Builds the WhatsApp message sent to you after booking
sendOwnerWhatsApp(payId)                         Opens WhatsApp with that message
showBookingSuccess(payId)                        Shows the "Booking Confirmed" screen
handleInquiry()                                  Handles the inquiry form submission
switchTnc(tab)                                   Switches between T&C tabs
Scroll listener at bottom                        Highlights the active nav link as you scroll

📱 PART 13 — Mobile Responsiveness
Near the bottom of the CSS you'll find @media (max-width: 768px) blocks. 
These override styles for small screens — stacking columns, hiding desktop nav, showing the hamburger menu, etc. 
Don't delete these or the mobile layout will break.

✅ Quick Reference: Most Common Changes
What you want to change                     Where to go
Site colors                                 :root { } at top of CSS
Business name / phone / email               Search the HTML for "Tiny Trekkers" / "+91 831"
Add/edit a trek event                       Copy an .event-card div in the Events section
Add an on-demand trek                       Copy a .demand-card div
Update stats (treks done, kids, etc.)       Find .chip-num (hero) or .as-num (about)
Change WhatsApp number                      Search for 918310441503 and replace all instances
Enable Razorpay payments                    Replace YOUR_RAZORPAY_KEY_ID in the JS
Edit T&C text                               Find the .tnc-panel divs
Update Google Maps link                     Find .map-btn and update href Sonnet 4.6Extended




