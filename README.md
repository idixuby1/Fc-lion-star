<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Real Lion FC</title>
<style>
body{
    margin:0;
    font-family: Arial, sans-serif;
    background:#0b6623;
    color:white;
    text-align:center;
}
header{
    background:#111;
    padding:20px;
    position:relative;
}
header img{
    width:100px;
    border-radius:50%;
    cursor:pointer;
}
#adminBtn{
    position:absolute;
    top:20px;
    right:20px;
    background:#444;
    color:white;
    padding:8px 12px;
    border:none;
    border-radius:5px;
    cursor:pointer;
    display:none;
}
#loginBox{
    display:none;
    background:#111;
    padding:15px;
    border-radius:10px;
    width:200px;
    margin:10px auto;
}
section{
    margin:20px auto;
    padding:20px;
    border-radius:12px;
    width:90%;
    max-width:900px;
    color:black;
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
}
button,input,select{
    padding:10px;
    margin:5px;
    border:none;
    border-radius:5px;
}
button{
    background:#111;
    color:white;
    cursor:pointer;
}
.adminOnly{
    display:none;
}
.deleteBtn{
    background:red;
    font-size:12px;
    padding:5px;
    cursor:pointer;
}
footer{
    background:#111;
    padding:15px;
    color:#aaa;
}
</style>
</head>
<body>

<header>
<img src="images - 2026-03-03T054620.224.jpeg" id="logo">
<h1>⚽ Real Lion FC</h1>
<button id="adminBtn" onclick="toggleLogin()">Control</button>
</header>

<!-- LOGIN -->
<div id="loginBox">
<input type="text" id="accessCode" placeholder="Access Code"><br>
<button onclick="login()">Enter</button>
</div>

<section>
<h2>About Team</h2>
<p>Real Lion FC is a passionate and fast-growing football club based in Kano, Nigeria...</p>
</section>

<!-- ADMIN PANEL -->
<section id="adminPanel" style="display:none;">
<h2>Club Control</h2>
<button onclick="logout()">Exit</button><br>

<h3>Players</h3>
<input type="text" id="name" placeholder="Player name">
<input type="file" id="image">
<button class="adminOnly" onclick="addPlayer()">Add Player</button>
<button class="adminOnly" onclick="clearTeam()">Clear All Players</button>
<div id="playersList"></div>

<h3>Matches</h3>
<input type="text" id="matchText" placeholder="Match">
<button class="adminOnly" onclick="addMatch()">Add Match</button>
<div id="matchesList"></div>

<h3>News</h3>
<input type="text" id="newsText" placeholder="News">
<button class="adminOnly" onclick="addNews()">Add News</button>
<div id="newsList"></div>

<h3>Gallery</h3>
<input type="file" id="galleryImage">
<button class="adminOnly" onclick="addGallery()">Add Image</button>
<div id="galleryList"></div>
</section>

<section>
<h2>Starting XI</h2>
<div class="pitch" id="pitch"></div>
</section>

<section id="contactSection">
<h2>Contact Us</h2>
<p><a href="https://wa.me/2349115568667" target="_blank">09115568667</a></p>
</section>

<footer>© 2026 Real Lion FC</footer>

<script>
// ==== ADMIN UNLOCK ====
let tapCount=0;
document.getElementById("logo").addEventListener("click", ()=>{
    tapCount++;
    if(tapCount===3){
        document.getElementById("adminBtn").style.display="block";
        alert("Control unlocked!");
        tapCount=0;
    }
    setTimeout(()=>{tapCount=0;},2000);
});

function toggleLogin(){
    let box = document.getElementById("loginBox");
    box.style.display = box.style.display==="block"?"none":"block";
}

function login(){
    let code = document.getElementById("accessCode").value;
    if(code==="lion123"){
        localStorage.setItem("admin","true");
        showAdminPanel();
        document.getElementById("loginBox").style.display="none";
    } else alert("Wrong code!");
}

function showAdminPanel(){
    document.getElementById("adminPanel").style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=>el.style.display="inline-block");
    loadPlayers();
    loadMatches();
    loadNews();
    loadGallery();
}

if(localStorage.getItem("admin")==="true") showAdminPanel();
function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// ==== PLAYERS ====
let formation="433";
const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}]
};

