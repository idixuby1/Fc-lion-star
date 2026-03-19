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
<img src="images - 2026-03-03T054620.224.jpeg">
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
// SECRET KEY TO SHOW CONTROL BUTTON
document.addEventListener("keydown", function(e){
    if(e.key === "A"){
        document.getElementById("adminBtn").style.display = "block";
    }
});

// TOGGLE
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

// KEEP YOUR OTHER FUNCTIONS SAME...
</script>

</body>
</html>
