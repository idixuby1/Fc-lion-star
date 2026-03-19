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
    color:white; /* white text for dark theme */
}

#playersSection{ background:#336699; }
#matchesSection{ background:#339966; }
#newsSection{ background:#993333; }
#gallerySection{ background:#999933; }
#adminPanel{ background:#eeeeee; color:black; }
#contactSection{ background:#333366; }

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

<!-- ACCESS BOX -->
<div id="loginBox">
<input type="text" id="accessCode" placeholder="Access Code"><br>
<button onclick="login()">Enter</button>
</div>

<section>
<h2>About Team</h2>
<p>Real Lion FC is a passionate and fast-growing football club based in Kano, Nigeria...</p>
</section>

<!-- CONTROL PANEL -->
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
<div class="pitch" id="pitch"></div>
</section>

<section id="contactSection">
<h2>Contact Us</h2>
<p><a href="https://wa.me/2349115568667" target="_blank">09115568667</a></p>
</section>

<footer>© 2026 Real Lion FC</footer>

<script>
// TAP LOGO 3 TIMES TO UNLOCK
let tapCount = 0;
document.getElementById("logo").addEventListener("click", function(){
    tapCount++;
    if(tapCount === 3){
        document.getElementById("adminBtn").style.display = "block";
        alert("Control unlocked!");
        tapCount = 0;
    }
    setTimeout(()=>{ tapCount = 0; }, 2000);
});

// TOGGLE LOGIN
function toggleLogin(){
    let box = document.getElementById("loginBox");
    box.style.display = box.style.display === "block" ? "none" : "block";
}

// LOGIN
function login(){
    let code = document.getElementById("accessCode").value;
    if(code === "lion123"){
        localStorage.setItem("admin","true");
        showAdminPanel();
        document.getElementById("loginBox").style.display = "none";
    } else {
        alert("Wrong code!");
    }
}

function showAdminPanel(){
    document.getElementById("adminPanel").style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
}

if(localStorage.getItem("admin")==="true"){
    showAdminPanel();
}

function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// ===== DATA MANAGEMENT =====
function addPlayerInfo(){
    let players = JSON.parse(localStorage.getItem("players"))||[];
    let name = document.getElementById("playerName").value;
    if(name){
        players.push(name);
        localStorage.setItem("players", JSON.stringify(players));
        loadPlayers();
        document.getElementById("playerName").value="";
    }
}

function loadPlayers(){
    let list = document.getElementById("playersList");
    let players = JSON.parse(localStorage.getItem("players"))||[];
    list.innerHTML="";
    players.forEach((p,i)=>{
        list.innerHTML+=`<p>${p} <button class="deleteBtn adminOnly" onclick="deleteItem('players',${i})">X</button></p>`;
    });
}

function addMatch(){
    let matches = JSON.parse(localStorage.getItem("matches"))||[];
    let text = document.getElementById("matchText").value;
    if(text){
        matches.push(text);
        localStorage.setItem("matches", JSON.stringify(matches));
        loadMatches();
        document.getElementById("matchText").value="";
    }
}

function loadMatches(){
    let list = document.getElementById("matchesList");
    let matches = JSON.parse(localStorage.getItem("matches"))||[];
    list.innerHTML="";
    matches.forEach((m,i)=>{
        list.innerHTML+=`<p>${m} <button class="deleteBtn adminOnly" onclick="deleteItem('matches',${i})">X</button></p>`;
    });
}

function addNews(){
    let news = JSON.parse(localStorage.getItem("news"))||[];
    let text = document.getElementById("newsText").value;
    if(text){
        news.push(text);
        localStorage.setItem("news", JSON.stringify(news));
        loadNews();
        document.getElementById("newsText").value="";
    }
}

