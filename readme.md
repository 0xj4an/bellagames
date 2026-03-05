# Juegos Bella

Coleccion de juegos para Bella. Abre `juegosbella.html` para ver el menu principal.

## Juegos

### Carrera de Animalitos (`animalitos.html`)

Elige tu animal favorito y compite contra dos rivales saltando obstaculos.

- **Controles**: ESPACIO o toca la pantalla para saltar
- **Animales**: 🐎 Estrella · 🐕 Luna · 🐈 Michi · 🐢 Tito · 🐇 Rayo · 🦊 Zorro · 🐖 Rosita · 🦮 Bingo · 🦜 Coco · 🐻 Oso
- Obstaculos: vallas, rocas, charcos, troncos
- 3 vidas, los rivales tambien saltan y fallan

### Hipodromo (`caballos.html`)

Carrera de caballos de cuarto de milla. Toca rapido para galopar.

- **Controles**: Toca rapido o presiona ESPACIO repetidamente para dar velocidad
- **Caballos**: Relampago · Estrella · Tornado · Luna · Rayo · Princesa · Trueno · Nube
- 6 carriles, pista recta, IA con bursts de velocidad
- Mientras mas rapido toques, mas rapido corre tu caballo

### Letras y Numeros (`letras.html`)

Aprende letras y numeros con mini-juegos variados.

- **Mini-juegos**: Encuentra la letra/numero, asocia emoji con letra inicial, cuenta emojis, teclea la letra correcta
- **Controles**: Flechas + Enter, clic/toca, o tecla directa
- Sin vidas ni penalizacion, siempre positivo
- Celebracion con confeti cada 5 aciertos
- Letras A-Z con animales, numeros 1-10 con conteo

## Tecnologia

Archivos HTML independientes con CSS + JavaScript puro. Cero dependencias.

- Canvas 2D para los juegos
- Web Audio API para sonidos y musica
- Google Fonts (Bubblegum Sans + Fredoka)
- Soporte movil (touch) y escritorio (teclado)

## Ejecutar

Abrir `juegosbella.html` en el navegador, o con servidor local:

```bash
python3 -m http.server 3000
# abrir http://localhost:3000/juegosbella.html
```
