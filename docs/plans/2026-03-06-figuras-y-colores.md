# Figuras y Colores - Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create a new educational game (`figuras.html`) where kids learn geometric shapes and colors through 5 types of audio-guided visual quiz rounds.

**Architecture:** Single self-contained HTML file with inline CSS + JS, following the exact same patterns as `letras.html`. SVG inline elements for shapes, DOM-based quiz with the same screen management, audio, TTS, keyboard nav, and feedback systems.

**Tech Stack:** Vanilla HTML/CSS/JS, inline SVG for shapes, Web Audio API, TTS (Polly + fallback)

---

### Task 1: Create figuras.html with boilerplate + level screen

**Files:**
- Create: `figuras.html`

**Step 1: Create the file with full boilerplate**

Copy the exact structure from `letras.html` (lines 1-101) adapting only:
- Title: `Figuras y Colores - Mis Juegos`
- Background: `#E3F2FD` to `#BBDEFB` to `#E1BEE7` (light blue to lavender)
- Level grid: same 5-column grid as letras.html
- Color accents: use `#42A5F5` (blue) instead of `#CE93D8` (purple) for text shadows

Include all of these sections from letras.html verbatim:
- Meta tags (lines 1-13)
- Font imports (line 16)
- CSS reset + body (lines 17-18)
- Screen management CSS (lines 19-20)
- Back button + levels button CSS (lines 21-28)
- Level screen + level card CSS (lines 29-40) — change `#CE93D8` to `#42A5F5`
- Game screen + game area CSS (lines 42-43)
- Prompt CSS (line 45) — change shadow color
- Options grid + opt-btn CSS (lines 54-61) — change text color to `#1565C0`
- HUD CSS (lines 71-72)
- Feedback + confetti + star particle CSS (lines 74-85)

Add NEW CSS for shape display:
```css
.shape-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:18px;width:100%;max-width:500px;justify-items:center}
@media(min-width:600px){.shape-grid.cols-3{grid-template-columns:repeat(3,1fr)}}
.shape-card{background:rgba(255,255,255,.25);border-radius:22px;padding:16px;cursor:pointer;border:5px solid transparent;box-shadow:0 4px 15px rgba(0,0,0,.08);transition:all .2s cubic-bezier(.34,1.56,.64,1);display:flex;align-items:center;justify-content:center;min-width:100px;min-height:100px}
.shape-card:hover{transform:translateY(-4px) scale(1.05);border-color:#FFD54F}
.shape-card.selected{border-color:#FFD54F;transform:translateY(-4px) scale(1.05)}
.shape-card.correct{background:rgba(102,187,106,.3);border-color:#43A047;transform:scale(1.1)}
.shape-card.wrong{background:rgba(239,154,154,.3);border-color:#EF5350;animation:shake .4s}
.shape-card.hint{border-color:#66BB6A;background:rgba(102,187,106,.15)}
.shape-card svg{width:clamp(60px,15vw,120px);height:clamp(60px,15vw,120px);filter:drop-shadow(2px 4px 6px rgba(0,0,0,.15))}
.big-shape{display:flex;align-items:center;justify-content:center;margin:10px 0}
.big-shape svg{width:clamp(100px,25vw,180px);height:clamp(100px,25vw,180px);filter:drop-shadow(3px 6px 10px rgba(0,0,0,.2));animation:charPop .4s cubic-bezier(.34,1.56,.64,1)}
```

HTML body structure (same as letras.html):
```html
<a href="juegos.html" class="back-btn">Volver</a>
<div id="level-screen" class="screen hidden">
  <div class="level-title">Elige nivel</div>
  <div class="level-grid" id="levelGrid"></div>
</div>
<div id="game-screen" class="screen hidden">
  <div class="hud"><div class="star-count" id="starCount">⭐ 0</div></div>
  <button class="levels-btn" id="levelsBtn">Niveles</button>
  <div class="game-area" id="gameArea"></div>
</div>
<div class="feedback hidden" id="feedback"><div class="fb-text" id="fbText">BIEN!</div></div>
```

