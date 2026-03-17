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
discipline, and winning mentality. We develop young talents and play with pride.
</p>
</section>

<section>
<h2>Admin Panel</h2>
<input type="password" id="pass" placeholder="Password"><br>
<input type="text" id="name" placeholder="Player name">
<input type="file" id="image"><br>
<select id="formation" onchange="setFormation()">
<option value="433">4-3-3</option>
<option value="442">4-4-2</option>
<option value="352">3-5-2</option>
</select><br>
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
let maxPlayers=11;
let formation="433";

const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}]
};

function setFormation(){formation=document.getElementById("formation").value;reloadTeam();}

function addPlayer(){
let password=document.getElementById("pass").value;
if(password!=="1234"){alert("Wrong password"); return;}
let team=JSON.parse(localStorage.getItem("team"))||[];
if(team.length>=maxPlayers){alert("Only 11 players allowed!"); return;}
let name=document.getElementById("name").value;
let file=document.getElementById("image").files[0];
if(!file){alert("Select image"); return;}
let reader=new FileReader();
reader.onload=function(e){
let player={name:name,img:e.target.result,x:0,y:0};
team.push(player);
localStorage.setItem("team",JSON.stringify(team));
reloadTeam();
};
reader.readAsDataURL(file);
}

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

// DRAG FUNCTION
let offsetX,offsetY;
div.onmousedown=function(e){
offsetX=e.offsetX; offsetY=e.offsetY;
document.onmousemove=function(e){
div.style.left=(e.pageX-pitch.offsetLeft-offsetX)/pitch.offsetWidth*100+"%";
div.style.top=(e.pageY-pitch.offsetTop-offsetY)+"px";
};
document.onmouseup=function(){document.onmousemove=null; saveTeam();};
};

// REMOVE PLAYER ON CLICK + SHIFT KEY
div.ondblclick=function(){team.splice(i,1); localStorage.setItem("team",JSON.stringify(team)); reloadTeam();};

pitch.appendChild(div);
});
}

function saveTeam(){
let team=[];
document.querySelectorAll(".player").forEach(el=>{
let name=el.querySelector("span").textContent;
let img=el.querySelector("img").src;
team.push({name,img,x:parseFloat(el.style.left),y:parseFloat(el.style.top)});
});
localStorage.setItem("team",JSON.stringify(team));
}

function clearTeam(){localStorage.removeItem("team"); reloadTeam();}

reloadTeam();
</script>
</body>

