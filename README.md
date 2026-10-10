<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Minesweeper Setan 👹</title>
<style>
  body { background: #0d0000; color: #ff3333; font-family: 'Courier New', monospace; text-align: center; }
  h1 { text-shadow: 0 0 10px #ff0000; }
  #grid { display: grid; grid-template-columns: repeat(8, 40px); gap: 2px; justify-content: center; margin: 20px auto; }
  .cell { width: 40px; height: 40px; background: #220000; border: 1px solid #440000; display: flex; align-items: center; justify-content: center; font-size: 20px; font-weight: bold; cursor: pointer; user-select: none; }
  .cell.revealed { background: #550000; color: #ff6666; cursor: default; }
  .cell.mine { background: #990000; }
  button { background: #440000; color: #ff3333; border: 2px solid #ff3333; padding: 10px 20px; font-weight: bold; cursor: pointer; text-transform: uppercase; margin-top: 15px; }
  button:hover { background: #ff3333; color: #000; }
</style>
</head>
<body>
  <h1>👹 MINESWEEPER NERAKA 👹</h1>
  <div style="font-size: 18px;">WAKTU: <span id="time">0</span>s | SKOR: <span id="score">0</span></div>
  <div id="grid"></div>
  <button onclick="initGame()">COBA LAGI 🩸</button>

<script>
const size = 8, minesCount = 8;
let grid = [], cells = [], gameOver = false, timer, seconds = 0, score = 0;

function initGame() {
  clearInterval(timer); seconds = 0; score = 0; gameOver = false;
  document.getElementById('time').innerText = 0;
  document.getElementById('score').innerText = 0;
  timer = setInterval(() => { if(!gameOver) { seconds++; document.getElementById('time').innerText = seconds; } }, 1000);
  
  const g = document.getElementById('grid'); g.innerHTML = '';
  grid = Array(size).fill().map(() => Array(size).fill(0));
  cells = Array(size).fill().map(() => Array(size));

  let placed = 0;
  while(placed < minesCount) {
    let r = Math.floor(Math.random()*size), c = Math.floor(Math.random()*size);
    if(grid[r][c] !== 'M') { grid[r][c] = 'M'; placed++; }
  }

  for(let r=0; r<size; r++) {
    for(let c=0; c<size; c++) {
      if(grid[r][c] !== 'M') {
        let count = 0;
        for(let i=-1; i<=1; i++) {
          for(let j=-1; j<=1; j++) {
            if(grid[r+i]?.[c+j] === 'M') count++;
          }
        }
        grid[r][c] = count;
      }
      const el = document.createElement('div');
      el.className = 'cell';
      el.onclick = () => reveal(r, c);
      g.appendChild(el);
      cells[r][c] = el;
    }
  }
}

function reveal(r, c) {
  if(gameOver || cells[r][c].classList.contains('revealed')) return;
  cells[r][c].classList.add('revealed');
  
  if(grid[r][c] === 'M') {
    cells[r][c].classList.add('mine');
    cells[r][c].innerText = '💀';
    gameOver = true; clearInterval(timer);
    alert('Kamu Teror Setan! Game Over 👻');
  } else {
    cells[r][c].innerText = grid[r][c] || '';
    score += 10;
    document.getElementById('score').innerText = score;
    if(score === (64 - minesCount) * 10) {
      gameOver = true; clearInterval(timer);
      alert('Selamat! Kamu berhasil lolos dari cengkeraman setan! 🎉');
    }
  }
}

initGame();
</script>
</body>
</html>

