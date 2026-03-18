<script>
// VARIABLES
let adminPanel = document.getElementById("adminPanel");
let loginBox = document.getElementById("loginBox");
let pitch = document.getElementById("pitch");

// TOGGLE LOGIN
function toggleLogin(){
    loginBox.style.display = loginBox.style.display==="none" ? "block" : "none";
}

// LOGIN
function login(){
    let u = document.getElementById("user").value;
    let p = document.getElementById("pass").value;

    if(u==="admin" && p==="1234"){
        localStorage.setItem("admin","true");
        adminPanel.style.display="block";
        loginBox.style.display="none";

        // SHOW DELETE BUTTONS (NO RELOAD)
        document.querySelectorAll(".adminOnly").forEach(el=>{
            el.style.display="inline-block";
        });

    }else{
        alert("Wrong login!");
    }
}

// AUTO LOGIN
if(localStorage.getItem("admin")==="true"){
    adminPanel.style.display="block";

    setTimeout(()=>{
        document.querySelectorAll(".adminOnly").forEach(el=>{
            el.style.display="inline-block";
        });
    },100);
}

// LOGOUT
function logout(){
    localStorage.removeItem("admin");
    location.reload();
}

// FORMATIONS
let formation="433";
const positions={
"433":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:25},{top:250,left:45},{top:250,left:65},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"442":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:250,left:15},{top:250,left:35},{top:250,left:55},{top:250,left:75},{top:100,left:35},{top:100,left:55}],
"352":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:250,left:90},{top:100,left:35},{top:100,left:55}],
"4231":[{top:450,left:45},{top:350,left:15},{top:350,left:35},{top:350,left:55},{top:350,left:75},{top:280,left:35},{top:280,left:55},{top:200,left:25},{top:200,left:45},{top:200,left:65},{top:80,left:45}],
"343":[{top:450,left:45},{top:350,left:25},{top:350,left:45},{top:350,left:65},{top:250,left:10},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:25},{top:80,left:45},{top:100,left:65}],
"532":[{top:450,left:45},{top:350,left:10},{top:350,left:30},{top:350,left:50},{top:350,left:70},{top:350,left:90},{top:250,left:30},{top:250,left:50},{top:250,left:70},{top:100,left:35},{top:100,left:55}]
};

// FORMATION SWITCH
function setFormation(){
    formation=document.getElementById("formation").value;
    reloadTeam();
}

// ADD PLAYER TO PITCH
function addPlayer(){
    let team = JSON.parse(localStorage.getItem("team"))||[];
    if(team.length>=11){ alert("Max 11"); return; }

    let file = document.getElementById("image").files[0];
    let reader = new FileReader();

    reader.onload = function(e){
        team.push({name:document.getElementById("name").value, img:e.target.result, x:0, y:0});
        localStorage.setItem("team",JSON.stringify(team));
        reloadTeam();
    };
    if(file) reader.readAsDataURL(file);
}

// RELOAD PITCH
function reloadTeam(){
    pitch.innerHTML="";
    let team = JSON.parse(localStorage.getItem("team"))||[];

    team.forEach((p,i)=>{
        let pos = positions[formation][i];
        let div = document.createElement("div");
        div.className="player";
        div.style.top=(p.y||pos.top)+"px";
        div.style.left=(p.x||pos.left)+"%";

        div.innerHTML=`<img src="${p.img}"><span>${p.name}</span>`;

        // DRAG
        div.onmousedown = function(e){
            let ox=e.offsetX, oy=e.offsetY;

            function moveHandler(e){
                div.style.left=(e.pageX-pitch.offsetLeft-ox)/pitch.offsetWidth*100+"%";
                div.style.top=(e.pageY-pitch.offsetTop-oy)+"px";
            }

            document.addEventListener("mousemove",moveHandler);

            document.addEventListener("mouseup",()=>{
                document.removeEventListener("mousemove",moveHandler);
                saveTeam();
            }, {once:true});
        };

        // DOUBLE CLICK REMOVE
        div.ondblclick=function(){
            team.splice(i,1);
            localStorage.setItem("team",JSON.stringify(team));
            reloadTeam();
        };

        pitch.appendChild(div);
    });
}

// SAVE PITCH
function saveTeam(){
    let team=[];
    document.querySelectorAll(".player").forEach(el=>{
        team.push({
            name: el.querySelector("span").textContent,
            img: el.querySelector("img").src,
            x: parseFloat(el.style.left),
            y: parseFloat(el.style.top)
        });
    });
    localStorage.setItem("team",JSON.stringify(team));
}

// CLEAR PITCH
function clearTeam(){
    localStorage.removeItem("team");
    reloadTeam();
}

// DELETE ITEM
function deleteItem(type,index){
    let data=JSON.parse(localStorage.getItem(type))||[];
    data.splice(index,1);
    localStorage.setItem(type,JSON.stringify(data));
    loadAll();
}

// LOAD LIST (WITH SMALL INLINE BUTTON)
function loadList(type,element){
    let list=document.getElementById(element);
    list.innerHTML="";

    (JSON.parse(localStorage.getItem(type))||[]).forEach((item,i)=>{
        list.innerHTML+=`
        <p style="display:flex; justify-content:center; align-items:center; gap:5px;">
            ${item}
            <button class="deleteBtn adminOnly" onclick="deleteItem('${type}',${i})">✖</button>
        </p>`;
    });
}

// LOAD GALLERY
function loadGallery(){
    let list=document.getElementById("galleryList");
    list.innerHTML="";

    (JSON.parse(localStorage.getItem("gallery"))||[]).forEach((img,i)=>{
        list.innerHTML+=`
        <div style="margin:10px;">
            <img src="${img}" width="100"><br>
            <button class="deleteBtn adminOnly" onclick="deleteItem('gallery',${i})">✖</button>
        </div>`;
    });
}

// ADD ITEMS
function addPlayerInfo(){
    let data = JSON.parse(localStorage.getItem("players"))||[];
    data.push(document.getElementById("playerName").value);
    localStorage.setItem("players",JSON.stringify(data));
    loadList("players","playersList");
}

function addMatch(){
    let data = JSON.parse(localStorage.getItem("matches"))||[];
    data.push(document.getElementById("matchText").value);
    localStorage.setItem("matches",JSON.stringify(data));
    loadList("matches","matchesList");
}

function addNews(){
    let data = JSON.parse(localStorage.getItem("news"))||[];
    data.push(document.getElementById("newsText").value);
    localStorage.setItem("news",JSON.stringify(data));
    loadList("news","newsList");
}

function addGallery(){
    let file=document.getElementById("galleryImage").files[0];
    let reader=new FileReader();

    reader.onload=function(e){
        let data=JSON.parse(localStorage.getItem("gallery"))||[];
        data.push(e.target.result);
        localStorage.setItem("gallery",JSON.stringify(data));
        loadGallery();
    };

    if(file) reader.readAsDataURL(file);
}

// INIT
function loadAll(){
    reloadTeam();
    loadList("players","playersList");
    loadList("matches","matchesList");
    loadList("news","newsList");
    loadGallery();
}

loadAll();
</script>
