lang="en">
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
border-radius:12px;
width:90%;
max-width:900px;
}

/* PITCH */
.pitch{
position:relative;
width:100%;
height:500px;
background:linear-gradient(#0f7a2f,#0b6623);
border:4px solid white;
border-radius:12px;
overflow:hidden;
}

/* PLAYER CARD */
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
font-size:11px;
background:white;
color:black;
border-radius:10px;
margin-top:2px;
}

/* INPUT */
input, select, button{
padding:10px;
margin:5px;
border-radius:6px;
border:none;
}

button{
background:#111;
color:white;
cursor:pointer;
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
<img src="logo.png">
<h1>⚽ Lion Star FC</h1>
</header>

<section>
<h2>Admin Panel</h2>

<input type="password" id="pass" placeholder="Password"><br>

<input type="text" id="name" placeholder="Player name">

<input type="file" id="image">

<br>

<button onclick="addPlayer()">Add Player</button>
<button onclick="clearTeam()">Clear Team</button>

</section>

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

let pitch = document.getElementById("pitch");

// LOAD SAVED
window.onload = function(){
let saved = JSON.parse(localStorage.getItem("team")) || [];
saved.forEach(p => createPlayer(p));
}

function addPlayer(){

let password = document.getElementById("pass").value;

if(password !== "1234"){
alert("Wrong password!");
return;
}

let name = document.getElementById("name").value;
let file = document.getElementById("image").files[0];

if(!file){ alert("Select image"); return; }

let reader = new FileReader();

reader.onload = function(e){

let player = {
name:name,
img:e.target.result,
x:100,
y:100
};

createPlayer(player);

// SAVE
let team = JSON.parse(localStorage.getItem("team")) || [];
team.push(player);
localStorage.setItem("team", JSON.stringify(team));

};

reader.readAsDataURL(file);

}

function createPlayer(data){

let div = document.createElement("div");
div.className = "player";
div.style.left = data.x + "px";
div.style.top = data.y + "px";

div.innerHTML = `
<img src="${data.img}">
<span>${data.name}</span>
`;

makeDraggable(div,data);

div.onclick = function(e){
e.stopPropagation();

let newName = prompt("Edit name:", data.name);
if(newName){
data.name = newName;
div.querySelector("span").textContent = newName;
saveAll();
}
};

pitch.appendChild(div);
}

function makeDraggable(el,data){

let offsetX, offsetY;

el.onmousedown = function(e){
offsetX = e.offsetX;
offsetY = e.offsetY;

document.onmousemove = function(e){
el.style.left = (e.pageX - pitch.offsetLeft - offsetX) + "px";
el.style.top = (e.pageY - pitch.offsetTop - offsetY) + "px";

data.x = parseInt(el.style.left);
data.y = parseInt(el.style.top);
};

document.onmouseup = function(){
document.onmousemove = null;
saveAll();
};
};

}

function saveAll(){

let players = [];
document.querySelectorAll(".player").forEach(el=>{
let name = el.querySelector("span").textContent;
let img = el.querySelector("img").src;

players.push({
name:name,
img:img,
x:parseInt(el.style.left),
y:parseInt(el.style.top)
});
});

localStorage.setItem("team", JSON.stringify(players));
}

function clearTeam(){
localStorage.removeItem("team");
pitch.innerHTML = "";
}

</script>

</body>
</html>