function loadNews(){
    let list = document.getElementById("newsList");
    let news = JSON.parse(localStorage.getItem("news"))||[];
    list.innerHTML="";
    news.forEach((n,i)=>{
        list.innerHTML+=`<p>${n} <button class="deleteBtn adminOnly" onclick="deleteItem('news',${i})">X</button></p>`;
    });
}

function addGallery(){
    let file = document.getElementById("galleryImage").files[0];
    if(!file) return;
    let reader = new FileReader();
    reader.onload = function(e){
        let gallery = JSON.parse(localStorage.getItem("gallery"))||[];
        gallery.push(e.target.result);
        localStorage.setItem("gallery", JSON.stringify(gallery));
        loadGallery();
    }
    reader.readAsDataURL(file);
}

function loadGallery(){
    let list = document.getElementById("galleryList");
    let gallery = JSON.parse(localStorage.getItem("gallery"))||[];
    list.innerHTML="";
    gallery.forEach((img,i)=>{
        list.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('gallery',${i})">X</button></div>`;
    });
}

function deleteItem(type,index){
    let data = JSON.parse(localStorage.getItem(type))||[];
    data.splice(index,1);
    localStorage.setItem(type, JSON.stringify(data));
    // reload section
    if(type==='players') loadPlayers();
    else if(type==='matches') loadMatches();
    else if(type==='news') loadNews();
    else if(type==='gallery') loadGallery();
}

// ===== PITCH PLAYERS =====
let formation="433";
const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}],
"4231":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:280,left:35},{top:280,left:55},{top:200,left:25},{top:200,left:45},{top:200,left:65},{top:80,left:45}],
"343":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"532":[{top:450,left:45},{top:350,left:10},{top:350,left:30},{top:350,left:50},{top:350,left:70},{top:350,left:90},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:35},{top:100,left:55}]
};

function setFormation(){ formation=document.getElementById("formation").value; reloadTeam(); }

function addPlayer(){
    let team = JSON.parse(localStorage.getItem("team"))||[];
    if(team.length>=11){ alert("Max 11"); return; }
    let file = document.getElementById("image").files[0];
    let reader = new FileReader();
    reader.onload = function(e){
        team.push({name:document.getElementById("name").value, img:e.target.result, x:0, y:0});
        localStorage.setItem("team",JSON.stringify(team));
        reloadTeam();
        document.getElementById("name").value="";
    };
    if(file) reader.readAsDataURL(file);
}

function reloadTeam(){
    let pitch = document.getElementById("pitch");
    pitch.innerHTML="";
    let team = JSON.parse(localStorage.getItem("team"))||[];
    team.forEach((p,i)=>{
        let pos = positions[formation][i];
        let div = document.createElement("div");
        div.className="player";
        div.style.top=(p.y||pos.top)+"px";
        div.style.left=(p.x||pos.left)+"%";
        div.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;
        div.onmousedown = function(e){
            let ox=e.offsetX, oy=e.offsetY;
            function moveHandler(e){
                div.style.left=(e.pageX-pitch.offsetLeft-ox)/pitch.offsetWidth*100+"%";
                div.style.top=(e.pageY-pitch.offsetTop-oy)+"px";
            }
            document.addEventListener("mousemove",moveHandler);
            document.addEventListener("mouseup",()=>{ document.removeEventListener("mousemove",moveHandler); saveTeam(); }, {once:true});
        };
        div.ondblclick=function(){
            team.splice(i,1);
            localStorage.setItem("team",JSON.stringify(team));
            reloadTeam();
        };
        pitch.appendChild(div);
    });
}

function saveTeam(){
    let team=[];
    document.querySelectorAll(".player").forEach(el=>{
        team.push({
            name: el.querySelector("span").textContent,
            img: el.querySelector("img").src,
            x: parseFloat(el.style.left),
            y: parseFloat(el.style.top)
        });
    });
    localStorage.setItem("team",JSON.stringify(team));
}

function clearTeam(){
    localStorage.removeItem("team");
    reloadTeam();
}

// LOAD ALL
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
