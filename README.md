<section id="adminPanel">
<h2>Admin Panel</h2>
<button onclick="logout()">Logout</button><br>

<!-- Player for Pitch -->
<input type="text" id="name" placeholder="Player Name">
<select id="position">
<option value="GK">Goalkeeper</option>
<option value="DEF">Defender</option>
<option value="MID">Midfielder</option>
<option value="FWD">Forward</option>
</select>
<input type="file" id="image"><br>
<button class="adminOnly" onclick="addPlayer()">Add Player</button>

<h3>Players List</h3>
<input type="text" id="playerName" placeholder="Player Name">
<button class="adminOnly" onclick="addPlayerInfo()">Add to List</button>
<div id="playersList"></div>

<h3>Matches</h3>
<input type="text" id="matchText" placeholder="Match Info">
<button class="adminOnly" onclick="addMatch()">Add Match</button>
<div id="matchesList"></div>

<h3>News</h3>
<input type="text" id="newsText" placeholder="News">
<button class="adminOnly" onclick="addNews()">Add News</button>
<div id="newsList"></div>

<h3>Gallery</h3>
<input type="file" id="galleryImage">
<button class="adminOnly" onclick="addGallery()">Upload Image</button>
<div id="galleryList"></div>

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
<button class="adminOnly" onclick="clearTeam()">Clear Team</button>
</section>
