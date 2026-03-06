# -Gplace
Publica tus logros envia un gmail a manosamarcel@gmail.com con lo que quieres poner y en cueston de minutos ya lo tendras publicado pero nescesitas una prueva para verificar
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Gplace</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🏆</text></svg>">
<style>
body{
font-family:Arial;
background:#0b1d3a;
color:white;
text-align:center;
margin:0;
}

#start{
height:100vh;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
}

button{
padding:12px 25px;
margin:8px;
border:none;
border-radius:10px;
cursor:pointer;
font-weight:bold;
font-size:16px;
}

button:hover{opacity:0.9;}

.green{background:#2ecc71;color:white;}
.red{background:#e74c3c;color:white;}

input{
padding:12px;
margin:8px;
border-radius:10px;
border:1px solid #000;
background:#050c18;
color:white;
width:300px;
font-size:16px;
}

input::placeholder{color:#8892b0;}

#gplaceHeader{
display:flex;
align-items:center;
gap:12px;
justify-content:center;
margin-top:20px;
font-size:32px;
font-weight:bold;
}

#topbar{
position:fixed;
top:10px;
left:10px;
background:#020814;
padding:10px 20px;
border-radius:25px;
font-weight:bold;
box-shadow:0 0 12px rgba(0,0,0,0.6);
font-size:16px;
}

#version{position:fixed;bottom:10px;right:10px;color:#9aa4c7;font-size:14px;}

.record{
background:#050c18;
margin:15px auto;
padding:15px;
border-radius:12px;
width:350px;
box-shadow:0 0 10px rgba(0,0,0,0.6);
}

h1{font-size:80px;margin-bottom:10px;}
h2{margin-top:80px;font-size:28px;}
</style>
</head>
<body>

<div id="topbar">👥 Conectados: <span id="online">1</span></div>

<div id="start">
<div id="gplaceHeader"><span>🏆</span><span>Gplace</span></div>
<p>Actualización 3.4.121</p>
<button onclick="openRecords()">Ver records</button>
</div>

<div id="app" style="display:none">
<div id="gplaceHeader"><span>🏆</span><span>Gplace</span></div>
<h2>Records</h2>

<input id="name" placeholder="Tu nombre">
<input id="recordType" placeholder="Tipo de récord (ej: bottleflip)">
<input id="score" placeholder="Resultado">
<input id="recordPass" type="password" placeholder="Contraseña para publicar">
<br>
<button onclick="addRecord()">Guardar récord</button>
<br><br>
<input id="search" placeholder="Buscar récord..." oninput="render()">
<div id="records"></div>
</div>

<div id="version">version 3.4.121</div>

<script>
let records = JSON.parse(localStorage.getItem("gplace_records")) || [];
let online = JSON.parse(localStorage.getItem("gplace_online")) || [];

function emoji(type){
 type = type.toLowerCase();
 if(type.includes("bottle")) return "🍾";
 if(type.includes("speed")) return "⚡";
 if(type.includes("kill")) return "💥";
 if(type.includes("puntos")) return "⭐";
 return "🎮";
}

function openRecords(){
 document.getElementById("start").style.display="none";
 document.getElementById("app").style.display="block";
 render();
}

function addRecord(){
 let name = document.getElementById("name").value;
 let type = document.getElementById("recordType").value;
 let score = document.getElementById("score").value;
 let pass = document.getElementById("recordPass").value;

 if(!name || !type || !score){alert("Completa todos los campos"); return;}
 if(pass !== "Gplace26"){alert("Contraseña incorrecta para publicar"); return;}

 records.push({name,type,score});
 localStorage.setItem("gplace_records",JSON.stringify(records));
 render();
}

function render(){
 let container = document.getElementById("records");
 container.innerHTML="";
 let search = document.getElementById("search").value.toLowerCase();
 records.filter(r=>r.name.toLowerCase().includes(search)||r.type.toLowerCase().includes(search)).forEach((r,i)=>{
  let div = document.createElement("div");
  div.className="record";
  let medal="";
  if(i===0) medal="🥇";
  else if(i===1) medal="🥈";
  else if(i===2) medal="🥉";
  div.innerHTML=`<h3>${medal} ${emoji(r.type)} ${r.type}</h3><p>${r.name} — ${r.score}</p><button onclick="deleteRecord(${i})">Borrar</button>`;
  container.appendChild(div);
 });
}

function deleteRecord(i){
 let pass = prompt("Introducir contraseña para borrar");
 if(pass !== "Gplace26"){alert("Contraseña incorrecta"); return;}
 if(confirm("¿Estas seguro de que quieres borrar el record?")){
  records.splice(i,1);
  localStorage.setItem("gplace_records",JSON.stringify(records));
  render();
 }
}

render();
</script>
</body>
</html>
