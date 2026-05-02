# Romantic Picnic Invitation — Agent Instructions

## Project Overview

Single self-contained HTML file (`invitacion_picnic_romantica.html`) — a romantic picnic invitation in **Spanish** with an interactive RSVP form. No build tools, no dependencies, no server.

## Key Facts

- **Language**: Spanish throughout (text, variable names in JS, and comments)
- **Fonts**: `Cormorant Garamond` (serif, headings/body copy) and `Lato` (sans-serif, small UI elements) — loaded via Google Fonts CDN
- **Color palette**: rose-pink (`#e8849e`, `#c96a88`, `#b05578`) and lavender-purple (`#c0a0e0`, `#b090d0`)
- **Mobile breakpoint**: 520px (`@media (max-width: 520px)`)
- **Event details hardcoded**: May 3, 3:30–4:00 pm, Parque detrás de San Antonio, Cali

## Architecture

Everything lives in one file in this order:

1. `<style>` — all CSS (reset → layout → components → animations → responsive)
2. `<body>` — background SVG flowers, falling-petal container `#petals-container`, two modals (`#modal-no`, `#modal-confirm`), main `.container`
3. `<script>` — all JS at the bottom (no external libs)

## Interaction Flow

```
User clicks "Sí" → handleSi() → shows #modal-confirm + .si-section + startPetals()
User clicks "No" → handleNo() → shows #modal-no
  → "Acepto desde modal" → acceptFromModal() → handleSi()
  → "Me quedo con mi decisión" → closeModalNo()
User submits form → submitForm() → validates food selection → simulated send → #success-msg
```

## EmailJS Integration

The `submitForm()` function has **real EmailJS code commented out**. To enable it:
1. Replace `'YOUR_EMAILJS_PUBLIC_KEY'` with the actual public key
2. Replace `'tu_correo@gmail.com'` with the destination email
3. Uncomment the `fetch` block and delete the `setTimeout` simulation above it
4. Load the EmailJS SDK: add `<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>` in `<head>`

## Conventions

- CSS class names use kebab-case (`.invitation-card`, `.choice-btn`, `.si-section`)
- JS function names use camelCase (`handleSi`, `selectFood`, `startPetals`)
- IDs match their purpose: `#btn-si`, `#btn-no`, `#si-section`, `#modal-no`, `#modal-confirm`, `#success-msg`, `#sending-state`
- Animations are CSS keyframes (`fadeInUp`, `modalIn`, `fall`, `spin`); JS only triggers them by toggling `.visible` / `.active` classes
- Decorative SVG flowers are inline (not external files)

## Common Tasks

- **Change event date/time/location**: update the hardcoded text in `.invitation-text`, `.location-pill`, and the `#modal-confirm` text, plus `template_params.date` and `template_params.location` in `submitForm()`
- **Add a food option**: add a `<label class="food-option">` inside `#food-options` and add its `value` key to the `foodLabels` map in `submitForm()`
- **Change color theme**: the primary gradient is `linear-gradient(135deg, #e8849e, #c96a88, #b05578)` used on `.confirm-btn` and `.modal-btn.yes`
- **Adjust petal count/speed**: edit the loop limit (`18`) and duration range (`3 + Math.random() * 4`) inside `startPetals()`
