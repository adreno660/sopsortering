<!DOCTYPE html>
<html lang="sv">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sopsortering – Sortera rätt</title>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@400;600;700&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --green: #2d6a4f;
    --green-light: #52b788;
    --green-pale: #d8f3dc;
    --amber: #d4831a;
    --amber-light: #f4a261;
    --cream: #faf7f0;
    --brown: #6b4226;
    --text: #1a2e1a;
    --muted: #5a7262;
    --border: #c5dfc9;
    --white: #ffffff;
    --red: #b5413a;
    --blue: #2a6496;
    --blue-light: #5ba3cc;
    --yellow: #c9a000;
    --yellow-light: #f7d070;
    --orange: #c0551a;
    --orange-light: #e8865a;
    --gray: #7a7a7a;
    --glass: #0a1f0e;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--cream);
    color: var(--text);
    min-height: 100vh;
  }

  header {
    background: var(--green);
    padding: 2.5rem 2rem 2rem;
    text-align: center;
    position: relative;
    overflow: hidden;
  }

  header::before {
    content: '';
    position: absolute;
    bottom: -30px; left: 0; right: 0;
    height: 60px;
    background: var(--cream);
    border-radius: 50% 50% 0 0 / 60px 60px 0 0;
  }

  header h1 {
    font-family: 'Fraunces', serif;
    font-size: clamp(2rem, 5vw, 3rem);
    color: #fff;
    font-weight: 700;
    letter-spacing: -0.5px;
    line-height: 1.1;
  }

  header p {
    color: var(--green-light);
    font-size: 1rem;
    margin-top: 0.5rem;
    font-weight: 300;
  }

  .leaf-deco {
    position: absolute;
    font-size: 4rem;
    opacity: 0.12;
    user-select: none;
  }
  .leaf-deco.l1 { top: -10px; left: 20px; transform: rotate(-20deg); }
  .leaf-deco.l2 { top: 10px; right: 30px; transform: rotate(30deg); }

  main { max-width: 900px; margin: 0 auto; padding: 2rem 1.5rem 4rem; }

  /* GAME SECTION */
  .game-section {
    margin-bottom: 3rem;
  }

  .section-title {
    font-family: 'Fraunces', serif;
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--green);
    margin-bottom: 0.4rem;
  }

  .section-sub {
    font-size: 0.875rem;
    color: var(--muted);
    margin-bottom: 1.5rem;
  }

  /* DRAG GAME */
  .game-board {
    background: var(--white);
    border: 1.5px solid var(--border);
    border-radius: 20px;
    padding: 1.5rem;
  }

  .item-pool {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    min-height: 70px;
    border: 1.5px dashed var(--border);
    border-radius: 12px;
    padding: 12px;
    margin-bottom: 1.5rem;
    align-items: center;
    background: var(--cream);
    transition: background 0.2s;
  }

  .trash-item {
    background: var(--white);
    border: 1.5px solid var(--border);
    border-radius: 100px;
    padding: 7px 14px;
    font-size: 0.85rem;
    cursor: grab;
    display: flex;
    align-items: center;
    gap: 6px;
    transition: transform 0.15s, box-shadow 0.15s, border-color 0.2s;
    user-select: none;
    white-space: nowrap;
  }

  .trash-item:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
  .trash-item.dragging { opacity: 0.4; }
  .trash-item.correct-anim {
    animation: correct-bounce 0.4s ease;
  }
  .trash-item.wrong-anim {
    animation: wrong-shake 0.4s ease;
  }

  @keyframes correct-bounce {
    0%,100%{transform:scale(1)} 40%{transform:scale(1.15)} 70%{transform:scale(0.95)}
  }
  @keyframes wrong-shake {
    0%,100%{transform:translateX(0)} 20%{transform:translateX(-6px)} 60%{transform:translateX(6px)}
  }

  .bins-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
    gap: 12px;
  }

  .bin {
    border-radius: 16px;
    padding: 14px 10px;
    text-align: center;
    border: 2px solid transparent;
    transition: border-color 0.2s, transform 0.15s, background 0.2s;
    cursor: pointer;
    min-height: 120px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
  }

  .bin.drag-over {
    transform: scale(1.04);
    filter: brightness(1.05);
  }

  .bin .bin-icon { font-size: 2.2rem; line-height: 1; }
  .bin .bin-name { font-size: 0.78rem; font-weight: 500; letter-spacing: 0.3px; margin-top: 2px; }
  .bin .bin-count { font-size: 0.72rem; opacity: 0.7; }
  .bin .bin-items { display: flex; flex-wrap: wrap; justify-content: center; gap: 4px; margin-top: 4px; }
  .bin .bin-chip { font-size: 0.65rem; background: rgba(255,255,255,0.5); padding: 2px 7px; border-radius: 100px; }

  .bin-paper  { background: #fff8e1; color: #7c5a00; border-color: #f0d070; }
  .bin-plastic { background: #e3f2fd; color: #1a4f7a; border-color: #90caf9; }
  .bin-glass  { background: #e8f5e9; color: #1a5c30; border-color: #81c784; }
  .bin-metal  { background: #f3f3f3; color: #3a3a3a; border-color: #b0b0b0; }
  .bin-food   { background: #fff3e0; color: #7c3d00; border-color: #ffb74d; }
  .bin-hazard { background: #fce4ec; color: #7c1a25; border-color: #f48fb1; }
  .bin-mixed  { background: #ede7f6; color: #3d1f7c; border-color: #b39ddb; }
  .bin-ewaste { background: #e0f7fa; color: #006064; border-color: #4dd0e1; }

  .score-bar {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1rem;
    flex-wrap: wrap;
  }

  .score-pill {
    background: var(--green);
    color: #fff;
    border-radius: 100px;
    padding: 5px 16px;
    font-size: 0.82rem;
    font-weight: 500;
  }
  .score-pill span { font-weight: 700; font-size: 1rem; }

  .msg {
    font-size: 0.82rem;
    padding: 6px 12px;
    border-radius: 8px;
    font-weight: 500;
    transition: opacity 0.3s;
  }
  .msg.correct { background: var(--green-pale); color: var(--green); }
  .msg.wrong   { background: #fde8e8; color: var(--red); }
  .msg.hidden  { opacity: 0; }

  /* GUIDE SECTION */
  .guide-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 16px;
    margin-top: 0.5rem;
  }

  .guide-card {
    border-radius: 16px;
    padding: 1.2rem 1.2rem 1rem;
    border: 1.5px solid transparent;
  }

  .guide-card h3 {
    font-family: 'Fraunces', serif;
    font-size: 1rem;
    font-weight: 600;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .guide-card ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .guide-card ul li {
    font-size: 0.82rem;
    display: flex;
    align-items: center;
    gap: 7px;
    line-height: 1.4;
  }

  .guide-card ul li::before {
    content: '•';
    font-size: 1rem;
    flex-shrink: 0;
    opacity: 0.6;
  }

  .gc-paper  { background: #fff8e1; border-color: #f0d070; color: #5a3e00; }
  .gc-plastic{ background: #e3f2fd; border-color: #90caf9; color: #0d3a5c; }
  .gc-glass  { background: #e8f5e9; border-color: #81c784; color: #1a5c30; }
  .gc-metal  { background: #f3f3f3; border-color: #b0b0b0; color: #2a2a2a; }
  .gc-food   { background: #fff3e0; border-color: #ffb74d; color: #5c2800; }
  .gc-hazard { background: #fce4ec; border-color: #f48fb1; color: #5c0a14; }

  /* TIPS */
  .tips-row {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 12px;
    margin-top: 0.5rem;
  }

  .tip-card {
    background: var(--white);
    border: 1.5px solid var(--border);
    border-radius: 14px;
    padding: 1rem;
  }

  .tip-card .tip-icon { font-size: 1.5rem; margin-bottom: 6px; }
  .tip-card h4 { font-size: 0.85rem; font-weight: 500; color: var(--green); margin-bottom: 4px; }
  .tip-card p { font-size: 0.78rem; color: var(--muted); line-height: 1.5; }

  footer {
    text-align: center;
    padding: 1.5rem;
    font-size: 0.78rem;
    color: var(--muted);
    border-top: 1px solid var(--border);
  }

  .reset-btn {
    background: none;
    border: 1.5px solid var(--border);
    border-radius: 100px;
    padding: 5px 14px;
    font-size: 0.8rem;
    color: var(--muted);
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    transition: background 0.15s, color 0.15s;
  }
  .reset-btn:hover { background: var(--green-pale); color: var(--green); border-color: var(--green-light); }

  @media (max-width: 480px) {
    .bins-grid { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>

<header>
  <div class="leaf-deco l1">🌿</div>
  <div class="leaf-deco l2">♻️</div>
  <h1>Sopsortering</h1>
  <p>Lär dig sortera rätt – för miljöns skull</p>
</header>

<main>

  <!-- GAME -->
  <section class="game-section">
    <h2 class="section-title">Sorteringsövning</h2>
    <p class="section-sub">Dra varje föremål till rätt behållare – eller klicka på föremålet och sedan behållaren.</p>

    <div class="game-board">
      <div class="score-bar">
        <div class="score-pill">Rätt: <span id="correct-count">0</span> / <span id="total-count">0</span></div>
        <div class="msg hidden" id="feedback-msg">–</div>
        <button class="reset-btn" onclick="resetGame()">↺ Börja om</button>
      </div>

      <div class="item-pool" id="item-pool"></div>

      <div class="bins-grid" id="bins-grid"></div>
    </div>
  </section>

  <!-- GUIDE -->
  <section style="margin-bottom:3rem">
    <h2 class="section-title">Sorteringsguide</h2>
    <p class="section-sub">Vad hör hemma var?</p>
    <div class="guide-grid">
      <div class="guide-card gc-paper">
        <h3>📦 Tidningar &amp; papper</h3>
        <ul>
          <li>Tidningar och tidskrifter</li>
          <li>Kontorspapper, kuvert</li>
          <li>Papperskassar och kartong</li>
          <li>Böcker (utan pärm)</li>
        </ul>
      </div>
      <div class="guide-card gc-plastic">
        <h3>🧴 Plastförpackningar</h3>
        <ul>
          <li>PET-flaskor och petflaskor</li>
          <li>Shampoflaskor, förpackningar</li>
          <li>Plastpåsar och folie</li>
          <li>Yoghurtbägare, matförpackningar</li>
        </ul>
      </div>
      <div class="guide-card gc-glass">
        <h3>🍾 Glasförpackningar</h3>
        <ul>
          <li>Glasflaskor (ej pant)</li>
          <li>Konservburkar av glas</li>
          <li>Glaskrukor och glasburkar</li>
          <li>Fönsterglas ej OK</li>
        </ul>
      </div>
      <div class="guide-card gc-metal">
        <h3>🥫 Metallförpackningar</h3>
        <ul>
          <li>Konservburkar av metall</li>
          <li>Aluminiumfolie (hoprullt)</li>
          <li>Metallkapsyler och lock</li>
          <li>Tomma färgburkar (torra)</li>
        </ul>
      </div>
      <div class="guide-card gc-food">
        <h3>🍂 Matavfall</h3>
        <ul>
          <li>Skal och matrester</li>
          <li>Kaffe- och tepåsar</li>
          <li>Blomster och växter</li>
          <li>Pappersservetter (smutsiga)</li>
        </ul>
      </div>
      <div class="guide-card gc-hazard">
        <h3>⚠️ Farligt avfall</h3>
        <ul>
          <li>Batterier och lampor</li>
          <li>Färg och lösningsmedel</li>
          <li>Läkemedel och kemikalier</li>
          <li>Elektronik (e-avfall)</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- TIPS -->
  <section>
    <h2 class="section-title">Bra att veta</h2>
    <p class="section-sub">Enkla vanor som gör stor skillnad</p>
    <div class="tips-row">
      <div class="tip-card">
        <div class="tip-icon">🚿</div>
        <h4>Skölj rent</h4>
        <p>Skölj ur förpackningar innan de sorteras – smutsiga förpackningar kan förorena hela partier.</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🏷️</div>
        <h4>Kolla symbolen</h4>
        <p>Sök efter återvinningssymbolen och materialförkortningen (t.ex. PP, PET, ALU) på förpackningen.</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🍕</div>
        <h4>Pizzakartongen</h4>
        <p>Är den fet och smutsig? Lägg den i matavfallet. Ren kartong går till papper.</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🔋</div>
        <h4>Batterier</h4>
        <p>Lämna alltid batterier i de röda batterihållarna i butiker eller på återvinningsstationer.</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🏪</div>
        <h4>Pant</h4>
        <p>Flaskor och burkar med pantsymbol lämnas i pantautomaten – inte i återvinningen.</p>
      </div>
    </div>
  </section>

</main>

<footer>
  Sortera rätt – det spelar roll 🌱
</footer>

<script>
const ITEMS = [
  { id: 1, emoji: '📰', label: 'Tidning', bin: 'paper' },
  { id: 2, emoji: '🧴', label: 'Shampoflaska', bin: 'plastic' },
  { id: 3, emoji: '🍾', label: 'Glasflaska', bin: 'glass' },
  { id: 4, emoji: '🥫', label: 'Konservburk', bin: 'metal' },
  { id: 5, emoji: '🍌', label: 'Bananskal', bin: 'food' },
  { id: 6, emoji: '🔋', label: 'Batteri', bin: 'hazard' },
  { id: 7, emoji: '📦', label: 'Kartong', bin: 'paper' },
  { id: 8, emoji: '💊', label: 'Medicin', bin: 'hazard' },
  { id: 9, emoji: '🥤', label: 'PET-flaska', bin: 'plastic' },
  { id: 10, emoji: '🍵', label: 'Tepåse', bin: 'food' },
  { id: 11, emoji: '🫙', label: 'Glasburk', bin: 'glass' },
  { id: 12, emoji: '💡', label: 'Glödlampa', bin: 'hazard' },
  { id: 13, emoji: '📱', label: 'Gammal mobil', bin: 'hazard' },
  { id: 14, emoji: '🥚', label: 'Äggkartong', bin: 'paper' },
  { id: 15, emoji: '🧻', label: 'Pappersrulle', bin: 'paper' },
  { id: 16, emoji: '🫙', label: 'Aluminiumfolie', bin: 'metal' },
  { id: 17, emoji: '☕', label: 'Kaffesump', bin: 'food' },
  { id: 18, emoji: '🍕', label: 'Ren pizzabox', bin: 'paper' },
];

const BINS = [
  { id: 'paper',   icon: '📦', name: 'Tidningar & papper', cls: 'bin-paper' },
  { id: 'plastic', icon: '🧴', name: 'Plastförpackningar', cls: 'bin-plastic' },
  { id: 'glass',   icon: '🍾', name: 'Glasförpackningar',  cls: 'bin-glass' },
  { id: 'metal',   icon: '🥫', name: 'Metallförpackningar',cls: 'bin-metal' },
  { id: 'food',    icon: '🍂', name: 'Matavfall',           cls: 'bin-food' },
  { id: 'hazard',  icon: '⚠️', name: 'Farligt avfall',      cls: 'bin-hazard' },
];

let state = { correct: 0, total: ITEMS.length, selected: null, placed: {} };
let shuffled = [];

function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length-1; i>0; i--) {
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]] = [a[j],a[i]];
  }
  return a;
}

function resetGame() {
  state = { correct: 0, total: ITEMS.length, selected: null, placed: {} };
  shuffled = shuffle(ITEMS);
  renderPool();
  renderBins();
  updateScore();
  hideFeedback();
}

function updateScore() {
  document.getElementById('correct-count').textContent = state.correct;
  document.getElementById('total-count').textContent = state.total;
}

function showFeedback(text, type) {
  const el = document.getElementById('feedback-msg');
  el.textContent = text;
  el.className = 'msg ' + type;
  clearTimeout(el._t);
  el._t = setTimeout(() => el.classList.add('hidden'), 2200);
}
function hideFeedback() {
  const el = document.getElementById('feedback-msg');
  el.className = 'msg hidden';
}

function renderPool() {
  const pool = document.getElementById('item-pool');
  pool.innerHTML = '';
  shuffled.forEach(item => {
    if (state.placed[item.id]) return;
    const el = document.createElement('div');
    el.className = 'trash-item';
    el.dataset.id = item.id;
    el.draggable = true;
    el.innerHTML = `<span style="font-size:1.1rem">${item.emoji}</span>${item.label}`;

    if (state.selected === item.id) {
      el.style.borderColor = '#2d6a4f';
      el.style.background = '#d8f3dc';
    }

    el.addEventListener('click', () => selectItem(item.id));
    el.addEventListener('dragstart', e => {
      e.dataTransfer.setData('itemId', item.id);
      el.classList.add('dragging');
    });
    el.addEventListener('dragend', () => el.classList.remove('dragging'));
    pool.appendChild(el);
  });

  if (pool.children.length === 0) {
    pool.innerHTML = '<p style="color:var(--muted);font-size:0.85rem;margin:auto">Bra jobbat! Alla föremål sorterade 🎉</p>';
  }
}

function renderBins() {
  const grid = document.getElementById('bins-grid');
  grid.innerHTML = '';
  BINS.forEach(bin => {
    const el = document.createElement('div');
    el.className = 'bin ' + bin.cls;
    el.dataset.bin = bin.id;

    const items = Object.entries(state.placed).filter(([,b])=>b===bin.id).map(([id])=>ITEMS.find(i=>i.id==id));
    const chipsHtml = items.map(i=>`<span class="bin-chip">${i.emoji} ${i.label}</span>`).join('');

    el.innerHTML = `
      <div class="bin-icon">${bin.icon}</div>
      <div class="bin-name">${bin.name}</div>
      <div class="bin-count">${items.length} föremål</div>
      <div class="bin-items">${chipsHtml}</div>`;

    el.addEventListener('click', () => dropToSelected(bin.id));
    el.addEventListener('dragover', e => { e.preventDefault(); el.classList.add('drag-over'); });
    el.addEventListener('dragleave', () => el.classList.remove('drag-over'));
    el.addEventListener('drop', e => {
      e.preventDefault();
      el.classList.remove('drag-over');
      const itemId = parseInt(e.dataTransfer.getData('itemId'));
      placeItem(itemId, bin.id);
    });

    grid.appendChild(el);
  });
}

function selectItem(id) {
  state.selected = state.selected === id ? null : id;
  renderPool();
}

function dropToSelected(binId) {
  if (state.selected !== null) {
    placeItem(state.selected, binId);
    state.selected = null;
  }
}

function placeItem(itemId, binId) {
  const item = ITEMS.find(i => i.id === itemId);
  if (!item || state.placed[itemId]) return;

  if (item.bin === binId) {
    state.correct++;
    state.placed[itemId] = binId;
    showFeedback(`✓ Rätt! ${item.emoji} ${item.label} hör till ${BINS.find(b=>b.id===binId).name}.`, 'correct');
  } else {
    const correctBin = BINS.find(b => b.id === item.bin);
    showFeedback(`✗ Fel! ${item.emoji} ${item.label} ska till ${correctBin.name}.`, 'wrong');
  }

  updateScore();
  renderPool();
  renderBins();
}

resetGame();
</script>
</body>
</html>

