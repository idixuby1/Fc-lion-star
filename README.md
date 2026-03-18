
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
width:60px;
text-align:center;
cursor:grab;
}
.player img{
width:60px;
height:60px;
border-radius:50%;
border:2px solid white;
}
.player span{
display:block;
font-size:12px;
background:white;
color:black;
border-radius:10px;
margin-top:2px;
}
button,input,select{
padding:10px;
margin:5px;
border-radius:5px;
border:none;
}
button{
background:#111;
color:white;
}
button:hover{
background:#444;
}
footer{
background:#111;
padding:15px;
color:#aaa;
margin-top:20px;
}
</style>
</head>

<body>

<header>
<img src="images - 2026-03-03T054620.224.jpeg">
<h1>⚽ Lion Star FC</h1>
</header>

<section>
<h2>About Team</h2>
<p>
Lion Star FC is a strong and passionate football club focused on teamwork,
discipline, and winning mentality.
</p>
</section>

<!-- ADMIN PANEL -->
<section>
<h2>Admin Panel</h2>
<input type="password" id="pass" placeholder="Password"><br>

<h3>Add Player Info</h3>
<input type="text" id="playerName" placeholder="Player name">
<button onclick="addPlayerInfo()">Add Player</button>

<h3>Add Match</h3>
<input type="text" id="matchText" placeholder="Match result">
<button onclick="addMatch()">Add Match</button>

<h3>Add News</h3>
<input type="text" id="newsText" placeholder="News update">
<button onclick="addNews()">Add News</button>

<h3>Add Gallery Image</h3>
<input type="file" id="galleryImage">
<button onclick="addGallery()">Add Image</button>

<br><br>

<input type="text" id="name" placeholder="Player name">
<input type="file" id="image"><br>

<select id="formation" onchange="setFormation()">
<option value="433">4-3-3</option>
<option value="442">4-4-2</option>
<option value="352">3-5-2</option>
</select><br>

<button onclick="addPlayer()">Add Player to Pitch</button>
<button onclick="clearTeam()">Clear Team</button>
</section>

<!-- DYNAMIC SECTIONS -->
<section id="playersSection" style="display:none;">
<h2>Players</h2>
<div id="playersList"></div>
</section>

<section id="matchesSection" style="display:none;">
<h2>Matches</h2>
<div id="matchesList"></div>
</section>

<section id="newsSection" style="display:none;">
<h2>News</h2>
<div id="newsList"></div>
</section>

<section id="gallerySection" style="display:none;">
<h2>Gallery</h2>
<div id="galleryList"></div>
</section>

<!-- PITCH -->
<section>
<h2>Starting XI (Drag Players)</h2>
<div class="pitch" id="pitch"></div>
</section>

<section>
<h2>Contact Us</h2>
<p>📱 WhatsApp: 09115568667</p>
<p>📷 Instagram: Lion_star_Fc</p>
</section>

<footer>
© 2026 Lion Star FC
</footer>

<script>
let maxPlayers=11;
let formation="433";

const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}]
};

// ADMIN CHECK
function isAdmin(){
let pass=document.getElementById("pass").value;
if(pass!=="1234"){alert("Admin only!"); return false;}
return true;
}

// FORMATION
function setFormation(){
formation=document.getElementById("formation").value;
reloadTeam();
}

// ADD PLAYER TO PITCH
function addPlayer(){
if(!isAdmin()) return;
let team=JSON.parse(localStorage.getItem("team"))||[];
if(team.length>=maxPlayers){alert("Only 11 players allowed!"); return;}
let name=document.getElementById("name").value;
let file=document.getElementById("image").files[0];
let reader=new FileReader();
reader.onload=function(e){
team.push({name,img:e.target.result,x:0,y:0});
localStorage.setItem("team",JSON.stringify(team));
reloadTeam();
};
reader.readAsDataURL(file);
}

