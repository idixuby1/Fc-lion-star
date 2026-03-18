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

/* ADMIN BUTTON AT BOTTOM */
#adminBtn{
    position:fixed; bottom:10px; left:50%; transform:translateX(-50%);
    background:#444; color:white; padding:10px 20px; border:none; border-radius:10px;
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

<!-- ADMIN BUTTON -->
<button id="adminBtn" onclick="goAdmin()">Admin Panel</button>

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
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}]
};

function reloadTeam(){
    let pitch=document.getElementById("pitch");
    pitch.innerHTML="";
    let team=JSON.parse(localStorage.getItem("team"))||[];
    team.forEach((p,i)=>{
        let pos=positions[formation][i];
        let div=document.createElement("div");
        div.className="player";
        div.style.top=pos.top+"px";
        div.style.left=pos.left+"%";
        div.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;
        pitch.appendChild(div);
    });
}

function loadAll(){
    loadList("players","playersList");
    loadList("matches","matchesList");
    loadList("news","newsList");
    loadGallery();
    reloadTeam();
}

loadAll();
</script>

</body>
</html>
