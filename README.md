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
#playersSection{ background:#ddeeff; }#matchesSection{ background:#ddffdd; }#newsSection{ background:#ffdddd; }#gallerySection{ background:#ffffdd; }#adminPanel{ background:#eeeeee; display:none; }
.pitch{position:relative;height:500px;background:green;border:4px solid white;border-radius:10px;margin-bottom:20px;}
.player{position:absolute;width:60px;text-align:center;cursor:grab;}
.player img{width:60px;height:60px;border-radius:50%;border:2px solid white;}
.player span{display:block;font-size:12px;background:white;color:black;border-radius:10px;}
button,input,select{padding:10px;margin:5px;border:none;border-radius:5px;cursor:pointer;}
button{background:#111;color:white;}
.adminOnly{ display:none; }
.deleteBtn{ background:red; font-size:12px; padding:5px; cursor:pointer; }
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
<h2
