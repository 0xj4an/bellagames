# Juegos de Bela y Penu

Coleccion de 4 juegos educativos para ninos pequenos (2-6 anos), construidos con HTML, CSS y JavaScript puro — cero dependencias. Toda la experiencia se personaliza con el nombre del jugador: voz narrativa, felicitaciones y mensajes adaptados.

## Menu principal (`juegos.html`)

Al abrir la app se muestra la pantalla de seleccion de jugador. Por defecto vienen **Bela** y **Penu**, pero se pueden agregar nuevos jugadores (nombre + color + emoji) o eliminar existentes. Los perfiles se guardan en `localStorage` y el jugador activo en `sessionStorage`.

Despues de elegir jugador se muestra el menu con 4 tarjetas de juego en grid 2×2. Navegacion completa por teclado (flechas, Enter, teclas numericas 1-4) y touch.

`index.html` redirige automaticamente a `juegos.html`.

---

## Juegos

### 1. Carrera de Animalitos (`animalitos.html`)

Carrera de obstaculos en Canvas 2D. Elige tu animal y compite contra 2 rivales controlados por IA.

- **Controles**: ESPACIO o toca la pantalla para saltar
- **14 animales**:
  - 🐎 Estrella · 🐕 Luna · 🐈 Michi · 🐢 Tito · 🐇 Rayo · 🦊 Zorro · 🐖 Rosita · 🦮 Bingo · 🦜 Coco · 🐻 Oso
  - 🐕‍🦺 Chase · 🐕 Marshall · 🐕 Skye · 🐶 Rubble (Paw Patrol)
- **Obstaculos**: vallas, rocas, charcos, troncos — aparecen con indicador "!" de advertencia
- **5 niveles de dificultad**:
  - 🐣 Principiante — 5 vidas, pista corta, IA salta 80%
  - 🐰 Normal — 3 vidas, IA salta 70%
  - 🦌 Dificil — 3 vidas, obstaculos mas frecuentes, IA salta 60%
  - 🐻 Experto — 2 vidas, IA salta 50%
  - 🏆 Campeon — 1 vida, pista larga, IA salta 40%
- **Efectos**: confeti al ganar, particulas de polvo al aterrizar, explosion de escombros al chocar, sacudida de pantalla, parallax en nubes y colinas
- **HUD**: corazones (vidas), posicion, barra de progreso con avatar, pista de salto
- **Fisica**: salto con arco parabolico (gravedad simulada), sombra que escala en el aire

### 2. Hipodromo (`horserace.html`)

Carrera de caballos de cuarto de milla en Canvas 2D. Toca rapido para acelerar tu caballo en 6 carriles.

- **Controles**: toca rapido o presiona ESPACIO repetidamente para galopar
- **11 caballos + personalizados**:
  - Relampago · Estrella · Tornado · Luna · Rayo · Princesa · Trueno · Nube
  - Chase · Marshall · Skye (Paw Patrol, con imagen)
  - **Caballos personalizados**: crea los tuyos con nombre y color (se guardan en localStorage)
- **5 niveles**:
  - 🐴 Paseo — pista corta, IA lenta
  - 🏇 Trote — pista media
  - 💨 Galope — IA competitiva
  - ⚡ Sprint — IA rapida con bursts
  - 🏆 Gran Premio — pista larga, IA agresiva
- **Mecanica**: cada toque suma velocidad con decay temporal; IA con bursts aleatorios y comportamiento de manada (se ajustan al grupo)
- **Efectos**: particulas de polvo, estelas de velocidad, confeti, linea de meta con patron de cuadros, tribuna con publico animado

### 3. Letras y Numeros (`letras.html`)

Juego de aprendizaje con 4 tipos de mini-juego que rotan aleatoriamente cada ronda.

- **Mini-juegos**:
  1. **Encuentra la letra/numero** — se muestra un caracter grande y hay que tocarlo entre las opciones
  2. **Asocia emoji con letra** — se muestra un animal y hay que encontrar su letra inicial; o se muestran N emojis/personajes Paw Patrol y hay que contarlos
  3. **Teclea la letra** — en escritorio hay que presionar la tecla correcta; en movil es opcion multiple
  4. **Completa la palabra** — una palabra con una letra faltante (20 palabras: GATO, PERRO, SOL, LUNA, CHASE, SKYE, MARSHALL, etc.)
- **5 niveles**:
  - A️⃣ Vocales — A, E, I, O, U (3 opciones)
  - 🔤 Letras — A-Z (4 opciones, incluye palabras)
  - 1️⃣ Numeros — 1-9 (4 opciones)
  - ⭐ Mezcla — letras + numeros (5 opciones)
  - 🏆 Campeon — todo mezclado (6 opciones)
- **Paw Patrol**: 7 personajes (Chase, Marshall, Skye, Rubble, Zuma, Rocky, Everest) aparecen como imagenes circulares en rondas de conteo (~30% de probabilidad) y como palabras para completar
- **Controles**: flechas + Enter, clic/toca, tecla directa, numeros 1-9
- Sin vidas ni penalizacion — siempre positivo
- Confeti cada 5 aciertos, estrellas acumulativas
- Voz narra cada pregunta y felicita con el nombre del jugador

