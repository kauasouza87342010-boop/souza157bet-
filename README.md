# souza157bet-<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Souza157 Bet - Versão 6 Avançada</title>
<style>
  body { font-family: Arial, sans-serif; background: #1a1a1a; color: #fff; margin:0; padding:0; }
  header { background:#c60000; padding:15px; text-align:center; font-size:2em; font-weight:bold; color:gold; }
  nav { display:flex; justify-content:center; background:#333; flex-wrap:wrap; }
  nav button { margin:5px; padding:10px 20px; background:#c60000; border:none; color:gold; font-weight:bold; cursor:pointer; border-radius:5px; }
  nav button:hover { background:gold; color:#c60000; }
  .container { padding:20px; text-align:center; }
  .saldo { font-size:1.5em; margin-bottom:20px; }
  .game, .admin-panel, .ranking, .stats { display:none; margin-top:20px; }
  input[type=number], input[type=text], select { padding:5px; border-radius:5px; border:none; text-align:center; margin-right:5px; }
  .result { margin-top:15px; font-size:1.2em; color:gold; }
  table { margin:20px auto; border-collapse:collapse; width:80%; }
  th, td { border:1px solid gold; padding:8px; }
  th { background:#c60000; color:gold; }
  h2 { margin-bottom:10px; }
</style>
</head>
<body>

<header>Souza157 Bet - Versão 6 Avançada</header>

<nav>
  <button onclick="showPanel('jogos')">Jogador</button>
  <button onclick="showPanel('admin')">Admin</button>
  <button onclick="showPanel('ranking')">Ranking</button>
  <button onclick="showPanel('stats')">Estatísticas</button>
</nav>

<div class="container">
  <div class="saldo">Saldo: R$<span id="saldo">1000</span></div>

  <!-- Painel do Jogador -->
  <div id="jogos" class="game">
    <h2>Jogos</h2>
    <button onclick="showGame('roleta')">Roleta</button>
    <button onclick="showGame('caca')">Caça-níqueis</button>
    <button onclick="showGame('blackjack')">Blackjack</button>
    <button onclick="showGame('poker')">Poker</button>
    <button onclick="showGame('apostas')">Apostas Esportivas</button>

    <!-- Roleta -->
    <div id="roleta" style="display:none;">
      <p>Aposte em Vermelho, Preto ou Verde (0)</p>
      <input type="text" id="apostaCor" placeholder="Cor (vermelho/preto/verde)">
      <input type="number" id="apostaValor" placeholder="Valor">
      <button onclick="jogarRoleta()">Apostar</button>
      <div class="result" id="resultadoRoleta"></div>
    </div>

    <!-- Caça-níqueis -->
    <div id="caca" style="display:none;">
      <p>Três rolos: 💎 🍒 7 ⭐</p>
      <input type="number" id="apostaCaca" placeholder="Valor">
      <button onclick="jogarCaca()">Jogar</button>
      <div class="result" id="resultadoCaca"></div>
    </div>

    <!-- Blackjack -->
    <div id="blackjack" style="display:none;">
      <input type="number" id="apostaBJ" placeholder="Valor">
      <button onclick="jogarBJ()">Jogar</button>
      <div class="result" id="resultadoBJ"></div>
    </div>

    <!-- Poker -->
    <div id="poker" style="display:none;">
      <input type="number" id="apostaPoker" placeholder="Valor">
      <button onclick="jogarPoker()">Jogar</button>
      <div class="result" id="resultadoPoker"></div>
    </div>

    <!-- Apostas Esportivas -->
    <div id="apostas" style="display:none;">
      <select id="time">
        <option value="flamengo">Flamengo</option>
        <option value="empate">Empate</option>
        <option value="palmeiras">Palmeiras</option>
      </select>
      <input type="number" id="apostaEsportiva" placeholder="Valor">
      <button onclick="apostarEsporte()">Apostar</button>
      <div class="result" id="resultadoEsporte"></div>
    </div>
  </div>

  <!-- Painel de Admin -->
  <div id="admin" class="admin-panel">
    <h2>Painel do Administrador</h2>
    <p>Gerenciar jogadores</p>
    <input type="text" id="adminJogador" placeholder="Nome do jogador">
    <input type="number" id="adminValor" placeholder="Adicionar/Remover">
    <button onclick="alterarSaldo()">Alterar Saldo</button>
    <div class="result" id="resultadoAdmin"></div>
  </div>

  <!-- Ranking -->
  <div id="ranking" class="ranking">
    <h2>Ranking de Jogadores</h2>
    <table>
      <thead>
        <tr><th>Jogador</th><th>Saldo</th></tr>
      </thead>
      <tbody id="rankingBody"></tbody>
    </table>
  </div>

  <!-- Estatísticas -->
  <div id="stats" class="stats">
    <h2>Estatísticas</h2>
    <p>Total de apostas: <span id="totalApostas">0</span></p>
    <p>Saldo total da plataforma: R$<span id="saldoPlataforma">1000</span></p>
    <p>Ganho total dos jogadores: R$<span id="ganhos">0</span></p>
  </div>

</div>

<script>
// Sistema
let saldo = 1000;
let historico = [];
let jogadores = {'Você': saldo};
let totalApostas = 0;
let ganhos = 0;

function showPanel(panel){
  ['jogos','admin','ranking','stats'].forEach(p => document.getElementById(p).style.display='none');
  document.getElementById(panel).style.display='block';
  if(panel==='ranking') atualizarRanking();
  if(panel==='stats') atualizarStats();
}

function showGame(game){
  ['roleta','caca','blackjack','poker','apostas'].forEach(g => document.getElementById(g).style.display='none');
  document.getElementById(game).style.display='block';
}

function atualizarSaldo(){
  document.getElementById('saldo').innerText = saldo;
  jogadores['Você'] = saldo;
  document.getElementById('saldoPlataforma').innerText = Object.values(jogadores).reduce((a,b)=>a+b,0);
}

function addHistorico(jogo, aposta, valor, resultado){
  historico.push({jogo,aposta,valor,resultado});
  totalApostas++;
  if(resultado==='Ganhou') ganhos += valor;
}

// Ranking
function atualizarRanking(){
  let tbody = document.getElementById('rankingBody');
  tbody.innerHTML = '';
  Object.entries(jogadores).sort((a,b)=>b[1]-a[1]).forEach(([nome,saldo])=>{
    tbody.innerHTML += `<tr><td>${nome}</td><td>R$${saldo}</td></tr>`;
  });
}

// Estatísticas
function atualizarStats(){
  document.getElementById('totalApostas').innerText = totalApostas;
  document.getElementById('ganhos').innerText = ganhos;
  document.getElementById('saldoPlataforma').innerText = Object.values(jogadores).reduce((a,b)=>a+b,0);
}

// Promoção diária automática
setInterval(()=>{
  let bonus = Math.floor(Math.random()*50)+10;
  saldo += bonus;
  alert(`Promoção diária! Você ganhou R$${bonus} de bônus.`);
  atualizarSaldo();
}, 60000); // 1 minuto = simulação de "diária"

// Admin
function alterarSaldo(){
  let nome = document.getElementById('adminJogador').value;
  let valor = parseInt(document.getElementById('adminValor').value);
  if(!jogadores[nome]) jogadores[nome] = 0;
  jogadores[nome] += valor;
  document.getElementById('resultadoAdmin').innerText = `Saldo de ${nome} alterado para R$${jogadores[nome]}`;
  atualizarRanking();
  atualizarSaldo();
}

// --- Jogos ---
function jogarRoleta(){
  let cor = document.getElementById('apostaCor').value.toLowerCase();
  let valor = parseInt(document.getElementById('apostaValor').value);
  if(valor>saldo){ alert('Saldo insuficiente'); return; }
  saldo -= valor;
  const cores = ['vermelho','preto','verde'];
  let resultado = cores[Math.floor(Math.random()*3)];
  let ganho=0;
  if(cor===resultado){ ganho=cor==='verde'?valor*14:valor*2; saldo+=ganho; document.getElementById('resultadoRoleta').innerText=`Ganhou! ${resultado} - R$${ganho}`; addHistorico('Roleta',cor,valor,'Ganhou'); }
  else { document.getElementById('resultadoRoleta').innerText=`Perdeu! ${resultado}`; addHistorico('Roleta',cor,valor,'Perdeu'); }
  atualizarSaldo();
}

function jogarCaca(){
  let valor = parseInt(document.getElementById('apostaCaca').value);
  if(valor>saldo){ alert('Saldo insuficiente'); return; }
  saldo -= valor;
  const s=['💎','🍒','7','⭐'];
  let r=[s[Math.floor(Math.random()*4)],s[Math.floor(Math.random()*4)],s[Math.floor(Math.random()*4)]];
  let ganho=0;
  if(r[0]===r[1]&&r[1]===r[2]){ ganho=valor*10; saldo+=ganho; document.getElementById('resultadoCaca').innerText=`Ganhou! ${r.join(' ')} - R$${ganho}`; addHistorico('Caça-níqueis',r.join(' '),valor,'Ganhou'); }
  else { document.getElementById('resultadoCaca').innerText=`Perdeu! ${r.join(' ')}`; addHistorico('Caça-níqueis',r.join(' '),valor,'Perdeu'); }
  atualizarSaldo();
}

function jogarBJ(){
  let valor=parseInt(document.getElementById('apostaBJ').value); if(valor>saldo){ alert('Saldo insuficiente'); return; } saldo-=valor;
  let jogador=Math.floor(Math.random()*21)+1; let dealer=Math.floor(Math.random()*21)+1; let ganho=0;
  if(jogador>dealer){ ganho=valor*2; saldo+=ganho; document.getElementById('resultadoBJ').innerText=`Ganhou! Você:${jogador} Dealer:${dealer}`; addHistorico('Blackjack','Valor apostado',valor,'Ganhou'); }
  else if(jogador===dealer){ saldo+=valor; document.getElementById('resultadoBJ').innerText=`Empate! Você:${jogador} Dealer:${dealer}`; addHistorico('Blackjack','Valor apostado',valor,'Empate'); }
  else { document.getElementById('resultadoBJ').innerText=`Perdeu! Você:${jogador} Dealer:${dealer}`; addHistorico('Blackjack','Valor apostado',valor,'Perdeu'); }
  atualizarSaldo();
}

function jogarPoker(){
  let valor=parseInt(document.getElementById('apostaPoker').value); if(valor>saldo){ alert('Saldo insuficiente'); return; } saldo-=valor;
  let jogador=Math.floor(Math.random()*100)+1; let dealer=Math.floor(Math.random()*100)+1; let ganho=0;
  if(jogador>dealer){ ganho=valor*2; saldo+=ganho; document.getElementById('resultadoPoker').innerText=`Ganhou! Mão:${jogador} Dealer:${dealer}`; addHistorico('Poker','Valor apostado',valor,'Ganhou'); }
  else { document.getElementById('resultadoPoker').innerText=`Perdeu! Mão:${jogador} Dealer:${dealer}`; addHistorico('Poker','Valor apostado',valor,'Perdeu'); }
  atualizarSaldo();
}

function apostarEsporte(){
  let aposta=document.getElementById('time').value; let valor=parseInt(document.getElementById('apostaEsportiva').value);
  if(valor>saldo){ alert('Saldo insuficiente'); return; } saldo-=valor;
  const odds={'flamengo':1.8,'empate':3,'palmeiras':2.5};
  const resultadoReal=['flamengo','empate','palmeiras'][Math.floor(Math.random()*3)];
  let ganho=0;
  if(aposta===resultadoReal){ ganho=valor*odds[aposta]; saldo+=ganho; document.getElementById('resultadoEsporte').innerText=`Ganhou! Resultado:${resultadoReal} - R$${ganho.toFixed(2)}`; addHistorico('Aposta Esportiva',aposta,valor,'Ganhou'); }
  else { document.getElementById('resultadoEsporte').innerText=`Perdeu! Resultado:${resultadoReal}`; addHistorico('Aposta Esportiva',aposta,valor,'Perdeu'); }
  atualizarSaldo();
}

atualizarSaldo();
</script>

</body>
</html>
