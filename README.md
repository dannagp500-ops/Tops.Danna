
<html lang="es">
<head>
<meta charset="UTF-8">
<title>GUPAL CINE</title>
<style>
body{margin:0;font-family:Arial;background:#111;color:#fff;text-align:center;}
button{padding:12px 18px;margin:10px;border:none;border-radius:8px;background:#ff4757;color:#fff;cursor:pointer;font-size:16px;}
button:hover{background:#c0392b;}
.hide{display:none;}

/* PELÍCULAS */
#cine{display:flex;height:100vh;}
.sidebar{width:30%;background:#222;padding:20px;overflow-y:auto;}
.sidebar input{width:100%;padding:8px;margin-bottom:10px;border:none;border-radius:5px;}
.sidebar li{list-style:none;background:#333;margin:6px 0;padding:8px;border-radius:5px;cursor:pointer;}
.sidebar li:hover{background:#555;}
.content{flex:1;background:#fff;color:#000;padding:20px;overflow-y:auto;}
.movie-container{display:flex;}
.movie-container img{width:160px;margin-right:14px;border-radius:5px;}

/* TRIVIA */
#trivia{display:flex;justify-content:center;align-items:center;height:100vh;background:#eee;color:#000;}
#triviaBox{background:#fff;padding:20px;border-radius:10px;width:340px;}

/* REGISTRO */
#registro{display:flex;justify-content:center;align-items:center;height:100vh;background:#f3f6fb;color:#000;}
#regBox{background:#fff;padding:20px;width:320px;border-radius:10px;}
input{width:100%;padding:8px;margin:6px 0;border-radius:6px;border:1px solid #ccc;}
</style>
</head>
<body>

<!-- PANTALLA PRINCIPAL -->
<div id="inicio">
    <h1>🎬 BIENVENIDA A GUPAL CINE</h1>
    <p>Selecciona una opción:</p>
    <button onclick="mostrar('cine')">Ver Películas</button>
    <button onclick="mostrar('trivia')">Jugar Trivia</button>
    <button onclick="mostrar('registro')">Formulario de Registro</button>
</div>

<!-- PELÍCULAS -->
<div id="cine" class="hide">
  <button onclick="mostrar('inicio')">⬅ Volver</button>
  <div class="sidebar">
    <h2>Películas</h2>
    <input type="text" id="searchInput" placeholder="Buscar...">
    <ul id="movieList"></ul>
  </div>
  <div class="content" id="movieContent">
    <h2>Selecciona una película</h2>
  </div>
</div>

<!-- TRIVIA -->
<div id="trivia" class="hide">
  <div id="triviaBox">
    <button onclick="mostrar('inicio')">⬅ Volver</button>
    <h2>Trivia de Películas 🎥</h2>

    <div id="inicioT">
      <p>Presiona para comenzar</p>
      <button onclick="comenzar()">Comenzar</button>
    </div>

    <div id="pregunta" class="hide">
      <p id="textoPregunta"></p>
      <div id="opciones"></div>
      <button onclick="siguiente()">Siguiente</button>
    </div>

    <div id="resultado" class="hide">
      <h3 id="puntaje"></h3>
      <button onclick="reiniciar()">Jugar otra vez</button>
    </div>
  </div>
</div>

<!-- FORMULARIO -->
<div id="registro" class="hide">
  <div id="regBox">
    <button onclick="mostrar('inicio')">⬅ Volver</button>
    <h2>Registro</h2>
    <input type="text" id="nombre" placeholder="Nombre">
    <input type="email" id="correo" placeholder="Correo">
    <input type="password" id="clave" placeholder="Contraseña">
    <button onclick="guardar()">Guardar</button>
    <p id="msg"></p>
  </div>
</div>

<script>
// CAMBIO DE PANTALLAS
function mostrar(id){
  document.querySelectorAll("body > div").forEach(d=>d.classList.add("hide"));
  document.getElementById(id).classList.remove("hide");
}

/* LISTA DE 30 PELÍCULAS */
const movies = [
{title:"Gladiador",desc:"Un general romano lucha por su honor.",poster:"https://via.placeholder.com/160"},
{title:"Titanic",desc:"Una historia de amor en el famoso barco.",poster:"https://via.placeholder.com/160"},
{title:"Avatar",desc:"Viaje al mundo de Pandora.",poster:"https://via.placeholder.com/160"},
{title:"El Rey León",desc:"Un cachorro destinado a reinar.",poster:"https://via.placeholder.com/160"},
{title:"Harry Potter 1",desc:"El inicio del joven mago.",poster:"https://via.placeholder.com/160"},
{title:"Harry Potter 7",desc:"La batalla final.",poster:"https://via.placeholder.com/160"},
{title:"Spider-Man",desc:"El héroe arácnido.",poster:"https://via.placeholder.com/160"},
{title:"Iron Man",desc:"El inicio de los Vengadores.",poster:"https://via.placeholder.com/160"},
{title:"Los Vengadores",desc:"Héroes unidos.",poster:"https://via.placeholder.com/160"},
{title:"Black Panther",desc:"El legado de Wakanda.",poster:"https://via.placeholder.com/160"},
{title:"Coco",desc:"Un viaje al mundo de los muertos.",poster:"https://via.placeholder.com/160"},
{title:"Toy Story",desc:"Los juguetes tienen vida.",poster:"https://via.placeholder.com/160"},
{title:"Frozen",desc:"Un reino congelado.",poster:"https://via.placeholder.com/160"},
{title:"Ratatouille",desc:"Un ratón chef.",poster:"https://via.placeholder.com/160"},
{title:"Cars",desc:"Carreras y amistad.",poster:"https://via.placeholder.com/160"},
{title:"Shrek",desc:"Un ogro muy especial.",poster:"https://via.placeholder.com/160"},
{title:"It",desc:"El payaso Pennywise.",poster:"https://via.placeholder.com/160"},
{title:"El Conjuro",desc:"Terror basado en hechos reales.",poster:"https://via.placeholder.com/160"},
{title:"Annabelle",desc:"La muñeca poseída.",poster:"https://via.placeholder.com/160"},
{title:"Minions",desc:"Amarillos y locos.",poster:"https://via.placeholder.com/160"},
{title:"Intensamente",desc:"Las emociones cobran vida.",poster:"https://via.placeholder.com/160"},
{title:"Zootopia",desc:"Una ciudad de animales.",poster:"https://via.placeholder.com/160"},
{title:"Barbie",desc:"La muñeca en el mundo real.",poster:"https://via.placeholder.com/160"},
{title:"Dune",desc:"Una guerra en Arrakis.",poster:"https://via.placeholder.com/160"},
{title:"Tenet",desc:"Acción y tiempo.",poster:"https://via.placeholder.com/160"},
{title:"Batman",desc:"El vigilante de Gotham.",poster:"https://via.placeholder.com/160"},
{title:"Superman",desc:"El héroe de acero.",poster:"https://via.placeholder.com/160"},
{title:"Hulk",desc:"El gigante verde.",poster:"https://via.placeholder.com/160"},
{title:"Thor",desc:"El dios del trueno.",poster:"https://via.placeholder.com/160"},
{title:"Doctor Strange",desc:"La magia hecha realidad.",poster:"https://via.placeholder.com/160"},
];

const list=document.getElementById("movieList");
const content=document.getElementById("movieContent");

function showMovies(){
 list.innerHTML="";
 movies.forEach(m=>{
  let li=document.createElement("li");
  li.textContent=m.title;
  li.onclick=()=> content.innerHTML=`<div class='movie-container'><img src="${m.poster}"><div><h2>${m.title}</h2><p>${m.desc}</p></div></div>`;
  list.appendChild(li);
 });
}
showMovies();

// FILTRO
document.getElementById("searchInput").oninput=function(){
  const f=this.value.toLowerCase();
  list.innerHTML="";
  movies.filter(m=>m.title.toLowerCase().includes(f))
  .forEach(m=>{
    let li=document.createElement("li");
    li.textContent=m.title;
    li.onclick=()=> content.innerHTML=`<div class='movie-container'><img src="${m.poster}"><div><h2>${m.title}</h2><p>${m.desc}</p></div></div>`;
    list.appendChild(li);
 });
};

// TRIVIA
let preguntas=[
{texto:"¿Quién interpreta a Jack en Titanic?", opciones:["Leonardo DiCaprio","Brad Pitt","Chris Evans"], correcta:0},
{texto:"¿Qué película tiene a un payaso maligno?", opciones:["It","Shrek","Minions"], correcta:0},
];
let index=0, score=0;

function comenzar(){ inicioT.classList.add("hide"); pregunta.classList.remove("hide"); cargar(); }
function cargar(){ let p=preguntas[index]; textoPregunta.innerText=p.texto; opciones.innerHTML="";
p.opciones.forEach((o,i)=> opciones.innerHTML+=`<label><input type='radio' name='r' value='${i}'> ${o}</label><br>`);}
function siguiente(){ let s=document.querySelector("input[name='r']:checked"); if(!s)return alert("Selecciona una opción");
if(s.value==preguntas[index].correcta) score++; index++; if(index<preguntas.length)cargar(); else finalizar();}
function finalizar(){ pregunta.classList.add("hide"); resultado.classList.remove("hide"); puntaje.innerText=`Puntaje: ${score}/${preguntas.length}`;}
function reiniciar(){ location.reload(); }

// REGISTRO
function guardar(){
 localStorage.setItem("usuario",JSON.stringify({nombre:nombre.value,correo:correo.value,clave:clave.value}));
 msg.innerText="✅ Registrado con éxito";
}

</script>
</body>
</html>

