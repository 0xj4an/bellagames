# Juegos Bella y Penu

Coleccion de juegos educativos para Bella y Penu. Abre `juegosbella.html` para ver el menu principal.

Al iniciar, se pregunta quien va a jugar (Bella o Penu) y toda la experiencia se personaliza con su nombre.

## Juegos

### 1. Carrera de Animalitos (`animalitos.html`)

Elige tu animal favorito y compite contra dos rivales saltando obstaculos.

- **Controles**: ESPACIO o toca la pantalla para saltar
- **Animales**: 🐎 Estrella · 🐕 Luna · 🐈 Michi · 🐢 Tito · 🐇 Rayo · 🦊 Zorro · 🐖 Rosita · 🦮 Bingo · 🦜 Coco · 🐻 Oso
- Obstaculos: vallas, rocas, charcos, troncos
- 3 vidas, los rivales tambien saltan y fallan

### 2. Hipodromo (`caballos.html`)

Carrera de caballos de cuarto de milla. Toca rapido para galopar.

- **Controles**: Toca rapido o presiona ESPACIO repetidamente para dar velocidad
- **Caballos**: Relampago · Estrella · Tornado · Luna · Rayo · Princesa · Trueno · Nube
- 6 carriles, pista recta, IA con bursts de velocidad

### 3. Letras y Numeros (`letras.html`)

Aprende letras y numeros con mini-juegos variados, incluyendo personajes de Paw Patrol.

- **Mini-juegos**:
  - Encuentra la letra/numero
  - Asocia emoji con letra inicial
  - Cuenta emojis
  - Teclea la letra correcta
  - Completa la palabra (encuentra la letra que falta)
- **Paw Patrol**: Palabras con personajes como Chase, Skye, Marshall, Rocky, Zuma, Rubble, Tracker y Everest
- **Controles**: Flechas + Enter, clic/toca, o tecla directa
- Sin vidas ni penalizacion, siempre positivo
- Celebracion con confeti cada 5 aciertos
- Voz que narra y felicita personalizada con el nombre de la jugadora

### 4. Sumas y Restas (`mates.html`)

Aprende a sumar y restar con emojis contables y explicacion visual paso a paso.

- **5 niveles**:
  - 🐥 Pollitos — sumas hasta 5
  - 🐰 Conejitos — sumas hasta 10
  - 🐱 Gatitos — restas faciles
  - ⭐ Estrellitas — sumas y restas mezcladas
  - 🏆 Campeones — operaciones hasta 20
- Emojis visuales para contar (aparecen uno por uno)
- Restas muestran emojis desvaneciendose
- Voz personalizada que explica cada problema
- **Controles**: Flechas + Enter, clic/toca, o numeros directos

## Tecnologia

Archivos HTML independientes con CSS + JavaScript puro. Cero dependencias.

- Canvas 2D para los juegos de carreras
- Web Audio API para sonidos y musica
- Web Speech API para voz narrativa (prioriza voces latinoamericanas)
- Google Fonts (Bubblegum Sans + Fredoka)
- Soporte movil (touch) y escritorio (teclado)
- PWA con manifest.json e iconos para home screen
- sessionStorage para seleccion de jugadora (se reinicia al cerrar la app)

## Ejecutar

Abrir `juegosbella.html` en el navegador, o con servidor local:

```bash
python3 -m http.server 3000
# abrir http://localhost:3000/juegosbella.html
```
