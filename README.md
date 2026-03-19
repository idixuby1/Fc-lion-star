
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

#playersSection{ background:#ddeeff; }
#matchesSection{ background:#ddffdd; }
#newsSection{ background:#ffdddd; }
#gallerySection{ background:#ffffdd; }
#adminPanel{ background:#eeeeee; }
#contactSection{ background:#cce5ff; color:black; }

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

button,input,select,textarea{
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
<img src="images - 2026-03-03T054620.224.jpeg" alt="Real Lion FC Logo">
<h1>⚽ Real Lion FC</h1>
<button id="adminBtn" onclick="toggleLogin()">Admin</button>
</header>

<div id="loginBox">
<input type="text" id="user" placeholder="Username"><br>
<input type="password" id="pass" placeholder="Password"><br>
<button onclick="login()">Login</button>
</div>

<section>
<h2>About Team</h2>
<p>Real Lion FC is a passionate and fast-growing football club based in Kano, Nigeria. The club is dedicated to developing young talents, promoting teamwork, and building a strong football culture in the community. At Real Lion FC, we believe football is more than a game — it is a way to inspire discipline, unity, and success. Our players are committed, hardworking, and always ready to give their best on and off the pitch as we aim to grow into one of the top football clubs in Nigeria.</p>
</section>

<section id="adminPanel" style="display:none;">
<h2>Admin Panel</h2>
<button onclick="logout()">Logout</button><br>
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

<section id="playersSection">
<h2>Players</h2>
<div id="playersList"></div>
</section>

<section id="matchesSection">
<h2>Matches</h2>
<div id="matchesList"></div>
</section>

<section id="newsSection">
<h2>News</h2>
<div id="newsList"></div>
</section>

<section id="gallerySection">
<h2>Gallery</h2>
<div id="galleryList"></div>
</section>

<section>
<h2>Starting XI</h2>
<div class="pitch" id="pitch"></div>
</section>

<!-- Contact Us Section -->
<section id="contactSection">
  <h2>Contact Us</h2>
  <p>You can reach us directly on WhatsApp:</p>
  <p><a href="https://wa.me/2349115568667" target="_blank" style="color:green; font-weight:bold; font-size:18px;">09115568667</a></p>
</section>

<footer>© 2026 Real Lion FC</footer>

<script>
// ===== Admin Toggle and Login =====
let adminPanel = document.getElementById("adminPanel");
let loginBox = document.getElementById("loginBox");
let pitch = document.getElementById("pitch");

function toggleLogin(){
    let loginBoxEl = document.getElementById("loginBox");
    if(!loginBoxEl) return;
    loginBoxEl.style.display = loginBoxEl.style.display === "block" ? "none" : "block";
}

function login(){
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;
    if(u==="admin" && p==="1234"){
        localStorage.setItem("admin","true");
        showAdminPanel();
        loginBox.style.display = "none";
    } else {
        alert("Wrong login!");
    }
}

function showAdminPanel(){
    adminPanel.style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
}

// Auto show admin if already logged in
if(localStorage.getItem("admin")==="true"){
    showAdminPanel();
}

function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// ===== Team, Players, Matches, News, Gallery =====
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
    };
    if(file) reader.readAsDataURL(file);
}

function reloadTeam(){
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

function deleteItem(type,index){
    let data=JSON.parse(localStorage.getItem(type))||[];
    data.splice(index,1);
    localStorage.setItem(type,JSON.stringify(data));
    loadAll();
}

function loadList(type,element){
    let list=document.getElementById(element);
    list.innerHTML="";
    (JSON.parse(localStorage.getItem(type))||[]).forEach((item,i)=>{
        list.innerHTML+=`<p>${item} <button class="deleteBtn adminOnly" onclick="deleteItem('${type}',${i})">X</button></p>`;
    });
}

function loadGallery(){
    let list=document.getElementById("galleryList");
    list.innerHTML="";
    (JSON.parse(localStorage.getItem("gallery"))||[]).forEach((img,i)=>{
        list.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('gallery',${i})">X</button></div>`;
    });
}

function addPlayerInfo(){
    let data = JSON.parse(localStorage.getItem("players"))||[];
    data.push(document.getElementById("playerName").value);
    localStorage.setItem("players",JSON.stringify(data));
    loadList("players","playersList");
}

function addMatch(){
    let data = JSON.parse(localStorage.getItem("matches"))||[];
    data.push(document.getElementById("matchText").value);
    localStorage.setItem("matches",JSON.stringify(data));
    loadList("matches","matchesList");
}

function addNews(){
    let data = JSON.parse(localStorage.getItem("news"))||[];
    data.push(document.getElementById("newsText").value);
    localStorage.setItem("news",JSON.stringify(data));
    loadList("news","newsList");
}

function addGallery(){
    let file=document.getElementById("galleryImage").files[0];
    let reader=new FileReader();
    reader.onload=function(e){
        let data=JSON.parse(localStorage.getItem("gallery"))||[];
        data.push(e.target.result);
        localStorage.setItem("gallery",JSON.stringify(data));
        loadGallery();
    };
    if(file) reader.readAsDataURL(file);
}

// ===== Load All =====
function loadAll(){
    reloadTeam();
    loadList("players","playersList");
    loadList("matches","matchesList");
    loadList("news","newsList");
    loadGallery();
}
loadAll();
</script>
</body>
</html>
