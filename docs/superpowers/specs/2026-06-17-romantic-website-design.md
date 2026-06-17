# Romantic Website — Design Spec

## Overview
Single-page romantic website for girlfriend. Vanilla HTML/CSS/JS, single file, dark-gold trading-terminal aesthetic.

## Theme
Dark background, gold/rose-gold accents, candlestick-graph visual language reappropriated for romance. Typography: monospace for terminal feel, serif for messages.

## Sections (Scroll Order)

### 1. Password Gate
- Full-screen black overlay, centered input field
- Password: `070725`
- Wrong → input shakes, clears
- Correct → overlay slides up, reveals content

### 2. Hero
- Large greeting text, fade+slide in
- Date reference

### 3. All-Time High Graph
- SVG/Canvas candlestick chart, always trending up
- Marker at 07/07/25 (relationship start) with label
- Ticker bar on top: scrolling "LOVE ▲ +∞"
- Animated draw-in on scroll

### 4. Message Cards
- ~15 heartfelt messages (placeholder Thai text)
- Randomize button — click to shuffle through messages
- Card flip or fade transition

### 5. Heart Animation
- Click/tap anywhere → floating heart particles rise up
- Pure CSS/JS animation

### Music
- YouTube embed (hidden iframe) playing `9uZF7BlUQPE`
- Floating music toggle button (bottom-right)
- Autoplay muted on load (browser policy), user can unmute

## Technical
- Single `index.html` file
- No dependencies, no build step
- Responsive: mobile-first with breakpoints for desktop
- Deploy: Vercel or GitHub Pages

## Out of Scope
- Memory Gallery (photos/videos)
- Mini Game
- Real-time WebSocket (same-screen animation only)
- IoT integration
