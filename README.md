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
    cursor:pointer;
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
<img src="images - 2026-03-03T054620.224.jpeg" id="logo">
<h1>⚽ Real Lion FC</h1>
<button id="adminBtn" onclick="toggleLogin()">Control</button>
</header>

<!-- LOGIN -->
<div id="loginBox">
<input type="text" id="accessCode" placeholder="Access Code"><br>
<button onclick="login()">Enter</button>
</div>

<!-- CONTROL PANEL -->
<section id="adminPanel" style="display:none;">
<h2>Club Control</h2>
<button onclick="logout()">Exit</button><br>

<!-- Add Section -->
<input type="text" id="newSectionName" placeholder="New Section Name">
<select id="newSectionType">
<option value="list">List</option>
<option value="gallery">Gallery</option>
</select>
<button class="adminOnly" onclick="addNewSection()">Add Section</button>
<br><br>

<!-- Add Item to Section -->
<select id="selectSection"></select><br>
<input type="text" id="newItemText" placeholder="New Item (for lists)">
<input type="file" id="newItemImage">
<button class="adminOnly" onclick="addItem()">Add Item</button>
</section>

<section id="pitchSection">
<h2>Starting XI</h2>
<div class="pitch" id="pitch"></div>
</section>

<section id="contactSection">
<h2>Contact Us</h2>
<p><a href="https://wa.me/2349115568667" target="_blank">09115568667</a></p>
</section>

<footer>© 2026 Real Lion FC</footer>

<script>
// ================= ADMIN CONTROL =================
let tapCount = 0;
document.getElementById("logo").addEventListener("click", function(){
    tapCount++;
    if(tapCount === 3){
        document.getElementById("adminBtn").style.display = "block";
        alert("Control unlocked!");
        tapCount = 0;
    }
    setTimeout(()=>{ tapCount=0; },2000);
});

function toggleLogin(){
    let box = document.getElementById("loginBox");
    box.style.display = box.style.display==="block"?"none":"block";
}

function login(){
    let code = document.getElementById("accessCode").value;
    if(code==="lion123"){
        localStorage.setItem("admin","true");
        showAdminPanel();
        document.getElementById("loginBox").style.display="none";
    } else { alert("Wrong code!"); }
}

function showAdminPanel(){
    document.getElementById("adminPanel").style.display="block";
    document.querySelectorAll(".adminOnly").forEach(el=>el.style.display="inline-block");
}

if(localStorage.getItem("admin")==="true") showAdminPanel();

function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// ================= DYNAMIC SECTIONS =================
let sections = JSON.parse(localStorage.getItem("sections")) || [
    {id:"pitchSection", name:"Starting XI", type:"list"}
];

function saveSections(){ localStorage.setItem("sections", JSON.stringify(sections)); }

function renderSections(){
    // Remove old dynamic sections
    document.querySelectorAll(".dynamicSection")?.forEach(el=>el.remove());
    let select = document.getElementById("selectSection");
    select.innerHTML="";
    sections.forEach(sec=>{
        if(sec.id==="pitchSection" || sec.id==="contactSection") return;
        let s = document.createElement("section");
        s.className="dynamicSection";
        s.id=sec.id;
        s.innerHTML = `<h2>${sec.name} <button class="deleteBtn adminOnly" onclick="deleteSection('${sec.id}')">X</button></h2><div id="${sec.id}List"></div>`;
        document.body.insertBefore(s, document.getElementById("contactSection"));
        let opt = document.createElement("option");
        opt.value=sec.id; opt.textContent=sec.name;
        select.appendChild(opt);
    });
    document.querySelectorAll(".adminOnly").forEach(el=>el.style.display="inline-block");
}
renderSections();

function addNewSection(){
    let name = document.getElementById("newSectionName").value.trim();
    let type = document.getElementById("newSectionType").value;
    if(!name) return alert("Enter a name");
    let id = name.replace(/\s+/g,"") + Date.now();
    sections.push({id,name,type});
    saveSections();
    renderSections();
    document.getElementById("newSectionName").value="";
}

function deleteSection(id){
    sections = sections.filter(sec=>sec.id!==id);
    localStorage.setItem("sections", JSON.stringify(sections));
    document.getElementById(id)?.remove();
    renderSections();
}

function addItem(){
    let sectionId = document.getElementById("selectSection").value;
    let sec = sections.find(s=>s.id===sectionId);
    if(!sec) return;
    let dataKey = sectionId+"_data";
    let data = JSON.parse(localStorage.getItem(dataKey))||[];
    if(sec.type==="list"){
        let text = document.getElementById("newItemText").value.trim();
        if(!text) return;
        data.push(text);
        localStorage.setItem(dataKey, JSON.stringify(data));
        document.getElementById(sectionId+"List").innerHTML="";
        data.forEach((d,i)=>{
            document.getElementById(sectionId+"List").innerHTML+=`<p>${d} <button class="deleteBtn adminOnly" onclick="deleteItem('${dataKey}',${i},'${sectionId}')">X</button></p>`;
        });
        document.getElementById("newItemText").value="";
    } else if(sec.type==="gallery"){
        let file=document.getElementById("newItemImage").files[0];
        if(!file) return;
        let reader = new FileReader();
        reader.onload=function(e){
            data.push(e.target.result);
            localStorage.setItem(dataKey, JSON.stringify(data));
            let list=document.getElementById(sectionId+"List");
            list.innerHTML="";
            data.forEach((img,i)=>{
                list.innerHTML+=`<div><img src="${img}" width="100"><br><button class="deleteBtn adminOnly" onclick="deleteItem('${dataKey}',${i},'${sectionId}')">X</button></div>`;
            });
        }
        reader.readAsDataURL(file);
    }
}

function deleteItem(key,index,sectionId){
    let data = JSON.parse(localStorage.getItem(key))||[];
    data.splice(index,1);
    localStorage.setItem(key, JSON.stringify(data));
    addItem(); // refresh
}
</script>

</body>
</html>