### 4. Sumas y Restas (`mates.html`)

Aprendizaje de aritmetica con explicacion visual paso a paso usando emojis contables.

- **Flujo por ronda**:
  1. (4.5s) Muestra primer grupo de emojis + voz: "Mira [Jugador], tengo N [animalitos]"
  2. (4.5s) Muestra operador (+/−) y segundo grupo + voz: "y llegan N mas" o "pero se van N"
  3. (9s) Muestra "= ?" con 3-4 opciones de respuesta
- **5 niveles**:
  - 🐥 Pollitos — sumas hasta 5 (3 opciones)
  - 🐰 Conejitos — sumas hasta 10 (4 opciones)
  - 🐱 Gatitos — restas faciles (3 opciones)
  - ⭐ Estrellitas — sumas y restas mezcladas (4 opciones)
  - 🏆 Campeones — operaciones hasta 20 (4 opciones)
- **Visual**: emojis aparecen uno por uno con animacion pop; en restas los emojis removidos se desvanecen con tachado y escala reducida
- **Paw Patrol**: ~30% de probabilidad de que los emojis sean imagenes de personajes Paw Patrol en lugar de emojis estandar
- **Controles**: flechas + Enter, clic/toca, numeros directos

---

## Audio

### Efectos de sonido

Todos sintetizados con Web Audio API — sin archivos de audio externos para efectos.

- **Salto**: tono ascendente (400→700→1000 Hz)
- **Aterrizaje**: golpe grave (300 Hz)
- **Choque**: barrido de graves (120→80 Hz)
- **Correcto**: acorde mayor (523→659→784 Hz)
- **Incorrecto**: tono descendente (300→250 Hz)
- **Victoria**: melodia de 7 notas (Do-Mi-Sol-Do'-Sol'-Do''-Mi'')
- **Galope**: ritmo de golpe (200+160 Hz)
- **Campana**: campanada (1200+1500 Hz)

### Musica de fondo

Dos pistas de Kevin MacLeod (CC BY 4.0), volumen 0.18:

- `music/fun.mp3` — usada en animalitos, horserace y mates
- `music/calm.mp3` — usada en letras

### Voz narrativa (TTS)

Sistema de dos capas:

1. **Principal**: Amazon Polly (voz Mia, es-MX) via proxy Caddy en `/tts` → `streamlabs.com/polly/speak`
2. **Fallback**: Web Speech API con sistema de scoring para seleccionar la mejor voz disponible (prioriza voces premium, latinoamericanas y femeninas)

La voz narra preguntas, felicita por nombre ("Muy bien Bela!") y explica los problemas paso a paso.

---

## Tecnologia

Archivos HTML independientes con CSS + JavaScript puro. Cero dependencias externas.

| Componente | Tecnologia |
| ---------- | ---------- |
| Rendering carreras | Canvas 2D con requestAnimationFrame |
| Rendering quiz | HTML DOM |
| Sonidos | Web Audio API (sintetizados) |
| Voz | Amazon Polly (Caddy proxy) + Web Speech API fallback |
| Fuentes | Google Fonts — Bubblegum Sans (titulos) + Fredoka (cuerpo) |
| Almacenamiento | localStorage (jugadores, caballos custom) · sessionStorage (jugador activo) |
| PWA | manifest.json + iconos 192/512px |
| Responsive | CSS clamp(), media queries (<600px), soporte touch y teclado |

### Estructura de archivos

```text
juegobella/
├── index.html          # Redireccion a juegos.html
├── juegos.html         # Menu principal + seleccion de jugador
├── animalitos.html     # Carrera de animalitos (obstaculos)
├── horserace.html      # Hipodromo (tap-to-gallop)
├── letras.html         # Letras y numeros (quiz)
├── mates.html          # Sumas y restas (aritmetica visual)
├── manifest.json       # PWA manifest
├── Caddyfile           # Reverse proxy (TTS + static serving)
├── Staticfile          # Marcador de app estatica
├── icon-192.png        # Icono PWA 192×192
├── icon-512.png        # Icono PWA 512×512
├── music/
│   ├── fun.mp3         # Musica animada (Kevin MacLeod)
│   └── calm.mp3        # Musica tranquila (Kevin MacLeod)
└── paw/
    ├── chase.jpg
    ├── marshall.jpg
    ├── skye.jpg
    ├── rubble.jpg
    ├── zuma.jpg
    ├── rocky.jpg
    └── everest.jpg
```

---

## Deploy

Desplegado en **Railway** con Caddy como servidor. El Caddyfile configura:

- Puerto dinamico via `{$PORT}`
- Proxy inverso `/tts` → Streamlabs Polly (con headers `Host` y `Referer` requeridos)
- Servidor de archivos estaticos desde `/app`
- Headers no-cache para archivos HTML

### Ejecutar localmente

```bash
python3 -m http.server 3000
# abrir http://localhost:3000/juegos.html
```

> Nota: sin Caddy, la voz TTS usara el fallback Web Speech API del navegador.

---

## Creditos

- Musica: "Monkeys Spinning Monkeys" y "Carefree" por Kevin MacLeod — [incompetech.com](https://incompetech.com) — Licencia CC BY 4.0
- Imagenes Paw Patrol: pawpatrol.com (uso educativo)
