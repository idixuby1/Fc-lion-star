<script>
// TAP LOGO 3 TIMES TO UNLOCK
let tapCount = 0;

document.getElementById("logo").addEventListener("click", function(){
    tapCount++;
    if(tapCount === 3){
        document.getElementById("adminBtn").style.display = "block";
        alert("Control unlocked!");
        tapCount = 0;
    }
    setTimeout(()=>{ tapCount = 0; }, 2000);
});

// TOGGLE LOGIN
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

// SHOW ADMIN PANEL
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

// ===== Section Content Management =====

// Add Player to Players Section
function addPlayerInfo(){
    let name = document.getElementById("playerName").value.trim();
    if(!name) return alert("Enter player name");
    let players = JSON.parse(localStorage.getItem("players")) || [];
    players.push(name);
    localStorage.setItem("players", JSON.stringify(players));
    loadList("players", "playersList");
    document.getElementById("playerName").value="";
}

// Add Match
function addMatch(){
    let match = document.getElementById("matchText").value.trim();
    if(!match) return alert("Enter match");
    let matches = JSON.parse(localStorage.getItem("matches")) || [];
    matches.push(match);
    localStorage.setItem("matches", JSON.stringify(matches));
    loadList("matches", "matchesList");
    document.getElementById("matchText").value="";
}

// Add News
function addNews(){
    let news = document.getElementById("newsText").value.trim();
    if(!news) return alert("Enter news");
    let newsList = JSON.parse(localStorage.getItem("news")) || [];
    newsList.push(news);
    localStorage.setItem("news", JSON.stringify(newsList));
    loadList("news", "newsList");
    document.getElementById("newsText").value="";
}

// Add Image to Gallery
function addGallery(){
    let file = document.getElementById("galleryImage").files[0];
    if(!file) return alert("Choose image");
    let reader = new FileReader();
    reader.onload = function(e){
        let gallery = JSON.parse(localStorage.getItem("gallery")) || [];
        gallery.push(e.target.result);
        localStorage.setItem("gallery", JSON.stringify(gallery));
        loadGallery();
    }
    reader.readAsDataURL(file);
}

// Load List (Players, Matches, News)
function loadList(type, elementId){
    let list = document.getElementById(elementId);
    let items = JSON.parse(localStorage.getItem(type)) || [];
    list.innerHTML = "";
    items.forEach((item, i)=>{
        let div = document.createElement("div");
        div.innerHTML = `${item} <button class="deleteBtn adminOnly" onclick="deleteItem('${type}', ${i})">X</button>`;
        list.appendChild(div);
    });
}

// Load Gallery
function loadGallery(){
    let gallery = JSON.parse(localStorage.getItem("gallery")) || [];
    let list = document.getElementById("galleryList");
    list.innerHTML="";
    gallery.forEach((img, i)=>{
        let div = document.createElement("div");
        div.innerHTML = `<img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('gallery', ${i})">X</button>`;
        list.appendChild(div);
    });
}

// Delete Item from section
function deleteItem(type, index){
    let arr = JSON.parse(localStorage.getItem(type)) || [];
    arr.splice(index, 1);
    localStorage.setItem(type, JSON.stringify(arr));
    if(type==="gallery") loadGallery();
    else if(type==="players") loadList("players", "playersList");
    else if(type==="matches") loadList("matches", "matchesList");
    else if(type==="news") loadList("news", "newsList");
}

// Load all sections on page load
function loadAll(){
    loadList("players", "playersList");
    loadList("matches", "matchesList");
    loadList("news", "newsList");
    loadGallery();
}

loadAll();
</script>
