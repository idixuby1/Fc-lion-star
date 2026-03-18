
lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Lion Star FC</title>

<style>
/* KEEP YOUR CSS HERE (only once) */
</style>
</head>

<body>

<header>
<img src="images - 2026-03-03T054620.224.jpeg">
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
<p>Lion Star FC is a strong football club focused on teamwork.</p>
</section>

<section id="adminPanel" style="display:none;">
<h2>Admin Panel</h2>
<button onclick="logout()">Logout</button>
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
/* KEEP YOUR JAVASCRIPT HERE (only once) */
</script>

</body>