function addPlayer(){
    let team=JSON.parse(localStorage.getItem("team"))||[];
    if(team.length>=11){alert("Max 11 players");return;}
    let file=document.getElementById("image").files[0];
    if(!file){alert("Select image");return;}
    let reader=new FileReader();
    reader.onload=function(e){
        team.push({name:document.getElementById("name").value,img:e.target.result,x:0,y:0});
        localStorage.setItem("team",JSON.stringify(team));
        document.getElementById("name").value="";
        document.getElementById("image").value="";
        reloadTeam();
        loadPlayers();
    }
    reader.readAsDataURL(file);
}

function reloadTeam(){
    let pitch=document.getElementById("pitch");
    pitch.innerHTML="";
    let team=JSON.parse(localStorage.getItem("team"))||[];
    team.forEach((p,i)=>{
        let pos=positions[formation][i]||{top:400,left:i*7};
        let div=document.createElement("div");
        div.className="player";
        div.style.top=(p.y||pos.top)+"px";
        div.style.left=(p.x||pos.left)+"%";
        div.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;
        div.ondblclick=()=>{deletePlayer(i);}
        pitch.appendChild(div);
    });
}

function deletePlayer(index){
    let team=JSON.parse(localStorage.getItem("team"))||[];
    team.splice(index,1);
    localStorage.setItem("team",JSON.stringify(team));
    reloadTeam();
    loadPlayers();
}

function clearTeam(){
    localStorage.removeItem("team");
    reloadTeam();
    loadPlayers();
}

function loadPlayers(){
    let list=document.getElementById("playersList");
    list.innerHTML="";
    let team=JSON.parse(localStorage.getItem("team"))||[];
    team.forEach((p,i)=>{
        list.innerHTML+=`<p>${p.name} <button class="deleteBtn adminOnly" onclick="deletePlayer(${i})">X</button></p>`;
    });
    reloadTeam();
}

// ==== MATCHES ====
function addMatch(){
    let data=JSON.parse(localStorage.getItem("matches"))||[];
    let text=document.getElementById("matchText").value.trim();
    if(!text) return;
    data.push(text);
    localStorage.setItem("matches",JSON.stringify(data));
    document.getElementById("matchText").value="";
    loadMatches();
}

function loadMatches(){
    let list=document.getElementById("matchesList");
    list.innerHTML="";
    let data=JSON.parse(localStorage.getItem("matches"))||[];
    data.forEach((m,i)=>{
        list.innerHTML+=`<p>${m} <button class="deleteBtn adminOnly" onclick="deleteMatch(${i})">X</button></p>`;
    });
}

function deleteMatch(i){
    let data=JSON.parse(localStorage.getItem("matches"))||[];
    data.splice(i,1);
    localStorage.setItem("matches",JSON.stringify(data));
    loadMatches();
}

// ==== NEWS ====
function addNews(){
    let data=JSON.parse(localStorage.getItem("news"))||[];
    let text=document.getElementById("newsText").value.trim();
    if(!text) return;
    data.push(text);
    localStorage.setItem("news",JSON.stringify(data));
    document.getElementById("newsText").value="";
    loadNews();
}

function loadNews(){
    let list=document.getElementById("newsList");
    list.innerHTML="";
    let data=JSON.parse(localStorage.getItem("news"))||[];
    data.forEach((n,i)=>{
        list.innerHTML+=`<p>${n} <button class="deleteBtn adminOnly" onclick="deleteNews(${i})">X</button></p>`;
    });
}

function deleteNews(i){
    let data=JSON.parse(localStorage.getItem("news"))||[];
    data.splice(i,1);
    localStorage.setItem("news",JSON.stringify(data));
    loadNews();
}

// ==== GALLERY ====
function addGallery(){
    let file=document.getElementById("galleryImage").files[0];
    if(!file) return;
    let reader=new FileReader();
    reader.onload=function(e){
        let data=JSON.parse(localStorage.getItem("gallery"))||[];
        data.push(e.target.result);
        localStorage.setItem("gallery",JSON.stringify(data));
        loadGallery();
        document.getElementById("galleryImage").value="";
    }
    reader.readAsDataURL(file);
}

function loadGallery(){
    let list=document.getElementById("galleryList");
    list.innerHTML="";
    let data=JSON.parse(localStorage.getItem("gallery"))||[];
    data.forEach((img,i)=>{
        list.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteGallery(${i})">X</button></div>`;
    });
}

function deleteGallery(i){
    let data=JSON.parse(localStorage.getItem("gallery"))||[];
    data.splice(i,1);
    localStorage.setItem("gallery",JSON.stringify(data));
    loadGallery();
}

// ==== INIT ====
reloadTeam();
loadPlayers();
loadMatches();
loadNews();
loadGallery();
</script>

</body>
</html>
