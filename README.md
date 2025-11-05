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
.sidebar { width:30%; background:#222; padding:20px; overflow-y:auto; }
.sidebar input { width:100%; padding:8px; margin-bottom:15px; }
.sidebar li { list-style:none; background:#444; margin:6px 0; padding:8px; cursor:pointer; border-radius:5px; }
.sidebar li:hover { background:#666; }
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
 { title:"LOTR: La Comunidad del Anillo (2001)", desc:"Frodo y sus amigos inician la misión de destruir el anillo único.", poster:"https://via.placeholder.com/150x220?text=LOTR1", opinions:["Una de las mejores sagas de fantasía.","Muy larga para ver de una sola vez.","Impresionantes efectos visuales."] },
  { title:"Spider-Man (2002)", desc:"Peter Parker obtiene poderes arácnidos y lucha contra el mal en Nueva York.", poster:"https://via.placeholder.com/150x220?text=Spider-Man", opinions:["Tobey Maguire es increíble como Spider-Man.","Algunos efectos se ven antiguos hoy.","Historia entretenida y emocionante."] },
  { title:"Piratas del Caribe (2003)", desc:"Jack Sparrow busca recuperar su barco y un tesoro maldito.", poster:"https://via.placeholder.com/150x220?text=Piratas+Caribe", opinions:["Jack Sparrow es icónico y divertido.","La historia a veces es confusa.","Gran mezcla de acción y humor."] },
  { title:"Los Increíbles (2004)", desc:"Una familia de superhéroes trata de vivir una vida normal mientras salva al mundo.", poster:"https://via.placeholder.com/150x220?text=Increibles", opinions:["Muy divertida y emocionante.","No profundiza mucho en algunos personajes.","Excelente animación y música."] },
  { title:"Batman Inicia (2005)", desc:"El origen de Batman y su lucha contra el crimen en Gotham.", poster:"https://via.placeholder.com/150x220?text=Batman+Inicia", opinions:["Gran reinicio oscuro y realista.","Al principio puede parecer lenta.","Christian Bale actúa increíble."] },
  { title:"Ratatouille (2007)", desc:"Un ratón con pasión por la cocina busca cumplir su sueño en París.", poster:"https://via.placeholder.com/150x220?text=Ratatouille", opinions:["Inspiradora y tierna.","Algunos diálogos son simples para adultos.","Historia muy original y encantadora."] },
  { title:"El Caballero Oscuro (2008)", desc:"Batman enfrenta al Joker en una batalla por Gotham.", poster:"https://via.placeholder.com/150x220?text=Caballero+Oscuro", opinions:["Heath Ledger es impresionante.","Escenas muy intensas para niños.","Suspenso y acción perfectos."] },
  { title:"Avatar (2009)", desc:"Un exmarine explora Pandora y su conexión con los Na’vi.", poster:"https://via.placeholder.com/150x220?text=Avatar", opinions:["Visualmente impresionante.","Historia predecible.","Gran mundo y diseño de personajes."] },
  { title:"Origen (2010)", desc:"Un equipo entra en los sueños para robar secretos y plantar ideas.", poster:"https://via.placeholder.com/150x220?text=Origen", opinions:["Muy creativa y emocionante.","Puede ser confusa al principio.","Efectos visuales y trama fascinantes."] },
  { title:"Harry Potter 7 (2011)", desc:"El final épico de la saga de Harry Potter enfrentando a Voldemort.", poster:"https://via.placeholder.com/150x220?text=HP7", opinions:["Muy emotiva y épica.","Algunos personajes tienen poco desarrollo.","Cierre espectacular de la saga."] },
  { title:"Los Vengadores (2012)", desc:"Los superhéroes se unen contra Loki y un ejército alienígena.", poster:"https://via.placeholder.com/150x220?text=Vengadores", opinions:["Épico y entretenido.","Demasiados personajes para algunos.","Perfecta combinación de acción y humor."] },
  { title:"Frozen (2013)", desc:"Elsa y Anna enfrentan un invierno eterno y descubren el poder del amor familiar.", poster:"https://via.placeholder.com/150x220?text=Frozen", opinions:["Canciones inolvidables y divertida.","Algunas partes predecibles.","Gran animación y música pegajosa."] },
  { title:"Guardianes de la Galaxia (2014)", desc:"Un grupo de inadaptados salva la galaxia.", poster:"https://via.placeholder.com/150x220?text=Guardianes+Galaxia", opinions:["Muy divertida y con buena música.","Algunos chistes no funcionan para todos.","Personajes carismáticos y acción trepidante."] },
  { title:"Intensamente (2015)", desc:"Las emociones de una niña cobran vida y la ayudan a enfrentar su vida diaria.", poster:"https://via.placeholder.com/150x220?text=Intensamente", opinions:["Muy original y conmovedora.","Algunos conceptos son complejos para niños pequeños.","Divertida y educativa."] },
  { title:"Zootrópolis (2016)", desc:"Una coneja policía y un zorro resuelven un caso importante en la ciudad de animales.", poster:"https://via.placeholder.com/150x220?text=Zootropolis", opinions:["Ingeniosa y con mensaje social.","La trama puede ser simple para adultos.","Divertida y entretenida."] },
  { title:"Coco (2017)", desc:"Un niño viaja al mundo de los muertos para descubrir su historia familiar.", poster:"https://via.placeholder.com/150x220?text=Coco", opinions:["Emocionante y hermosa.","Puede ser triste para los niños.","Canciones y colores maravillosos."] },
  { title:"Pantera Negra (2018)", desc:"T’Challa defiende Wakanda de nuevas amenazas.", poster:"https://via.placeholder.com/150x220?text=Pantera+Negra", opinions:["Inspiradora y culturalmente importante.","Algunos villanos no desarrollados.","Acción y efectos visuales espectaculares."] },
  { title:"Avengers: Endgame (2019)", desc:"Los héroes intentan revertir el chasquido de Thanos y salvar el universo.", poster:"https://via.placeholder.com/150x220?text=Endgame", opinions:["Épico y emocionante.","Puede ser confuso si no viste las películas previas.","Excelente cierre de saga."] },
  { title:"Tenet (2020)", desc:"Un agente manipula el tiempo para evitar la Tercera Guerra Mundial.", poster:"https://via.placeholder.com/150x220?text=Tenet", opinions:["Innovadora y visualmente impresionante.","Difícil de entender para algunos.","Muy original y entretenida."] },
  { title:"Dune (2021)", desc:"Un joven noble enfrenta su destino en un planeta desértico lleno de conspiraciones.", poster:"https://via.placeholder.com/150x220?text=Dune", opinions:["Visualmente espectacular.","Puede parecer lenta al inicio.","Gran adaptación de la novela."] },
  { title:"El Batman (2022)", desc:"Batman investiga a un asesino en serie en Gotham.", poster:"https://via.placeholder.com/150x220?text=Batman+2022", opinions:["Muy oscura y detectivesca.","Puede ser demasiado seria para niños.","Gran interpretación de Robert Pattinson."] },
  { title:"Oppenheimer (2023)", desc:"La historia del creador de la bomba atómica y sus dilemas morales.", poster:"https://via.placeholder.com/150x220?text=Oppenheimer", opinions:["Profunda y emocionante.","Difícil de entender sin contexto histórico.","Actuaciones impresionantes."] },
  { title:"Barbie (2023)", desc:"Barbie cuestiona su mundo y viaja al mundo real.", poster:"https://via.placeholder.com/150x220?text=Barbie", opinions:["Divertida y con mensaje social.","Algunas escenas son demasiado fantásticas.","Muy colorida y entretenida."] },
  { title:"Spider-Man: A Través del Spider-Verso (2023)", desc:"Miles Morales explora el multiverso.", poster:"https://via.placeholder.com/150x220?text=Spider-Verso", opinions:["Visualmente increíble y divertida.","Algunos saltos de historia confusos.","Gran animación y creatividad."] },
  { title:"Dune: Parte Dos (2024)", desc:"Paul Atreides lidera la rebelión en Arrakis.", poster:"https://via.placeholder.com/150x220?text=Dune2", opinions:["Épica y espectacular.","Muchos nombres y lugares difíciles de recordar.","Gran dirección y efectos."] },
  { title:"Intensamente 2 (2024)", desc:"Riley enfrenta nuevas emociones en su adolescencia.", poster:"https://via.placeholder.com/150x220?text=Intensamente2", opinions:["Divertida y emotiva.","No tan impactante como la primera.","Explora bien la vida adolescente."] },
  { title:"Avatar: El Camino del Agua (2025)", desc:"Jake Sully y Neytiri exploran los océanos de Pandora.", poster:"https://via.placeholder.com/150x220?text=Avatar2", opinions:["Increíble mundo visual.","Historia simple para algunos.","Efectos impresionantes y emocionante."] }
];

// Elementos del DOM
const movieList = document.getElementById("movieList");
const movieContent = document.getElementById("movieContent");
const searchInput = document.getElementById("searchInput");

// Función para mostrar película en el contenido
function showMovie(movie){
  movieContent.innerHTML = `
    <div class="movie-container">
      <img src="${movie.poster}" alt="${movie.title}">
      <div class="movie-info">
        <h2 class="movie-title">${movie.title}</h2>
        <p class="movie-desc">${movie.desc}</p>
        <div class="opinions"><b>Opiniones:</b>
          <ul>
            <li>👍 ${movie.opinions[0]}</li>
            <li>😐 ${movie.opinions[1]}</li>
            <li>👎 ${movie.opinions[2]}</li>
          </ul>
        </div>
      </div>
    </div>
  `;
}

// Función para mostrar lista de películas
function displayMovies(filter=""){
  movieList.innerHTML="";
  movies.filter(m => m.title.toLowerCase().includes(filter.toLowerCase()))
        .forEach(m => {
          const li = document.createElement("li");
          li.textContent = m.title;
          li.onclick = () => showMovie(m);
          movieList.appendChild(li);
        });
}

// Buscar por input
searchInput.addEventListener("input", () => displayMovies(searchInput.value));

// Mostrar todas las películas al cargar
displayMovies();
</script>
</body>
</html>

