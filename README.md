<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>GUPAL CINE</title>

<style>
body { font-family: Arial, sans-serif; margin:0; text-align:center; background:#111; color:#fff; }
button{ padding:12px 20px; background:#ff4757; border:none; border-radius:8px; color:white; margin:10px; cursor:pointer; font-size:16px; }
button:hover{ background:#c0392b; }
.hide{ display:none; }

/* Estilos de la sección de cine */
#cineContainer{ display:flex; height:100vh; }
.sidebar { width:30%; background:#333; padding:20px; overflow-y:auto; }
.sidebar input { width:100%; padding:8px; margin-bottom:15px; }
.sidebar li { list-style:none; background:#555; margin:6px 0; padding:8px; cursor:pointer; border-radius:5px; }
.sidebar li:hover { background:#777; }
.content { flex:1; background:#fff; color:#000; padding:20px; overflow-y:auto; }

/* Estilos trivia */
#triviaContainer{ display:flex; justify-content:center; align-items:center; height:100vh; background:#eee; }
#container{ background:#fff; padding:20px; border-radius:10px; width:350px; color:#000; }

/* Estilos registro */
#registroContainer{ background:#f3f6fb; min-height:100vh; display:flex; justify-content:center; align-items:center; color:#000; }
#formBox{ background:#fff; padding:20px; border-radius:10px; width:350px; }
input{ width:100%; padding:8px; margin:6px 0; }

</style>
</head>

<body>

<!-- ============= PANTALLA DE INICIO ============= -->
<div id="inicioPantalla">
    <h1>🎬 BIENVENIDA A GUPAL CINE</h1>
    <p>Selecciona una opción:</p>
    <button onclick="mostrar('cineContainer')">Ver Películas</button>
    <button onclick="mostrar('triviaContainer')">Jugar Trivia</button>
    <button onclick="mostrar('registroContainer')">Formulario de Registro</button>
</div>

<!-- ============= SECCIÓN DE PELÍCULAS ============= -->
<div id="cineContainer" class="hide">
  <button onclick="mostrar('inicioPantalla')">⬅ Volver</button>
  <div class="sidebar">
    <h2>Películas</h2>
    <input type="text" id="searchInput" placeholder="Buscar...">
    <ul id="movieList"></ul>
  </div>
  <div class="content" id="movieContent">
    <h2>Bienvenido a GUPAL CINE</h2>
    <p>Selecciona una película para ver detalles.</p>
  </div>
</div>

<!-- ============= TRIVIA ============= -->
<div id="triviaContainer" class="hide">
  <div id="container">
    <button onclick="mostrar('inicioPantalla')">⬅ Volver</button>
    <h2>Trivia de Películas 🎬</h2>

    <div id="inicio">
        <p>Presiona el botón para comenzar</p>
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

<!-- ============= REGISTRO ============= -->
<div id="registroContainer" class="hide">
  <div id="formBox">
    <button onclick="mostrar('inicioPantalla')">⬅ Volver</button>
    <h2>Registro</h2>
    <input type="text" id="nombre" placeholder="Tu nombre">
    <input type="email" id="correo" placeholder="Correo">
    <input type="password" id="clave" placeholder="Contraseña">
    <button onclick="guardarRegistro()">Guardar</button>
    <p id="mensaje"></p>
  </div>
</div>

<script>
function mostrar(id){
  document.getElementById("inicioPantalla").classList.add("hide");
  document.getElementById("cineContainer").classList.add("hide");
  document.getElementById("triviaContainer").classList.add("hide");
  document.getElementById("registroContainer").classList.add("hide");
  document.getElementById(id).classList.remove("hide");
}

/* Registro */
function guardarRegistro(){
  let nombre=document.getElementById("nombre").value;
  let correo=document.getElementById("correo").value;
  let clave=document.getElementById("clave").value;
  localStorage.setItem("usuario",JSON.stringify({nombre,correo,clave}));
  document.getElementById("mensaje").innerText="✅ Registro guardado correctamente";
}

/* Trivia */
let preguntas=[ {texto:"¿Quién interpreta a Jack en Titanic?", opciones:["Leonardo DiCaprio","Brad Pitt","Tom Cruise"], correcta:0}, 
{texto:"¿Qué película tiene a Pennywise?", opciones:["Annabelle","It","El Conjuro"], correcta:1} ];

let indice=0, aciertos=0;

function comenzar(){ document.getElementById("inicio").classList.add("hide"); document.getElementById("pregunta").classList.remove("hide"); mostrarPregunta(); }
function mostrarPregunta(){ let q=preguntas[indice]; document.getElementById("textoPregunta").innerText=q.texto; let cont=document.getElementById("opciones"); cont.innerHTML="";
q.opciones.forEach((o,i)=> cont.innerHTML+=`<label><input type='radio' name='r' value='${i}'> ${o}</label><br>`); }
function siguiente(){ let s=document.querySelector("input[name='r']:checked"); if(!s) return alert("Selecciona una opción");
if(s.value==preguntas[indice].correcta) aciertos++; indice++; if(indice<preguntas.length) mostrarPregunta(); else terminar(); }
function terminar(){ document.getElementById("pregunta").classList.add("hide"); document.getElementById("resultado").classList.remove("hide"); document.getElementById("puntaje").innerText=`Puntaje: ${aciertos}/${preguntas.length}`; }
function reiniciar(){ location.reload(); }

/* Películas */
const movies=[
{title:"Gladiador",desc:"Un general romano busca venganza.",poster:"https://via.placeholder.com/150"},];
const movieList=document.getElementById("movieList");
const movieContent=document.getElementById("movieContent");
function displayMovies(){ movieList.innerHTML=""; movies.forEach(m=>{ let li=document.createElement("li"); li.textContent=m.title; li.onclick=()=> movieContent.innerHTML=`<h2>${m.title}</h2><p>${m.desc}</p>`; movieList.appendChild(li); }); }
displayMovies();
</script>

</body>
</html>
>
