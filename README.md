<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Football Page</title>
<style>
  body { font-family: Arial; background:#0b6623; color:white; text-align:center; margin:0; padding:20px; }
  h1, h2 { margin-bottom:10px; }
  ul { list-style: none; padding:0; }
  li { padding:5px; }
  input, button { margin:5px; padding:5px; }
  button { cursor:pointer; }
</style>
</head>
<body>
<h1>Football Matches & Players</h1>

<h2>Players</h2>
<ul id="playerList"></ul>
<input type="text" id="playerInput" placeholder="Enter player name">
<button onclick="addPlayer()">Add Player</button>

<h2>Matches</h2>
<ul id="matchList"></ul>
<input type="text" id="matchInput" placeholder="Enter match name">
<button onclick="addMatch()">Add Match</button>

<script>
// --- Players ---
const playerList = document.getElementById("playerList");

function addPlayer() {
    const input = document.getElementById("playerInput");
    const name = input.value.trim();
    if(name === "") return;

    const li = document.createElement("li");
    li.textContent = name;

    const delBtn = document.createElement("button");
    delBtn.textContent = "Remove";
    delBtn.style.marginLeft = "10px";
    delBtn.onclick = () => li.remove();

    li.appendChild(delBtn);
    playerList.appendChild(li);
    input.value = "";
}

// --- Matches ---
const matchList = document.getElementById("matchList");

function addMatch() {
    const input = document.getElementById("matchInput");
    const name = input.value.trim();
    if(name === "") return;

    const li = document.createElement("li");
    li.textContent = name;

    const delBtn = document.createElement("button");
    delBtn.textContent = "Remove";
    delBtn.style.marginLeft = "10px";
    delBtn.onclick = () => li.remove();

    li.appendChild(delBtn);
    matchList.appendChild(li);
    input.value = "";
}
</script>
</body>
</html>
