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
}

#playersSection{ background:#336699; }
#matchesSection{ background:#339966; }
#newsSection{ background:#993333; }
#gallerySection{ background:#999933; }
#adminPanel{ background:#eeeeee; color:black; }
#contactSection{ background:#333366; }

/* ===== REAL PITCH ===== */
.pitch{
    position:relative;
    height:500px;
    background:linear-gradient(
        to bottom,
        #2e8b57 0%, #2e8b57 10%,
        #3cb371 10%, #3cb371 20%,
        #2e8b57 20%, #2e8b57 30%,
        #3cb371 30%, #3cb371 40%,
        #2e8b57 40%, #2e8b57 50%,
        #3cb371 50%, #3cb371 60%,
        #2e8b57 60%, #2e8b57 70%,
        #3cb371 70%, #3cb371 80%,
        #2e8b57 80%, #2e8b57 90%,
        #3cb371 90%, #3cb371 100%
    );
    border:4px solid white;
    border-radius:10px;
    overflow:hidden;
}

.pitch::before{
    content:"";
    position:absolute;
    width:100%;
    height:100%;
    border:2px solid white;
}

.pitch::after{
    content:"";
    position:absolute;
    left:50%;
    top:0;
    width:2px;
    height:100%;
    background:white;
}

.center-circle{
    position:absolute;
    top:50%;
    left:50%;
    width:120px;
    height:120px;
    border:2px solid white;
    border-radius:50%;
    transform:translate(-50%,-50%);
}

.box{
    position:absolute;
    width:150px;
    height:200px;
    border:2px solid white;
}

.top-box{
    top:0;
    left:50%;
    transform:translateX(-50%);
}

.bottom-box{
    bottom:0;
    left:50%;
    transform:translateX(-50%);
}

/* ===== PLAYERS ===== */
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

.adminOnly{ display:none; }

