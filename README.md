<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC</title>

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

/* FLOATING ADMIN BUTTON */
#adminBtn{
    position:fixed;
    bottom:20px;
    right:20px;
    background:#444;
    color:white;
    padding:10px 15px;
    border:none;
    border-radius:50%;
    font-size:16px;
    cursor:pointer;
    z-index:1000;
}

/* LOGIN BOX */
#loginBox{
    display:none;
    position:fixed;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
    background:#111;
    padding:20px;
    border-radius:12px;
    width:220px;
    z-index:1000;
}

section{
    margin:20px auto;
    padding:20px;
    border-radius:12px;
    width:90%;
    max-width:900px;
    color:black;
}

/* Section Colors */
#playersSection{ background:#ddeeff; }
#matchesSection{ background:#ddffdd; }
#newsSection{ background:#ffdddd; }
#gallerySection{ background:#ffffdd; }
#adminPanel{ background:#eeeeee; position:fixed; top:10%; right:10%; width:350px; max-width:90%; padding:20px; border-radius:12px; display:none; z-index:999; box-shadow:0 0 10px black; }

/* PITCH */
.pitch{
    position:relative;
    height:500px;
    background:green;
    border:4px solid white;
    border-radius:10px;
}

/* PLAYER */
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

/* FORM ELEMENTS */
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
<img src="images - 2026-03-03T054620.224.jpeg" alt="Lion Star FC Logo">
<h1>⚽ Lion Star FC</h1>
</header>

<!-- FLOATING ADMIN BUTTON -->
<button id="adminBtn" onclick="toggleLogin()">⚙️</button>

<!-- LOGIN -->
<div id="loginBox">
<h3>Admin Login</h3>
<input type="text" id="user" placeholder="Username"><br>
<input type="password" id="pass" placeholder="Password"><br>
<button onclick="login()">Login</button>
<button onclick="loginBox.style.display='none'">Cancel</button>
</div>

<!-- ABOUT -->
<section>
<h2>About Team</h2>
<p>Lion Star FC is a strong football club focused on teamwork and winning mentality.</p>
</section>

<!-- ADMIN PANEL -->
<section id="adminPanel">
<h2>Admin Panel</h2>
<button onclick="logout()">Logout</button><br><br>

<h3>Formation</h3>
<select id="formation" onchange="setFormation()">
    <option value="433">4-3-3</option>
    <option value="442">4-4-2</option>
    <option value="352">3-5-2</option>
    <option value="4231">4-2-3-1</option>
    <option value="343">3-4-3</option>
    <option value="532">5-3-2</option>
    <option value="451">4-5-1</option>
    <option value="541">5-4-1</option>
    <option value="4141">4-1-4-1</option>
    <option value="343d">3-4-3 Diamond</option>
</select>

<hr>

<h3>Add Player</h3>
<input type="text" id="name" placeholder="Player name">
<input type="file" id="image"><br>
<button class="adminOnly" onclick="addPlayer()">Add to Pitch</button>
<button class="adminOnly" onclick="clearTeam()">Clear Team</button>

<hr>

<h3>Players</h3>
<input type="text" id="playerName" placeholder="Player name">
<button class="adminOnly" onclick="addPlayerInfo()">Add Player Info</button>
<div id="playersList"></div>

<hr>

<h3>Matches</h3>
<input type="text" id="matchText" placeholder="Match">
<button class="adminOnly" onclick="addMatch()">Add Match</button>
<div id="matchesList"></div>

<hr>

<h3>News</h3>
<input type="text" id="newsText" placeholder="News">
<button class="adminOnly" onclick="addNews()">Add News</button>
<div id="newsList"></div>

<hr>

<h3>Gallery</h3>
<input type="file" id="galleryImage">
<button class="adminOnly" onclick="addGallery()">Add Image</button>
<div id="galleryList"></div>

</section>

<!-- PITCH -->
<section>
<h2>Starting XI</h2>
<div class="pitch" id="pitch"></div>
</section>

<footer>© 2026 Lion Star FC</footer>

<script>
// VARIABLES
let adminPanel = document.getElementById("adminPanel");
let loginBox = document.getElementById("loginBox");
let pitch = document.getElementById("pitch");

// TOGGLE LOGIN
function toggleLogin(){
    loginBox.style.display = loginBox.style.display==="none" ? "block" : "none";
}

// LOGIN
function login(){
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;

    if(u==="admin" && p==="1234"){
        localStorage.setItem("admin","true");
        adminPanel.style.display="block";
        document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
        loginBox.style.display="none";
    }else{
        alert("Wrong login!");
    }
}

// AUTO LOGIN
if(localStorage.getItem("admin")==="true"){
    adminPanel.style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
}

// LOGOUT
function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// FORMATIONS
let formation="433";
const positions = {
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}],
"4231":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:280,left:35},{top:280,left:55},{top:200,left:25},{top:200,left:45},{top:200,left:65},{top:80,left:45}],
"343":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"532":[{top:450,left:45},{top:350,left:10},{top:350,left:30},{top:350,left:50},{top:350,left:70},{top:350,left:90},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:35},{top:100,left:55}],
"451":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:250,left:45},{top:100,left:45}],
"541":[{top:450,left:45},{top:350,left:10},{top:350,left:30},{top:350,left:50},{top:350,left:70},{top:350,left:90},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:45}],
"4141":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:250,left:85},{top:100,left:35},{top:100,left:55}],
"343d":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:35},{top:250,left:45},{top:250,left:55},{top:100,left:25},{top:80,left:45},{top:100,left:65},{top:200,left:45}]
};

// FORMATION SWITCH
function setFormation(){
    formation=document.getElementById("formation").value;
    reloadTeam();
}

// ADD PLAYER TO PITCH
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

// RELOAD PITCH
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

// SAVE PITCH
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

// CLEAR PITCH
function clearTeam(){
    localStorage.removeItem("team");
    reloadTeam();
}

// DELETE ITEM
function deleteItem(type,index){
    let data=JSON.parse(localStorage.getItem(type))||[];
    data.splice(index,1);
    localStorage.setItem(type,JSON.stringify(data));
    loadAll();
}

// LOAD LIST
function loadList(type,element){
    let list=document.getElementById(element);
    list.innerHTML="";
    (JSON.parse(localStorage.getItem(type))||[]).forEach((item,i)=>{
        list.innerHTML+=`<p>${item} <button class="deleteBtn adminOnly" onclick="deleteItem('${type}',${i})">X</button></p>`;
    });
}

// LOAD GALLERY
function loadGallery(){
    let list=document.getElementById("galleryList");
    list.innerHTML="";
    (JSON.parse(local
