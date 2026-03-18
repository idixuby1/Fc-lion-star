<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#0b6623;color:white;text-align:center;}
header{background:#111;padding:20px;position:relative;}
header img{width:100px;border-radius:50%;}
#adminBtn{position:absolute;top:20px;right:20px;background:#444;color:white;padding:8px 12px;border:none;border-radius:5px;cursor:pointer;}
#loginBox{display:none;background:#111;padding:15px;border-radius:10px;width:200px;margin:10px auto;}
section{margin:20px auto;padding:20px;border-radius:12px;width:90%;max-width:900px;color:black;}
#playersSection{background:#ddeeff;}
#matchesSection{background:#ddffdd;}
#newsSection{background:#ffdddd;}
#gallerySection{background:#ffffdd;}
#adminPanel{background:#eeeeee;}
.pitch{position:relative;height:500px;background:green;border:4px solid white;border-radius:10px;}
.player{position:absolute;width:60px;text-align:center;cursor:grab;}
.player img{width:60px;height:60px;border-radius:50%;border:2px solid white;}
.player span{display:block;font-size:12px;background:white;color:black;border-radius:10px;}
button,input,select{padding:10px;margin:5px;border:none;border-radius:5px;}
button{background:#111;color:white;cursor:pointer;}
.adminOnly{display:none;}
.deleteBtn{background:red;font-size:12px;padding:3px 5px;cursor:pointer;margin-left:5px;border-radius:4px;}
footer{background:#111;padding:15px;color:#aaa;}
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

<section>
<h2>About Team</h2>
<p>Lion Star FC is a strong football club focused on teamwork and winning mentality.</p>
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
<option value="352">3-5-2</