.deleteBtn{
    background:red;
    font-size:12px;
    padding:5px;
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

<div id="loginBox">
<input type="text" id="accessCode" placeholder="Access Code"><br>
<button onclick="login()">Enter</button>
</div>

<section>
<h2>About Team</h2>
<p>Real Lion FC is a passionate and fast-growing football club based in Kano, Nigeria...</p>
</section>

<section id="adminPanel" style="display:none;">
<h2>Club Control</h2>
<button onclick="logout()">Exit</button><br>

<input type="text" id="playerName" placeholder="Player name">
<button class="adminOnly" onclick="addPlayerInfo()">Add Player</button>

<input type="text" id="matchText" placeholder="Match">
<button class="adminOnly" onclick="addMatch()">Add Match</button>

<input type="text" id="newsText" placeholder="News">
<button class="adminOnly" onclick="addNews()">Add News</button>

<input type="file" id="galleryImage">
<button class="adminOnly" onclick="addGallery()">Add Image</button>

<br><br>

<input type="text" id="name" placeholder="Player name">
<input type="file" id="image"><br>

<select id="formation" onchange="setFormation()">
<option value="433">4-3-3</option>
<option value="442">4-4-2</option>
<option value="352">3-5-2</option>
<option value="4231">4-2-3-1</option>
<option value="343">3-4-3</option>
<option value="532">5-3-2</option>
</select>

<button class="adminOnly" onclick="addPlayer()">Add to Pitch</button>
<button class="adminOnly" onclick="clearTeam()">Clear Team</button>
</section>

<section id="playersSection"><h2>Players</h2><div id="playersList"></div></section>
<section id="matchesSection"><h2>Matches</h2><div id="matchesList"></div></section>
<section id="newsSection"><h2>News</h2><div id="newsList"></div></section>
<section id="gallerySection"><h2>Gallery</h2><div id="galleryList"></div></section>

<section>
<h2>Starting XI</h2>
<div class="pitch" id="pitch">
    <div class="center-circle"></div>
    <div class="box top-box"></div>
    <div class="box bottom-box"></div>
</div>
</section>

<section id="contactSection">
<h2>Contact Us</h2>
<p><a href="https://wa.me/2349115568667" target="_blank">09115568667</a></p>
</section>

<footer>© 2026 Real Lion FC</footer>

<script>
// Unlock admin
let tapCount=0;
logo.onclick=()=>{
    tapCount++;
    if(tapCount===3){
        adminBtn.style.display="block";
        alert("Control unlocked!");
        tapCount=0;
    }
    setTimeout(()=>tapCount=0,2000);
};

function toggleLogin(){
    loginBox.style.display = loginBox.style.display==="block"?"none":"block";
}

function login(){
    if(accessCode.value==="Islamic+1"){
        localStorage.setItem("admin","true");
        showAdminPanel();
        loginBox.style.display="none";
    }else alert("Wrong code!");
}

function showAdminPanel(){
    adminPanel.style.display="block";
    document.querySelectorAll(".adminOnly").forEach(e=>e.style.display="inline-block");
}

if(localStorage.getItem("admin")==="true") showAdminPanel();

function logout(){ localStorage.removeItem("admin"); location.reload(); }

// ===== DATA =====
function addPlayerInfo(){
    let p=JSON.parse(localStorage.getItem("players"))||[];
    if(playerName.value){ p.push(playerName.value); localStorage.setItem("players",JSON.stringify(p)); loadPlayers(); playerName.value=""; }
}
function loadPlayers(){
    playersList.innerHTML="";
    (JSON.parse(localStorage.getItem("players"))||[]).forEach((p,i)=>{
        playersList.innerHTML+=`<p>${p} <button class="deleteBtn adminOnly" onclick="deleteItem('players',${i})">X</button></p>`;
    });
}

function addMatch(){
    let m=JSON.parse(localStorage.getItem("matches"))||[];
    if(matchText.value){ m.push(matchText.value); localStorage.setItem("matches",JSON.stringify(m)); loadMatches(); matchText.value=""; }
}
function loadMatches(){
    matchesList.innerHTML="";
    (JSON.parse(localStorage.getItem("matches"))||[]).forEach((m,i)=>{
        matchesList.innerHTML+=`<p>${m} <button class="deleteBtn adminOnly" onclick="deleteItem('matches',${i})">X</button></p>`;
    });
}

function addNews(){
    let n=JSON.parse(localStorage.getItem("news"))||[];
    if(newsText.value){ n.push(newsText.value); localStorage.setItem("news",JSON.stringify(n)); loadNews(); newsText.value=""; }
}
function loadNews(){
    newsList.innerHTML="";
    (JSON.parse(localStorage.getItem("news"))||[]).forEach((n,i)=>{
        newsList.innerHTML+=`<p>${n} <button class="deleteBtn adminOnly" onclick="deleteItem('news',${i})">X</button></p>`;
    });
}

function addGallery(){
    let file=galleryImage.files[0];
    if(!file) return;
    let reader=new FileReader();
    reader.onload=e=>{
        let g=JSON.parse(localStorage.getItem("gallery"))||[];
        g.push(e.target.result);
        localStorage.setItem("gallery",JSON.stringify(g));
        loadGallery();
    };
    reader.readAsDataURL(file);
}
function loadGallery(){
    galleryList.innerHTML="";
    (JSON.parse(localStorage.getItem("gallery"))||[]).forEach((img,i)=>{
        galleryList.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('gallery',${i})">X</button></div>`;
    });
}

function deleteItem(type,i){
    let d=JSON.parse(localStorage.getItem(type))||[];
    d.splice(i,1);
    localStorage.setItem(type,JSON.stringify(d));
    loadAll();
}

// ===== TEAM =====
let formation="433";
const positions={ "433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}] };

function addPlayer(){
    let team=JSON.parse(localStorage.getItem("team"))||[];
    if(team.length>=11)return alert("Max 11");
    let reader=new FileReader();
    reader.onload=e=>{
        team.push({name:name.value,img:e.target.result,x:0,y:0});
        localStorage.setItem("team",JSON.stringify(team));
        reloadTeam();
    };
    reader.readAsDataURL(image.files[0]);
}

function reloadTeam(){
    pitch.innerHTML=`
        <div class="center-circle"></div>
        <div class="box top-box"></div>
        <div class="box bottom-box"></div>
    `;
    let team=JSON.parse(localStorage.getItem("team"))||[];
    team.forEach((p,i)=>{
        let pos=positions[formation][i];
        let d=document.createElement("div");
        d.className="player";
        d.style.top=(p.y||pos.top)+"px";
        d.style.left=(p.x||pos.left)+"%";
        d.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;
        pitch.appendChild(d);
    });
}

function clearTeam(){ localStorage.removeItem("team"); reloadTeam(); }

function loadAll(){
    loadPlayers();
    loadMatches();
    loadNews();
    loadGallery();
    reloadTeam();
}

loadAll();
</script>

</body>
</html>
