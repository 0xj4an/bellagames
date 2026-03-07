# Figuras y Colores - Game Design

## Overview
New educational game for kids (2-6 years) to learn geometric shapes and colors.
Self-contained HTML file (`figuras.html`) following existing project patterns.
All interaction is visual + audio (TTS) - no reading required.

## Architecture
- Single file: `figuras.html` (inline CSS + JS, same as letras.html/mates.html)
- Figures rendered as inline SVG (easy to color, scale, animate with CSS)
- DOM-based quiz game (not Canvas)
- Same audio system: Web Audio API oscillators + TTS (Polly/fallback)
- Background music: `music/fun.mp3`

## Screens
1. **Level Screen** - 5 difficulty cards with keyboard/touch nav
2. **Game Screen** - Random round types with SVG figures

## Levels

| Level | Icon | Shapes | Colors | Round Types | Options |
|-------|------|--------|--------|-------------|---------|
| Figuritas | 🔵 | circulo, cuadrado, triangulo | rojo, azul, amarillo | identify shape | 3 |
| Colores | 🎨 | + estrella, rectangulo | + verde, naranja, morado | + identify color | 3 |
| Formas | 🔶 | + ovalo, corazon, diamante | all 8 | + count sides | 4 |
| Mezcla | ⭐ | all 12 | all 8 | + shape+color combo | 4 |
| Campeon | 🏆 | all 12 | all 8 | + find the different | 5 |

## Shapes (12 total, unlocked progressively)
- **Basic (L1):** circulo, cuadrado, triangulo
- **Intermediate (L2):** + estrella, rectangulo
- **Advanced (L3):** + ovalo, corazon, diamante
- **Expert (L4-5):** + pentagono, hexagono, trapecio, flecha

## Colors (8)
rojo (#EF5350), azul (#42A5F5), amarillo (#FFD54F), verde (#66BB6A),
naranja (#FF8A65), morado (#AB47BC), rosa (#F48FB1), celeste (#4FC3F7)

## Round Types

### 1. Identify Shape (from level 1)
- TTS: "Toca el circulo, [name]!"
- Shows 3-5 different shapes, ALL same color (avoids confusion)
- Child taps the correct shape

### 2. Identify Color (from level 2)
- TTS: "Toca la roja, [name]!"
- Shows 3-5 identical shapes in different colors
- Child taps the one with the correct color

### 3. Count Sides (from level 3)
- TTS: "Cuantos lados tiene, [name]?"
- Shows ONE large shape centered
- Options: number buttons (0, 3, 4, 5, 6)
- Circle = 0, triangle = 3, square/rectangle = 4, pentagon = 5, hexagon = 6

### 4. Shape + Color Combo (from level 4)
- TTS: "Toca el triangulo rojo, [name]!"
- Shows 4-5 shapes with varied forms AND colors
- Only one matches both criteria

### 5. Find the Different (from level 5)
- TTS: "Cual es diferente, [name]?"
- Shows 4 shapes: 3 identical + 1 different (shape OR color)
- Child taps the odd one out

## Input Handling

### Desktop (keyboard + mouse)
- Arrow keys (↑↓←→): navigate between shapes/options
- Enter: confirm selection
- 1-5: quick-jump to level
- Escape: back to levels / hub
- Mouse hover: highlight effect
- Click: select shape

### Mobile (touch)
- Tap: select shape directly
- Large touch targets (min 80px shapes, 44px buttons)
- No zoom (viewport meta)
- Responsive grid: fewer columns on small screens

### Responsive Layout
- Desktop (>600px): shapes in 2x2 or 3x2 grid, larger sizes
- Mobile (<600px): shapes in 2x1 or vertical stack, clamp() sizing

## Visual Design
- Background: gradient (light sky blue to lavender, e.g. #E3F2FD → #E1BEE7)
- SVG shapes: ~120px with white stroke (3px) and soft drop shadow
- Entry animation: staggered `pop` (scale 0→1) per shape
- Correct: green glow + scale 1.1 + floating feedback text
- Wrong: shake + show correct with hint styling
- Paw Patrol: ~30% of rounds show a character image alongside the prompt
- HUD: star counter (top-right), "Niveles" button (top-left)

## Audio
- Same TTS system as other games (Polly Mia + Web Speech fallback)
- Shape names in Spanish: circulo, cuadrado, triangulo, estrella, etc.
- Color names with feminine gender: "la roja", "la azul", "la amarilla"
- Same oscillator sounds: correct (C5-E5-G5), wrong (descending), celebrate (7-note)
- Celebrations personalized with player name

## Hub Integration
- New card #5 in juegos.html: "🔷 Figuras y Colores"
- Description: "Aprende figuras y colores"
- Grid expands to accommodate 5th game

## State
- Stars: cumulative, never resets (same as other games)
- Streak: consecutive correct, resets on wrong
- Level: in-memory selection
- Player name: from sessionStorage `jugadora`
