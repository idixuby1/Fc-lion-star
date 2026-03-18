<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC</title>
<style>
body{ margin:0; font-family:Arial,sans-serif; background:#0b6623; color:white; text-align:center;}
header{ background:#111; padding:20px;}
header img{ width:100px; border-radius:50%; }

/* FLOATING ADMIN BUTTON */
#adminBtn{
    position:fixed; bottom:20px; right:20px;
    background:#444; color:white; padding:10px; border:none; border-radius:50%;
    font-size:16px; cursor:pointer; z-index:1000;
}

/* SECTIONS */
section{ margin:20px auto; padding:20px; border-radius:12px; width:90%; max-width:900px; color:black;}
#playersSection{ background:#ddeeff; }
#matchesSection{ background:#ddffdd; }
#newsSection{ background:#ffdddd; }
#gallerySection{ background:#ffffdd; }

/* PITCH */
.pitch{ position:relative; height:500px; background:green; border:4px solid white; border-radius:10px;}
.player{ position:absolute; width:60px; text-align:center; }
.player img{ width:60px; height:60px; border-radius:50%; border:2px solid white; }
.player span{ display:block; font-size:12px; background:white; color:black; border-radius:10px; }

footer{ background:#111; padding:15px; color:#aaa; }
</style>
</head>
<body>

<header>
<img src="images - 2026-03-03T054620.224.jpeg" alt="Lion Star FC Logo">
<h1>⚽ Lion Star FC</h1>
</header>

<!-- ADMIN BUTTON -->
<button id="adminBtn" onclick="goAdmin()">⚙️</button>

<!-- ABOUT -->
<section>
<h2>About Team</h2>
<p>Lion Star FC is a strong football club focused on teamwork and winning mentality.</p>
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
// GO TO ADMIN PAGE
function goAdmin(){ window.location.href="admin.html"; }

// LOAD DATA
function loadList(type,element){
    let list=document.getElementById(element);
    list.innerHTML="";
    (JSON.parse(localStorage.getItem(type))||[]).forEach(item=>{
        list.innerHTML+=`<p>${item}</p>`;
    });
}
function loadGallery(){
    let list=document.getElementById("galleryList");
    list.innerHTML="";
    (JSON.parse(localStorage.getItem("gallery"))||[]).forEach(img=>{
        list.innerHTML+=`<img src="${img}" width="100" style="margin:5px;">`;
    });
}

// PITCH
let formation="433";
const positions = {
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC Admin</title>
<style>
body{ font-family:Arial,sans-serif; margin:0; background:#0b6623; color:white; text-align:center;}
section{ max-width:400px; margin:20px auto; background:#eee; color:black; border-radius:10px; padding:15px;}
input,select,button{ width:90%; padding:5px; margin:3px; border-radius:5px; border:none;}
button{background:#111; color:white; cursor:pointer;}
</style>
</head>
<body>

<section id="loginSection">
<h2>Admin Login</h2>
<input type="text" id="user" placeholder="Username"><br>
<input type="password" id="pass" placeholder="Password"><br>
<button onclick="login()">Login</button>
</section>

<section id="adminSection" style="display:none;">
<h2>Admin Panel</h2>
<button onclick="logout()">Logout</button><br>

<input type="text" id="playerName" placeholder="Player Name">
<button onclick="addPlayerInfo()">Add Player</button>

<input type="text" id="matchText" placeholder="Match">
<button onclick="addMatch()">Add Match</button>

<input type="text" id="newsText" placeholder="News">
<button onclick="addNews()">Add News</button>

<input type="file" id="galleryImage">
<button onclick="addGallery()">Add Image</button>

<input type="text" id="name" placeholder="Player Name"><br>
<input type="file" id="image"><br>

<select id="formation">
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
</select><br>

<button onclick="addPlayer()">Add to Pitch</button>
<button onclick="clearTeam()">Clear Team</button>
</section>

<script>
// LOGIN
function login(){
    let u=document.getElementById("user").value;
    let p=document.getElementById("pass").value;
    if(u==="admin" && p==="1234"){
        localStorage.setItem("admin","true");
        document.getElementById("loginSection").style.display="none";
        document.getElementById("adminSection").style.display="block";
    } else { alert("Wrong login!"); }
}

// LOGOUT
function logout(){ localStorage.removeItem("admin"); location.reload(); }

// ADD / DELETE / FORMATION FUNCTIONS
let formation="433";
const positions = {
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
// Add other formations similarly...
};

// Functions for addPlayerInfo, addMatch, addNews, addGallery, addPlayer, clearTeam are the same as previous merged code
</script>
</body>
</html>