**Step 2: Add the audio + TTS + music system**

Copy verbatim from `letras.html` lines 104-170:
- AudioContext setup, tone(), sndCorrect/Wrong/Click/Celeb
- TTS system: scoreVoice, pickVoice, stopSpeech, speakLocal, speak
- Background music: startMusic/stopMusic using `music/fun.mp3`

**Step 3: Add the data definitions**

```javascript
// === DATA ===
const SHAPES=[
  // Basic (Level 1)
  {id:'circulo',name:'circulo',sides:0,svg:d=>`<circle cx="60" cy="60" r="50" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'cuadrado',name:'cuadrado',sides:4,svg:d=>`<rect x="10" y="10" width="100" height="100" rx="4" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'triangulo',name:'triangulo',sides:3,svg:d=>`<polygon points="60,10 110,110 10,110" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  // Intermediate (Level 2)
  {id:'estrella',name:'estrella',sides:10,svg:d=>`<polygon points="60,5 72,45 115,45 80,70 92,110 60,85 28,110 40,70 5,45 48,45" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'rectangulo',name:'rectangulo',sides:4,svg:d=>`<rect x="5" y="25" width="110" height="70" rx="4" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  // Advanced (Level 3)
  {id:'ovalo',name:'ovalo',sides:0,svg:d=>`<ellipse cx="60" cy="60" rx="55" ry="38" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'corazon',name:'corazon',sides:0,svg:d=>`<path d="M60,100 C20,70 0,40 20,20 C40,0 60,15 60,35 C60,15 80,0 100,20 C120,40 100,70 60,100Z" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'diamante',name:'diamante',sides:4,svg:d=>`<polygon points="60,5 115,60 60,115 5,60" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  // Expert (Level 4-5)
  {id:'pentagono',name:'pentagono',sides:5,svg:d=>`<polygon points="60,5 115,40 95,105 25,105 5,40" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'hexagono',name:'hexagono',sides:6,svg:d=>`<polygon points="90,10 115,60 90,110 30,110 5,60 30,10" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'trapecio',name:'trapecio',sides:4,svg:d=>`<polygon points="35,15 85,15 115,105 5,105" fill="${d}" stroke="#fff" stroke-width="3"/>`},
  {id:'flecha',name:'flecha',sides:7,svg:d=>`<polygon points="60,5 110,50 85,50 85,115 35,115 35,50 10,50" fill="${d}" stroke="#fff" stroke-width="3"/>`},
];

const COLORS=[
  {id:'rojo',name:'roja',hex:'#EF5350'},
  {id:'azul',name:'azul',hex:'#42A5F5'},
  {id:'amarillo',name:'amarilla',hex:'#FFD54F'},
  {id:'verde',name:'verde',hex:'#66BB6A'},
  {id:'naranja',name:'naranja',hex:'#FF8A65'},
  {id:'morado',name:'morada',hex:'#AB47BC'},
  {id:'rosa',name:'rosa',hex:'#F48FB1'},
  {id:'celeste',name:'celeste',hex:'#4FC3F7'},
];

const LEVELS=[
  {name:'Figuritas',emoji:'🔵',desc:'Figuras basicas',shapes:3,colors:3,rounds:['shape'],opts:3},
  {name:'Colores',emoji:'🎨',desc:'Aprende colores',shapes:5,colors:6,rounds:['shape','color'],opts:3},
  {name:'Formas',emoji:'🔶',desc:'Figuras y lados',shapes:8,colors:8,rounds:['shape','color','sides'],opts:4},
  {name:'Mezcla',emoji:'⭐',desc:'Figura y color',shapes:12,colors:8,rounds:['shape','color','sides','combo'],opts:4},
  {name:'Campeon',emoji:'🏆',desc:'Todo mas dificil',shapes:12,colors:8,rounds:['shape','color','sides','combo','different'],opts:5},
];

const PAW_PATROL=[
  {name:'Chase',img:'paw/chase.jpg'},
  {name:'Marshall',img:'paw/marshall.jpg'},
  {name:'Skye',img:'paw/skye.jpg'},
  {name:'Rubble',img:'paw/rubble.jpg'},
  {name:'Zuma',img:'paw/zuma.jpg'},
  {name:'Rocky',img:'paw/rocky.jpg'},
  {name:'Everest',img:'paw/everest.jpg'},
];

const MSGS_OK=['BIEN!','GENIAL!','SUPER!','BRAVO!','PERFECTO!'];
const MSGS_RETRY=['Aaay no!','Uy uy uy!','Epa!','Nooo!','Casi!'];
const SPEECH_RETRY=['Aaay que mal, intenta de nuevo','Uy uy uy, esa no es','Nooo, intenta otra vez','Epa, casi casi','Aaay no, prueba otra'];
```

**Step 4: Add state + screen management + level screen logic**

Copy the exact pattern from letras.html lines 228-275:
- Player name from sessionStorage
- State variables: stars, streak, currentRound, locked, focusIdx, currentLevel, levelFocus
- show() function
- initAudio, show('level-screen'), buildLevelGrid(), startMusic()
- buildLevelGrid(), highlightLevel(), selectLevel()
- levelsBtn onclick handler

**Step 5: Add the SVG helper function**

```javascript
function renderSVG(shape,color,size){
  size=size||120;
  return '<svg viewBox="0 0 120 120" width="'+size+'" height="'+size+'">'+shape.svg(color)+'</svg>';
}
```

**Step 6: Verify level screen renders correctly**

Open `figuras.html` in browser. Verify:
- 5 level cards display with correct emojis/names
- Arrow keys navigate between cards
- Enter selects a level
- 1-5 quick-select works
- Escape goes back to juegos.html
- Player name shows in title if set

**Step 7: Commit**

```bash
git add figuras.html
git commit -m "feat: add figuras.html with boilerplate, audio, data, and level screen"
```

---

### Task 2: Implement Round 1 - Identify Shape

**Files:**
- Modify: `figuras.html`

**Step 1: Add nextRound() dispatcher**

```javascript
function nextRound(){
  locked=false;focusIdx=0;stopSpeech();
  const lv=currentLevel;
  const pick=lv.rounds[Math.floor(Math.random()*lv.rounds.length)];
  if(pick==='shape') roundShape();
  else if(pick==='color') roundColor();
  else if(pick==='sides') roundSides();
  else if(pick==='combo') roundCombo();
  else roundDifferent();
}
```

**Step 2: Add roundShape()**

```javascript
function roundShape(){
  const lv=currentLevel;
  const pool=SHAPES.slice(0,lv.shapes);
  const colPool=COLORS.slice(0,lv.colors);
  const target=pool[Math.floor(Math.random()*pool.length)];
  // all same color to avoid confusion
  const color=colPool[Math.floor(Math.random()*colPool.length)];
  const distractors=[];
  while(distractors.length<lv.opts-1){
    const d=pool[Math.floor(Math.random()*pool.length)];
    if(d.id!==target.id&&!distractors.some(x=>x.id===d.id))distractors.push(d);
  }
  const opts=shuffle([target,...distractors]);
  currentRound={type:'shape',answer:target.id,opts,color:color.hex,targetName:target.name};
  renderShapeRound();
}
```

**Step 3: Add renderShapeRound()**

```javascript
function renderShapeRound(){
  const r=currentRound;
  const area=document.getElementById('gameArea');
  const pn=player?', '+player:'';
  // ~30% Paw Patrol decoration
  const pw=Math.random()<0.3?PAW_PATROL[Math.floor(Math.random()*PAW_PATROL.length)]:null;
  const pawHtml=pw?'<img class="paw-img" src="'+pw.img+'" alt="'+pw.name+'" style="width:50px;height:50px;border-radius:50%;border:3px solid #FFD54F;margin-bottom:8px">':'';
  area.innerHTML=
    pawHtml+
    '<div class="prompt">Toca el '+r.targetName+'!</div>'+
    '<div class="shape-grid" id="opts">'+
    r.opts.map(function(s,i){
      return '<div class="shape-card" data-val="'+s.id+'" data-idx="'+i+'">'+renderSVG(s,r.color)+'</div>';
    }).join('')+'</div>';
  bindShapeOpts();
  speak('Toca el '+r.targetName+pn);
}
```

**Step 4: Add bindShapeOpts() and checkShapeAnswer()**

```javascript
function bindShapeOpts(){
  document.querySelectorAll('.shape-card').forEach(function(card){
    card.onclick=function(){checkShapeAnswer(card.dataset.val,parseInt(card.dataset.idx))};
  });
}

function highlightShape(i){
  const cards=document.querySelectorAll('.shape-card');
  if(!cards.length)return;
  focusIdx=((i%cards.length)+cards.length)%cards.length;
  cards.forEach(function(c,j){c.classList.toggle('selected',j===focusIdx)});
}

function checkShapeAnswer(val,idx){
  if(locked)return;
  const card=document.querySelectorAll('.shape-card')[idx];
  if(!card)return;
  if(val===currentRound.answer){
    locked=true;
    card.classList.add('correct');
    sndCorrect();stars++;streak++;
    document.getElementById('starCount').textContent='⭐ '+stars;
    const pn=player?(' '+player+'!'):'!';
    const msgs=['Muy bien'+pn,'Excelente'+pn,'Bravo'+pn,'Genial'+pn,'Perfecto'+pn];
    showFeedback(MSGS_OK[Math.floor(Math.random()*MSGS_OK.length)],false);
    stopSpeech();speak(msgs[Math.floor(Math.random()*msgs.length)]);
    spawnStars(card);
    if(streak%5===0)spawnConf();
    setTimeout(nextRound,3000);
  } else {
    card.classList.add('wrong');
    sndWrong();streak=0;
    const pn=player?(' '+player):'';
    showFeedback(MSGS_RETRY[Math.floor(Math.random()*MSGS_RETRY.length)],true);
    stopSpeech();speak(SPEECH_RETRY[Math.floor(Math.random()*SPEECH_RETRY.length)]+pn);
    setTimeout(function(){
      card.classList.remove('wrong');
      document.querySelectorAll('.shape-card').forEach(function(c){
        if(c.dataset.val===currentRound.answer)c.classList.add('hint');
      });
      setTimeout(function(){stopSpeech();speak('Es el '+currentRound.targetName)},800);
    },500);
  }
}
```

**Step 5: Add feedback, particles, shuffle functions**

Copy from letras.html lines 603-638: showFeedback, spawnStars, spawnConf, shuffle.

**Step 6: Add keyboard handler for game screen**

```javascript
window.addEventListener('keydown',function(e){
  // Level screen
  if(!document.getElementById('level-screen').classList.contains('hidden')){
    if(e.key==='ArrowDown'||e.key==='ArrowRight'){e.preventDefault();highlightLevel(levelFocus+1)}
    else if(e.key==='ArrowUp'||e.key==='ArrowLeft'){e.preventDefault();highlightLevel(levelFocus-1)}
    else if(e.key==='Enter'){e.preventDefault();sndClick();selectLevel(levelFocus)}
    else if(e.key>='1'&&e.key<='5'){e.preventDefault();sndClick();selectLevel(parseInt(e.key)-1)}
    else if(e.key==='Escape'){e.preventDefault();window.location.href='juegos.html'}
    return;
  }
  // Game screen
  if(!document.getElementById('game-screen').classList.contains('hidden')){
    if(e.key==='Escape'){e.preventDefault();sndClick();stopSpeech();show('level-screen');buildLevelGrid();return}
    if(locked)return;
    var cards=document.querySelectorAll('.shape-card');
    var btns=document.querySelectorAll('.opt-btn');
    var items=cards.length?cards:btns;
    if(!items.length)return;
    if(e.key==='ArrowRight'||e.key==='ArrowDown'){e.preventDefault();if(cards.length)highlightShape(focusIdx+1);else highlightOpt(focusIdx+1)}
    else if(e.key==='ArrowLeft'||e.key==='ArrowUp'){e.preventDefault();if(cards.length)highlightShape(focusIdx-1);else highlightOpt(focusIdx-1)}
    else if(e.key==='Enter'){
      e.preventDefault();
      if(cards.length){var c=cards[focusIdx];if(c)checkShapeAnswer(c.dataset.val,focusIdx)}
      else{var b=btns[focusIdx];if(b)checkAnswer(b.dataset.val,parseInt(b.dataset.idx))}
    }
  }
});
```

**Step 7: Verify round 1 works**

Open figuras.html, select level 1 (Figuritas). Verify:
- 3 different shapes appear, all same color
- TTS says "Toca el [shape name]"
- Tapping correct shape: green glow, sound, stars, feedback
- Tapping wrong: shake, hint on correct, TTS says answer
- Arrow keys navigate, Enter confirms
- Escape goes back to levels

**Step 8: Commit**

```bash
git add figuras.html
git commit -m "feat: add identify shape round with SVG rendering and full input handling"
```

---

### Task 3: Implement Round 2 - Identify Color

**Files:**
- Modify: `figuras.html`

**Step 1: Add roundColor()**

```javascript
function roundColor(){
  const lv=currentLevel;
  const pool=SHAPES.slice(0,lv.shapes);
  const colPool=COLORS.slice(0,lv.colors);
  const targetColor=colPool[Math.floor(Math.random()*colPool.length)];
  // all same shape, different colors
  const shape=pool[Math.floor(Math.random()*pool.length)];
  const distractors=[];
  while(distractors.length<lv.opts-1){
    const d=colPool[Math.floor(Math.random()*colPool.length)];
    if(d.id!==targetColor.id&&!distractors.some(x=>x.id===d.id))distractors.push(d);
  }
  const opts=shuffle([targetColor,...distractors]);
  currentRound={type:'color',answer:targetColor.id,opts:opts.map(c=>({colorObj:c,shape})),targetName:targetColor.name,shape};
  renderColorRound();
}
```

**Step 2: Add renderColorRound()**

```javascript
function renderColorRound(){
  const r=currentRound;
  const area=document.getElementById('gameArea');
  const pn=player?', '+player:'';
  const pw=Math.random()<0.3?PAW_PATROL[Math.floor(Math.random()*PAW_PATROL.length)]:null;
  const pawHtml=pw?'<img class="paw-img" src="'+pw.img+'" alt="'+pw.name+'" style="width:50px;height:50px;border-radius:50%;border:3px solid #FFD54F;margin-bottom:8px">':'';
  area.innerHTML=
    pawHtml+
    '<div class="prompt">Toca la '+r.targetName+'!</div>'+
    '<div class="shape-grid" id="opts">'+
    r.opts.map(function(o,i){
      return '<div class="shape-card" data-val="'+o.colorObj.id+'" data-idx="'+i+'">'+renderSVG(o.shape,o.colorObj.hex)+'</div>';
    }).join('')+'</div>';
  bindShapeOpts();
  speak('Toca la '+r.targetName+pn);
}
```

Note: `checkShapeAnswer` already handles any `currentRound.answer` string comparison, so it works for both shape IDs and color IDs. Update the "wrong" speech to say color name:

In `checkShapeAnswer`, change the wrong-answer speech to be context-aware:
```javascript
// In the wrong answer setTimeout callback, replace the speak line:
var hint=currentRound.type==='color'?'Es la '+currentRound.targetName:'Es el '+currentRound.targetName;
setTimeout(function(){stopSpeech();speak(hint)},800);
```

**Step 3: Verify round 2 works**

Select level 2 (Colores). Verify:
- Rounds randomly alternate between shape and color identification
- Color round: same shape, different colors
- TTS says "Toca la [color name]"
- Correct/wrong feedback works

**Step 4: Commit**

```bash
git add figuras.html
git commit -m "feat: add identify color round type"
```

---

### Task 4: Implement Round 3 - Count Sides

**Files:**
- Modify: `figuras.html`

**Step 1: Add highlightOpt() and checkAnswer() for number buttons**

```javascript
function highlightOpt(i){
  const btns=document.querySelectorAll('.opt-btn');
  if(!btns.length)return;
  focusIdx=((i%btns.length)+btns.length)%btns.length;
  btns.forEach(function(b,j){b.classList.toggle('selected',j===focusIdx)});
}

function checkAnswer(val,idx){
  if(locked)return;
  const btn=document.querySelectorAll('.opt-btn')[idx];
  if(!btn)return;
  if(val===currentRound.answer){
    locked=true;
    btn.classList.add('correct');
    sndCorrect();stars++;streak++;
    document.getElementById('starCount').textContent='⭐ '+stars;
    const pn=player?(' '+player+'!'):'!';
    const msgs=['Muy bien'+pn,'Excelente'+pn,'Bravo'+pn,'Genial'+pn,'Perfecto'+pn];
    showFeedback(MSGS_OK[Math.floor(Math.random()*MSGS_OK.length)],false);
    stopSpeech();speak(msgs[Math.floor(Math.random()*msgs.length)]);
    spawnStars(btn);
    if(streak%5===0)spawnConf();
    setTimeout(nextRound,3000);
  } else {
    btn.classList.add('wrong');
    sndWrong();streak=0;
    const pn=player?(' '+player):'';
    showFeedback(MSGS_RETRY[Math.floor(Math.random()*MSGS_RETRY.length)],true);
    stopSpeech();speak(SPEECH_RETRY[Math.floor(Math.random()*SPEECH_RETRY.length)]+pn);
    setTimeout(function(){
      btn.classList.remove('wrong');
      document.querySelectorAll('.opt-btn').forEach(function(b){
        if(b.dataset.val===currentRound.answer)b.classList.add('hint');
      });
      setTimeout(function(){stopSpeech();speak('Son '+currentRound.answer)},800);
    },500);
  }
}
```

**Step 2: Add roundSides()**

```javascript
function roundSides(){
  const lv=currentLevel;
  const pool=SHAPES.slice(0,lv.shapes);
  const colPool=COLORS.slice(0,lv.colors);
  const target=pool[Math.floor(Math.random()*pool.length)];
  const color=colPool[Math.floor(Math.random()*colPool.length)];
  const answer=String(target.sides);
  // generate side count options
  const possibleSides=['0','3','4','5','6','7','10'];
  const distractors=[];
  while(distractors.length<lv.opts-1){
    const d=possibleSides[Math.floor(Math.random()*possibleSides.length)];
    if(d!==answer&&!distractors.includes(d))distractors.push(d);
  }
  const opts=shuffle([answer,...distractors]);
  currentRound={type:'sides',answer,opts,shape:target,color:color.hex,targetName:target.name};
  renderSidesRound();
}
```

**Step 3: Add renderSidesRound()**

```javascript
function renderSidesRound(){
  const r=currentRound;
  const area=document.getElementById('gameArea');
  const pn=player?', '+player:'';
  area.innerHTML=
    '<div class="prompt">Cuantos lados tiene?</div>'+
    '<div class="big-shape">'+renderSVG(r.shape,r.color,160)+'</div>'+
    '<div class="options" id="opts">'+
    r.opts.map(function(o,i){return '<button class="opt-btn" data-val="'+o+'" data-idx="'+i+'">'+o+'</button>'}).join('')+'</div>';
  document.querySelectorAll('.opt-btn').forEach(function(btn){
    btn.onclick=function(){checkAnswer(btn.dataset.val,parseInt(btn.dataset.idx))};
  });
  speak('Cuantos lados tiene el '+r.targetName+pn+'?');
}
```

**Step 4: Verify round 3 works**

Select level 3 (Formas). Verify:
- Large shape appears centered
- TTS asks "Cuantos lados tiene?"
- Number buttons appear (e.g., 0, 3, 4, 5)
- Correct answer for circle = 0, triangle = 3, square = 4, etc.

**Step 5: Commit**

```bash
git add figuras.html
git commit -m "feat: add count sides round type"
```

---

### Task 5: Implement Round 4 - Shape + Color Combo

**Files:**
- Modify: `figuras.html`

**Step 1: Add roundCombo()**

```javascript
function roundCombo(){
  const lv=currentLevel;
  const shapePool=SHAPES.slice(0,lv.shapes);
  const colPool=COLORS.slice(0,lv.colors);
  const targetShape=shapePool[Math.floor(Math.random()*shapePool.length)];
  const targetColor=colPool[Math.floor(Math.random()*colPool.length)];
  // generate distractors: each must differ in shape OR color (not both matching)
  const opts=[{shape:targetShape,color:targetColor}];
  let tries=0;
  while(opts.length<lv.opts&&tries<200){
    tries++;
    const s=shapePool[Math.floor(Math.random()*shapePool.length)];
    const c=colPool[Math.floor(Math.random()*colPool.length)];
    if(s.id===targetShape.id&&c.id===targetColor.id)continue;
    if(!opts.some(o=>o.shape.id===s.id&&o.color.id===c.id))opts.push({shape:s,color:c});
  }
  const shuffled=shuffle(opts);
  const answerIdx=shuffled.findIndex(o=>o.shape.id===targetShape.id&&o.color.id===targetColor.id);
  currentRound={type:'combo',answerKey:targetShape.id+'_'+targetColor.id,opts:shuffled,targetShape,targetColor};
  renderComboRound();
}
```

**Step 2: Add renderComboRound()**

```javascript
function renderComboRound(){
  const r=currentRound;
  const area=document.getElementById('gameArea');
  const pn=player?', '+player:'';
  area.innerHTML=
    '<div class="prompt">Toca el '+r.targetShape.name+' '+r.targetColor.name+'!</div>'+
    '<div class="shape-grid" id="opts">'+
    r.opts.map(function(o,i){
      return '<div class="shape-card" data-val="'+o.shape.id+'_'+o.color.id+'" data-idx="'+i+'">'+renderSVG(o.shape,o.color.hex)+'</div>';
    }).join('')+'</div>';
  // Override answer for checkShapeAnswer
  currentRound.answer=r.answerKey;
  currentRound.targetName=r.targetShape.name+' '+r.targetColor.name;
  bindShapeOpts();
  speak('Toca el '+r.targetShape.name+' '+r.targetColor.name+pn);
}
```

**Step 3: Verify round 4 works**

Select level 4 (Mezcla). Verify:
- Multiple shapes with different forms AND colors
- TTS says "Toca el triangulo rojo"
- Only one combination is correct
- Feedback works correctly

**Step 4: Commit**

```bash
git add figuras.html
git commit -m "feat: add shape+color combo round type"
```

---

### Task 6: Implement Round 5 - Find the Different

**Files:**
- Modify: `figuras.html`

**Step 1: Add roundDifferent()**

```javascript
function roundDifferent(){
  const lv=currentLevel;
  const shapePool=SHAPES.slice(0,lv.shapes);
  const colPool=COLORS.slice(0,lv.colors);
  // pick base shape and color
  const baseShape=shapePool[Math.floor(Math.random()*shapePool.length)];
  const baseColor=colPool[Math.floor(Math.random()*colPool.length)];
  // decide if the odd one differs in shape or color
  const diffByShape=Math.random()<0.5;
  let oddShape,oddColor;
  if(diffByShape){
    oddColor=baseColor;
    do{oddShape=shapePool[Math.floor(Math.random()*shapePool.length)]}while(oddShape.id===baseShape.id);
  } else {
    oddShape=baseShape;
    do{oddColor=colPool[Math.floor(Math.random()*colPool.length)]}while(oddColor.id===baseColor.id);
  }
  // build 4 cards: 3 same + 1 different
  const oddIdx=Math.floor(Math.random()*4);
  const opts=[];
  for(let i=0;i<4;i++){
    if(i===oddIdx)opts.push({shape:oddShape,color:oddColor,isOdd:true});
    else opts.push({shape:baseShape,color:baseColor,isOdd:false});
  }
  currentRound={type:'different',answer:String(oddIdx),opts,targetName:'la diferente'};
  renderDifferentRound();
}
```

**Step 2: Add renderDifferentRound()**

```javascript
function renderDifferentRound(){
  const r=currentRound;
  const area=document.getElementById('gameArea');
  const pn=player?', '+player:'';
  area.innerHTML=
    '<div class="prompt">Cual es diferente?</div>'+
    '<div class="shape-grid" id="opts">'+
    r.opts.map(function(o,i){
      return '<div class="shape-card" data-val="'+i+'" data-idx="'+i+'">'+renderSVG(o.shape,o.color.hex)+'</div>';
    }).join('')+'</div>';
  currentRound.answer=String(r.opts.findIndex(o=>o.isOdd));
  bindShapeOpts();
  speak('Cual es diferente'+pn+'?');
}
```

Note: `checkShapeAnswer` compares `val===currentRound.answer` as strings, so using index as string works. Update the wrong-answer hint for 'different' type:

In `checkShapeAnswer` wrong-answer callback, add a case for 'different':
```javascript
var hint;
if(currentRound.type==='color')hint='Es la '+currentRound.targetName;
else if(currentRound.type==='different')hint='Esa es la diferente';
else hint='Es el '+currentRound.targetName;
```

**Step 3: Verify round 5 works**

Select level 5 (Campeon). Verify:
- 4 shapes appear: 3 identical + 1 different
- The difference is either shape or color
- TTS says "Cual es diferente?"
- All 5 round types rotate randomly

**Step 4: Commit**

```bash
git add figuras.html
git commit -m "feat: add find-the-different round type, all 5 rounds complete"
```

---

### Task 7: Add hub integration

**Files:**
- Modify: `juegos.html`

**Step 1: Add game card #5 to juegos.html**

After the `mates.html` game card (line 129), add:
```html
<a href="figuras.html" class="game-card" id="game5">
  <span class="game-num">5</span>
  <span class="game-icon">🔷</span>
  <div class="game-info"><div class="game-name">Figuras y Colores</div>
  <div class="game-desc">Aprende figuras geometricas y colores</div></div>
</a>
```

**Step 2: Update keyboard handler for game 5**

In juegos.html gameKeyHandler function (line 383), add:
```javascript
if(e.key==='5'){window.location.href='figuras.html';return}
```

**Step 3: Verify hub integration**

Open juegos.html. Verify:
- 5th game card appears in the grid
- Card is styled consistently with others
- Click/tap navigates to figuras.html
- Key "5" navigates to figuras.html
- Arrow keys cycle through all 5 cards
- Mobile view: cards stack correctly in 1-column layout

**Step 4: Commit**

```bash
git add juegos.html figuras.html
git commit -m "feat: add Figuras y Colores to hub, complete game integration"
```

---

### Task 8: Final polish and verification

**Files:**
- Modify: `figuras.html` (if needed)

**Step 1: Test all 5 levels end-to-end**

For each level:
- Verify correct round types are available
- Verify correct shape/color pools
- Verify option counts match level spec
- Test correct + wrong answers
- Verify streak counter and confetti at 5

**Step 2: Test responsive design**

- Desktop: verify 2-column grid, hover effects, keyboard nav
- Mobile (Chrome DevTools device mode): verify touch, grid layout, font scaling
- Test at 320px, 375px, 768px, 1024px widths

**Step 3: Test audio**

- Verify TTS speaks shape names and color names correctly
- Verify correct/wrong/celebrate sounds play
- Verify background music plays and loops

**Step 4: Fix any issues found**

Apply fixes as needed.

**Step 5: Final commit**

```bash
git add figuras.html
git commit -m "fix: polish figuras game after end-to-end testing"
```
