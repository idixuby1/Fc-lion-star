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
<img src="images - 2026-03-03T054620.224.jpeg" alt="Lion Star FC Logo">
<h1>⚽ Lion Star FC</h1>
<button id="adminBtn" onclick="toggleLogin()">Admin</button>
</header>

<div id="loginBox">
<input type="text" id="user" placeholder="Username"><br>
<input type="password" id="pass" placeholder="Password"><br>
<button onclick="login()">Login</button>
</div>

<!-- Updated About Team Section -->
<section>
<h2>About Team</h2>
<p>FC Lion is a passionate and fast-growing football club based in Kano, Nigeria. The club is dedicated to developing young talents, promoting teamwork, and building a strong football culture in the community. At FC Lion, we believe football is more than a game — it is a way to inspire discipline, unity, and success. Our players are committed, hardworking, and always ready to give their best on and off the pitch as we aim to grow into one of the top football clubs in Nigeria.</p>
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

<footer>© 2026 Lion Star FC</footer>

<script>
// (ALL YOUR JAVASCRIPT — unchanged)
let adminPanel = document.getElementById("adminPanel");
let loginBox = document.getElementById("loginBox");
let pitch = document.getElementById("pitch");

function toggleLogin(){
    loginBox.style.display = loginBox.style.display==="none" ? "block" : "none";
}

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

if(localStorage.getItem("admin")==="true"){
    adminPanel.style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=> el.style.display="inline-block");
}

function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

let formation="433";
const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}],
"4231":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:280,left:35},{top:280,left:55},{top:200,left:25},{top:200,left:45},{top:200,left:65},{top:80,left:45}],
"343":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"532":[{top:450,left:45},{top:350,left:10},{top:350,left:30},{top:350,left:50},{top:350,left:70},{top:350,left:90},{top:250,left:30},{top:250,left:50},{top:250,left
