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

/* HEADER */
header{
background:#111;
padding:20px;
}

header img{
width:120px;
border-radius:50%;
}

/* SECTION */
section{
background:white;
color:black;
margin:20px auto;
padding:20px;
border-radius:10px;
width:90%;
max-width:900px;
}

/* PITCH */
.pitch{
position:relative;
width:100%;
height:500px;
background:green;
border:5px solid white;
border-radius:10px;
}

.player{
position:absolute;
background:white;
color:black;
padding:5px 10px;
border-radius:20px;
font-size:12px;
}

/* FORMATION POSITIONS */
.gk{ bottom:10px; left:45%; }

.df1{ bottom:80px; left:15%; }
.df2{ bottom:80px; left:35%; }
.df3{ bottom:80px; left:55%; }
.df4{ bottom:80px; left:75%; }

.mf1{ bottom:200px; left:25%; }
.mf2{ bottom:200px; left:45%; }
.mf3{ bottom:200px; left:65%; }

.fw1{ top:50px; left:25%; }
.fw2{ top:30px; left:45%; }
.fw3{ top:50px; left:65%; }

/* INPUT */
input, select, button{
padding:10px;
margin:5px;
border-radius:5px;
border:none;
}

button{
background:#111;
color:white;
cursor:pointer;
}

</style>
</head>

<body>

<header>
<img src="logo.png">
<h1>⚽ Lion Star FC</h1>
</header>

<section>

<h2>Admin Panel (Add Player)</h2>

<input type="password" id="pass" placeholder="Enter password">
<br>

<input type="text" id="name" placeholder="Player name">

<select id="position">
<option value="gk">Goalkeeper</option>
<option value="df1">Defender 1</option>
<option value="df2">Defender 2</option>
<option value="df3">Defender 3</option>
<option value="df4">Defender 4</option>
<option value="mf1">Midfielder 1</option>
<option value="mf2">Midfielder 2</option>
<option value="mf3">Midfielder 3</option>
<option value="fw1">Forward 1</option>
<option value="fw2">Striker</option>
<option value="fw3">Forward 2</option>
</select>

<br>

<button onclick="addPlayer()">Add to Pitch</button>

</section>

<section>

<h2>Starting XI (4-3-3 Formation)</h2>

<div class="pitch" id="pitch"></div>

</section>

<script>

// LOAD SAVED TEAM
window.onload = function(){
let saved = JSON.parse(localStorage.getItem("team")) || [];
saved.forEach(p => displayPlayer(p.name, p.pos));
}

function addPlayer(){

let password = document.getElementById("pass").value;

if(password !== "1234"){
alert("Wrong password!");
return;
}

let name = document.getElementById("name").value;
let pos = document.getElementById("position").value;

displayPlayer(name, pos);

// SAVE
let team = JSON.parse(localStorage.getItem("team")) || [];
team.push({name:name, pos:pos});
localStorage.setItem("team", JSON.stringify(team));

}

function displayPlayer(name, pos){

let pitch = document.getElementById("pitch");

let player = document.createElement("div");
player.className = "player " + pos;
player.textContent = name;

pitch.appendChild(player);

}

</script>

</body>
</html>
