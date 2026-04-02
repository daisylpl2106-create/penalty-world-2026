<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Penalty World 2026</title>
<style>
body { font-family: Arial; background:#0b6623; color:white; text-align:center; }
button { padding:10px; margin:5px; cursor:pointer; }
.hidden { display:none; }
.goal { margin:20px auto; width:300px; height:150px; border:5px solid white; position:relative; }
.keeper { position:absolute; bottom:0; left:120px; font-size:40px; }
</style>
</head>

<body>

<h1>🏆 Penalty World 2026</h1>

<div id="menu">
  <button onclick="startGame()">JUGAR MUNDIAL</button>
</div>

<div id="teamSelect" class="hidden">
  <h2>Elige tu equipo</h2>
  <div id="teams"></div>
</div>

<div id="groupStage" class="hidden">
  <h2>Fase de Grupos</h2>
  <div id="groupTable"></div>
  <button onclick="playNextMatch()">Jugar siguiente partido</button>
</div>

<div id="match" class="hidden">
  <h2 id="matchTitle"></h2>

  <div class="goal">
    <div id="keeper" class="keeper">🧍‍♂️</div>
  </div>

  <h3 id="result"></h3>

  <button onclick="shoot('left')">⬅️</button>
  <button onclick="shoot('center')">⬆️</button>
  <button onclick="shoot('right')">➡️</button>

  <p id="score"></p>
</div>

<div id="knockout" class="hidden">
  <h2>Eliminatorias</h2>
  <div id="bracket"></div>
  <button onclick="playKnockout()">Jugar</button>
</div>

<div id="winner" class="hidden">
  <h1>🏆 CAMPEÓN</h1>
  <h2 id="champion"></h2>
</div>

<script>
const teams = [
"Argentina","Brasil","Francia","España",
"México","USA","Alemania","Portugal"
];

let playerTeam = "";
let group = [];
let matches = [];
let currentMatch = 0;
let goals = 0;
let shots = 0;

function startGame(){
  document.getElementById("menu").classList.add("hidden");
  document.getElementById("teamSelect").classList.remove("hidden");

  let html = "";
  teams.forEach(t=>{
    html += `<button onclick="selectTeam('${t}')">${t}</button>`;
  });
  document.getElementById("teams").innerHTML = html;
}

function selectTeam(team){
  playerTeam = team;
  group = [...teams].sort(()=>0.5-Math.random()).slice(0,4);
  generateMatches();

  document.getElementById("teamSelect").classList.add("hidden");
  document.getElementById("groupStage").classList.remove("hidden");
  renderTable();
}

function generateMatches(){
  matches = [];
  for(let i=0;i<group.length;i++){
    for(let j=i+1;j<group.length;j++){
      matches.push([group[i],group[j]]);
    }
  }
}

function renderTable(){
  let html = "<h3>Grupo</h3>";
  group.forEach(t=>{
    html += `<p>${t}</p>`;
  });
  document.getElementById("groupTable").innerHTML = html;
}

function playNextMatch(){
  if(currentMatch >= matches.length){
    startKnockout();
    return;
  }

  let m = matches[currentMatch];
  document.getElementById("groupStage").classList.add("hidden");
  document.getElementById("match").classList.remove("hidden");

  document.getElementById("matchTitle").innerText = m[0]+" vs "+m[1];

  goals = 0;
  shots = 0;
  updateScore();
}

function shoot(dir){
  const opts = ["left","center","right"];
  const keeper = opts[Math.floor(Math.random()*3)];
  const k = document.getElementById("keeper");

  if(keeper==="left") k.style.left="20px";
  if(keeper==="center") k.style.left="120px";
  if(keeper==="right") k.style.left="220px";

  shots++;

  if(dir===keeper){
    document.getElementById("result").innerText="ATAJADO";
  } else {
    document.getElementById("result").innerText="GOOOL";
    goals++;
  }

  updateScore();

  if(shots>=5){
    endMatch();
  }
}

function updateScore(){
  document.getElementById("score").innerText =
  "Goles: "+goals+" / 5";
}

function endMatch(){
  currentMatch++;
  document.getElementById("match").classList.add("hidden");
  document.getElementById("groupStage").classList.remove("hidden");
}

function startKnockout(){
  document.getElementById("groupStage").classList.add("hidden");
  document.getElementById("knockout").classList.remove("hidden");

  document.getElementById("bracket").innerText =
  "Semifinal → Final";
}

function playKnockout(){
  document.getElementById("knockout").classList.add("hidden");
  document.getElementById("winner").classList.remove("hidden");

  let champ = teams[Math.floor(Math.random()*teams.length)];
  document.getElementById("champion").innerText = champ;
}
</script>

</body>
</html># penalty-world-2026
