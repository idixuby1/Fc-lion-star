<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC</title>

<style>
body{
margin:0;
font-family:Arial;
background:#0b6623;
color:white;
text-align:center;
}

header{
background:#111;
padding:20px;
}

header img{
width:120px;
border-radius:50%;
}

section{
background:white;
color:black;
margin:20px auto;
padding:20px;
border-radius:12px;
width:90%;
max-width:900px;
}

.pitch{
position:relative;
height:500px;
background:green;
border:4px solid white;
border-radius:10px;
}

.player{
position:absolute;
background:white;
padding:5px 10px;
border-radius:15px;
font-size:12px;
cursor:grab;
}

button, input, select{
padding:10px;
margin:5px;
border-radius:5px;
border:none;
}

button{
background:#111;
color:white;
}

</style>
</head>

<body>

<header>
<img src="logo.png">
<h1>⚽ Lion Star FC</h1>
</header>

<!-- ABOUT TEAM -->
<section>
<h2>About Team</h2>
<p>
Lion Star FC is a strong and passionate football club focused on teamwork,
discipline, and winning mentality. We develop young talents and play with pride.
</p>
</section>

<!-- ADMIN -->
<section>
<h2>Admin Panel</h2>

<input type="password" id="pass" placeholder="Password"><br>

<input type="text" id="name" placeholder="Player name">

<select id="formation" onchange="setFormation()">
<option value="433">4-3-3</option>
<option value="442">4-4-2</option>
<option value="352">3-5-2</option>
</select>

<br>

<button onclick="addPlayer()">Add Player</button>
<button onclick="clearTeam()">Clear Team</button>

</section>

<!-- PITCH -->
<section>
<h2>Starting XI</h2>
<div class="pitch" id="pitch"></div>
</section>

<script>

let maxPlayers = 11;
let formation = "433";

const positions = {

"433":[
{top:450,left:45}, // GK
{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},
{top:250,left:25},{top:250,left:45},{top:250,left:65},
{top:100,left:25},{top:80,left:45},{top:100,left:65}
],

"442":[
{top:450,left:45},
{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},
{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},
{top:100,left:35},{top:100,left:55}
],

"352":[
{top:450,left:45},
{top:350,left:25},{top:350,left:45},{top:350,left:65},
{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},
{top:100,left:35},{top:100,left:55}
]

};

function setFormation(){
formation = document.getElementById("formation").value;
reloadTeam();
}

function addPlayer(){

let password = document.getElementById("pass").value;
if(password !== "1234"){ alert("Wrong password"); return; }

let team = JSON.parse(localStorage.getItem("team")) || [];

if(team.length >= maxPlayers){
alert("Only 11 players allowed!");
return;
}

let name = document.getElementById("name").value;

team.push({name:name});
localStorage.setItem("team", JSON.stringify(team));

reloadTeam();
}

function reloadTeam(){

let pitch = document.getElementById("pitch");
pitch.innerHTML = "";

let team = JSON.parse(localStorage.getItem("team")) || [];

team.forEach((p,i)=>{

let pos = positions[formation][i];

let div = document.createElement("div");
div.className = "player";
div.style.top = pos.top + "px";
div.style.left = pos.left + "%";
div.textContent = p.name;

// REMOVE PLAYER
div.onclick = function(){
team.splice(i,1);
localStorage.setItem("team", JSON.stringify(team));
reloadTeam();
};

pitch.appendChild(div);

});

}

function clearTeam(){
localStorage.removeItem("team");
reloadTeam();
}

reloadTeam();

</script>

</body>
</html>