// LOAD TEAM
function reloadTeam(){
let pitch=document.getElementById("pitch");
pitch.innerHTML="";
let team=JSON.parse(localStorage.getItem("team"))||[];

team.forEach((p,i)=>{
let pos=positions[formation][i]||{top:200,left:50};
let div=document.createElement("div");
div.className="player";
div.style.top=(p.y||pos.top)+"px";
div.style.left=(p.x||pos.left)+"%";
div.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;

// DRAG
let offsetX,offsetY;
div.onmousedown=function(e){
offsetX=e.offsetX; offsetY=e.offsetY;
document.onmousemove=function(e){
div.style.left=(e.pageX-pitch.offsetLeft-offsetX)/pitch.offsetWidth*100+"%";
div.style.top=(e.pageY-pitch.offsetTop-offsetY)+"px";
};
document.onmouseup=function(){document.onmousemove=null; saveTeam();};
};

// DELETE
div.ondblclick=function(){
team.splice(i,1);
localStorage.setItem("team",JSON.stringify(team));
reloadTeam();
};

pitch.appendChild(div);
});
}

// SAVE TEAM
function saveTeam(){
let team=[];
document.querySelectorAll(".player").forEach(el=>{
team.push({
name:el.querySelector("span").textContent,
img:el.querySelector("img").src,
x:parseFloat(el.style.left),
y:parseFloat(el.style.top)
});
});
localStorage.setItem("team",JSON.stringify(team));
}

function clearTeam(){
if(!isAdmin()) return;
localStorage.removeItem("team");
reloadTeam();
}

// ===== EXTRA SECTIONS =====

// SHOW
function showSections(){
if(localStorage.getItem("players")) document.getElementById("playersSection").style.display="block";
if(localStorage.getItem("matches")) document.getElementById("matchesSection").style.display="block";
if(localStorage.getItem("news")) document.getElementById("newsSection").style.display="block";
if(localStorage.getItem("gallery")) document.getElementById("gallerySection").style.display="block";
}

// PLAYERS
function addPlayerInfo(){
if(!isAdmin()) return;
let data=JSON.parse(localStorage.getItem("players"))||[];
data.push(document.getElementById("playerName").value);
localStorage.setItem("players",JSON.stringify(data));
loadPlayers();
}

function loadPlayers(){
let list=document.getElementById("playersList");
list.innerHTML="";
(JSON.parse(localStorage.getItem("players"))||[]).forEach(p=>{
list.innerHTML+=`<p>${p}</p>`;
});
}

// MATCHES
function addMatch(){
if(!isAdmin()) return;
let data=JSON.parse(localStorage.getItem("matches"))||[];
data.push(document.getElementById("matchText").value);
localStorage.setItem("matches",JSON.stringify(data));
loadMatches();
}

function loadMatches(){
let list=document.getElementById("matchesList");
list.innerHTML="";
(JSON.parse(localStorage.getItem("matches"))||[]).forEach(m=>{
list.innerHTML+=`<p>${m}</p>`;
});
}

// NEWS
function addNews(){
if(!isAdmin()) return;
let data=JSON.parse(localStorage.getItem("news"))||[];
data.push(document.getElementById("newsText").value);
localStorage.setItem("news",JSON.stringify(data));
loadNews();
}

function loadNews(){
let list=document.getElementById("newsList");
list.innerHTML="";
(JSON.parse(localStorage.getItem("news"))||[]).forEach(n=>{
list.innerHTML+=`<p>${n}</p>`;
});
}

// GALLERY
function addGallery(){
if(!isAdmin()) return;
let file=document.getElementById("galleryImage").files[0];
let reader=new FileReader();
reader.onload=function(e){
let data=JSON.parse(localStorage.getItem("gallery"))||[];
data.push(e.target.result);
localStorage.setItem("gallery",JSON.stringify(data));
loadGallery();
};
reader.readAsDataURL(file);
}

function loadGallery(){
let list=document.getElementById("galleryList");
list.innerHTML="";
(JSON.parse(localStorage.getItem("gallery"))||[]).forEach(img=>{
list.innerHTML+=`<img src="${img}" style="width:100px;margin:5px;">`;
});
}

// INIT
function loadAll(){
reloadTeam();
loadPlayers();
loadMatches();
loadNews();
loadGallery();
showSections();
}

loadAll();
</script>

</body>
</head>
