<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GUPAL CINE</title>
<style>
body { font-family: Arial, sans-serif; margin:0; display:flex; height:100vh; }
header { position:fixed; top:0; width:100%; background:#222; color:white; padding:10px; display:flex; align-items:center; z-index:1000; }
header img { height:50px; margin-right:15px; }
header h1 { font-size:20px; margin:0; }
.container { display:flex; width:100%; margin-top:70px; }
.sidebar { width:30%; background:#333; color:white; padding:20px; overflow-y:auto; height:calc(100vh - 70px); }
.sidebar input { width:100%; padding:8px; margin-bottom:15px; }
.sidebar ul { list-style:none; padding:0; }
.sidebar li { padding:8px; margin:5px 0; background:#444; cursor:pointer; border-radius:5px; }
.sidebar li:hover { background:#666; }
.content { flex:1; padding:20px; overflow-y:auto; background:white; }
.movie-container { display:flex; margin-bottom:20px; }
.movie-container img { width:150px; height:auto; margin-right:15px; border-radius:5px; }
.movie-info { max-width: calc(100% - 160px); }
.movie-title { font-size:24px; margin-bottom:10px; }
.movie-desc { font-size:16px; margin-bottom:10px; }
.opinions { background:#f9f9f9; padding:10px; border-left:3px solid #222; }
.opinions ul { padding-left:20px; margin:0; }
</style>
</head>
<body>
<header>
  <img src="https://i.ibb.co/BH2TCgwv/Blue-Black-Modern-Simple-Design-Hotel-and-Resort-Logo-Logos-4.png" alt="Logo de GUPAL CINE">
  <h1>🎥 GUPAL CINE – Creado por Danna Michell Guardo Palencia</h1>
</header>

<div class="container">
  <div class="sidebar">
    <h2>Películas</h2>
    <input type="text" id="searchInput" placeholder="Buscar...">
    <ul id="movieList"></ul>
  </div>
  <div class="content" id="movieContent">
    <h2>Bienvenido a GUPAL CINE</h2>
    <p>Selecciona una película de la lista o búscala arriba para ver su descripción, poster y opiniones.</p>
  </div>
</div>

<script>
// Lista de 30 películas en español con descripción, poster y opiniones
const movies = [
  { title:"Gladiador (2000)", desc:"Un general romano traicionado busca venganza en la arena y recuperar su honor.", poster:"https://via.placeholder.com/150x220?text=Gladiador", opinions:["Excelente historia y actuaciones.","Demasiada violencia para algunos.","Muy épica y emocionante."] },
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


<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Trivia de Películas</title>
<style>
    body{
        font-family: Arial, sans-serif;
        background: #eee;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    #container{
        background: white;
        padding: 20px;
        width: 350px;
        text-align: center;
        border-radius: 10px;
        box-shadow: 0px 0px 10px rgba(0,0,0,0.2);
    }
    .hide{ display:none; }
    button{
        margin-top: 10px;
        padding: 10px 20px;
        border: none;
        background: #4A63E7;
        color: #fff;
        border-radius: 8px;
        cursor: pointer;
    }
    button:hover{ background:#2a41ad; }
</style>
</head>
<body>

<div id="container">
    <h2>Trivia de Películas 🎬</h2>

    <!-- Botón para iniciar -->
    <div id="inicio">
        <p>Presiona el botón para comenzar</p>
        <button onclick="comenzar()">Comenzar</button>
    </div>

    <!-- Preguntas -->
    <div id="pregunta" class="hide">
        <p id="textoPregunta"></p>
        <div id="opciones"></div>
        <button onclick="siguiente()">Siguiente</button>
    </div>

    <!-- Resultado -->
    <div id="resultado" class="hide">
        <h3 id="puntaje"></h3>
        <button onclick="reiniciar()">Jugar otra vez</button>
    </div>
</div>

<script>
let preguntas = [
    {texto:"¿Quién interpreta a Jack en Titanic?", opciones:["Leonardo DiCaprio", "Brad Pitt", "Tom Cruise"], correcta:0},
    {texto:"¿Qué película presenta a un payaso llamado Pennywise?", opciones:["Annabelle","It","El Conjuro"], correcta:1},
    {texto:"¿Cuál es la película animada donde hay unos juguetes que cobran vida?", opciones:["Toy Story","Shrek","Cars"], correcta:0},
    {texto:"¿Qué superhéroe es parte del universo de Marvel?", opciones:["Batman","Iron Man","Wolverine de DC"], correcta:1}
];

let indice = 0;
let aciertos = 0;

function comenzar(){
    document.getElementById("inicio").classList.add("hide");
    document.getElementById("pregunta").classList.remove("hide");
    mostrarPregunta();
}

function mostrarPregunta(){
    document.getElementById("textoPregunta").innerText = preguntas[indice].texto;
    let contOpciones = document.getElementById("opciones");
    contOpciones.innerHTML = "";

    preguntas[indice].opciones.forEach((opc, i)=>{
        contOpciones.innerHTML += `<label><br><input type='radio' name='resp' value='${i}'> ${opc}</label>`;
    });
}

function siguiente(){
    let seleccion = document.querySelector("input[name='resp']:checked");

    if(seleccion){
        if(parseInt(seleccion.value) === preguntas[indice].correcta){
            aciertos++;
        }
        indice++;

        if(indice < preguntas.length){
            mostrarPregunta();
        } else {
            terminar();
        }
    } else {
        alert("Selecciona una respuesta antes de continuar.");
    }
}

function terminar(){
    document.getElementById("pregunta").classList.add("hide");
    document.getElementById("resultado").classList.remove("hide");
    document.getElementById("puntaje").innerText = `Puntaje final: ${aciertos} / ${preguntas.length}`;
}

function reiniciar(){
    indice = 0;
    aciertos = 0;
    document.getElementById("resultado").classList.add("hide");
    document.getElementById("inicio").classList.remove("hide");
}
</script>

</body>
</html>


<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Registro Ficticio — Demo</title>
  <style>
    :root{--bg:#f3f6fb;--card:#ffffff;--accent:#2b7cff;--muted:#6b7280}
    *{box-sizing:border-box;font-family:Inter,system-ui,Arial,sans-serif}
    body{margin:0;background:var(--bg);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:20px}
    .wrap{width:940px;max-width:100%;display:grid;grid-template-columns:1fr 420px;gap:20px}
    .panel{background:var(--card);border-radius:12px;padding:22px;box-shadow:0 8px 30px rgba(15,15,30,0.06)}
    .hero{display:flex;flex-direction:column;gap:12px;justify-content:center;align-items:flex-start;padding:32px}
    h1{margin:0;font-size:22px}
    p.lead{margin:0;color:var(--muted)}
    .cta{margin-top:8px}
    button{cursor:pointer;border:0;padding:10px 16px;border-radius:10px;background:var(--accent);color:#fff;font-weight:600}
    button.ghost{background:#fff;color:var(--accent);border:1px solid #e6eefc}
    form{display:grid;gap:10px}
    label{font-size:13px;color:#111;margin-bottom:4px}
    input,select{padding:10px;border-radius:8px;border:1px solid #e9eef6}
    .small{font-size:13px;color:var(--muted)}
    .flex{display:flex;gap:8px;align-items:center}
    .hide{display:none}
    .center{display:flex;justify-content:center;align-items:center}
    .profile-info{font-size:14px;color:#111}
    .muted{color:var(--muted);font-size:13px}
    footer{font-size:12px;color:var(--muted);margin-top:10px}
    @media(max-width:880px){.wrap{grid-template-columns:1fr;}.hero{align-items:center;text-align:center}}
  </style>
</head>
<body>

  <div class="wrap">
    <!-- Lado izquierdo: presentación + boton que abre el registro -->
    <div class="panel hero">
      <h1>Bienvenido a DemoRegistro</h1>
      <p class="lead">Página ficticia para practicar: haz clic en el botón y crea un perfil localmente. No se envía nada a servidores.</p>

      <div class="cta">
        <button id="openRegisterBtn">Registrarse</button>
        <button id="openProfileBtn" class="ghost" style="margin-left:8px">Ver mi perfil</button>
      </div>

      <div style="margin-top:18px" class="small">Consejos:</div>
      <ul style="margin:8px 0 0 18px;color:var(--muted)">
        <li>Los datos se guardan en tu navegador (localStorage).</li>
        <li>Puedes editar o borrar el perfil después de guardarlo.</li>
        <li>Es una demo, no uses contraseñas reales.</li>
      </ul>

      <footer>Demo creada para prácticas — funciona offline.</footer>
    </div>

    <!-- Lado derecho: panel que contiene vistas intercambiables -->
    <div class="panel" id="panelRight">
      <!-- Vista: botón grande (oculta si se abre registro directo) -->
      <div id="view-home">
        <h2>Registro rápido</h2>
        <p class="muted">Pulsa <strong>Registrarse</strong> para abrir el formulario.</p>
      </div>

      <!-- Vista: formulario de registro -->
      <div id="view-register" class="hide">
        <h2>Formulario de registro</h2>
        <form id="formReg" autocomplete="off">
          <div>
            <label for="name">Nombre completo</label>
            <input id="name" name="name" type="text" placeholder="Tu nombre" required>
          </div>

          <div>
            <label for="email">Correo electrónico</label>
            <input id="email" name="email" type="email" placeholder="ejemplo@correo.com" required>
          </div>

          <div>
            <label for="age">Edad</label>
            <input id="age" name="age" type="number" min="5" max="120" placeholder="18">
          </div>

          <div>
            <label for="password">Contraseña (demo)</label>
            <input id="password" name="password" type="password" minlength="4" placeholder="••••" required>
          </div>

          <div class="flex" style="margin-top:8px">
            <button type="submit">Guardar perfil</button>
            <button type="button" id="cancelRegister" class="ghost">Cancelar</button>
          </div>

          <div id="msgReg" class="muted" style="margin-top:8px"></div>
        </form>
      </div>

      <!-- Vista: perfil guardado -->
      <div id="view-profile" class="hide">
        <h2>Mi perfil</h2>
        <div id="profileCard" class="profile-info muted">No hay perfil guardado.</div>

        <div style="margin-top:12px" class="flex">
          <button id="editProfile">Editar</button>
          <button id="deleteProfile" class="ghost" style="margin-left:8px">Borrar</button>
          <button id="backHome" class="ghost" style="margin-left:auto">Volver</button>
        </div>

        <div style="margin-top:12px" class="small">Historial local: <span id="historyCount">0</span> registros (demo)</div>
      </div>
    </div>
  </div>

<script>
  // Elementos principales
  const openRegisterBtn = document.getElementById('openRegisterBtn');
  const openProfileBtn = document.getElementById('openProfileBtn');
  const viewHome = document.getElementById('view-home');
  const viewRegister = document.getElementById('view-register');
  const viewProfile = document.getElementById('view-profile');

  const formReg = document.getElementById('formReg');
  const msgReg = document.getElementById('msgReg');
  const cancelRegister = document.getElementById('cancelRegister');

  const profileCard = document.getElementById('profileCard');
  const editProfile = document.getElementById('editProfile');
  const deleteProfile = document.getElementById('deleteProfile');
  const backHome = document.getElementById('backHome');
  const historyCount = document.getElementById('historyCount');

  // Mostrar/ocultar vistas
  function show(view){
    viewHome.classList.add('hide');
    viewRegister.classList.add('hide');
    viewProfile.classList.add('hide');
    view.classList.remove('hide');
    msgReg.textContent = '';
  }

  // Guardar perfil ficticio en localStorage
  function saveProfile(obj){
    // guardamos como userProfile y además agregamos a historial
    localStorage.setItem('userProfile', JSON.stringify(obj));
    // historial simple
    const hist = JSON.parse(localStorage.getItem('userProfileHistory') || '[]');
    hist.push({name:obj.name,email:obj.email,age:obj.age,date:new Date().toISOString()});
    localStorage.setItem('userProfileHistory', JSON.stringify(hist));
  }

  function loadProfile(){
    const p = localStorage.getItem('userProfile');
    if(!p) return null;
    try { return JSON.parse(p); } catch(e) { return null; }
  }

  function renderProfile(){
    const p = loadProfile();
    const hist = JSON.parse(localStorage.getItem('userProfileHistory') || '[]');
    historyCount.textContent = hist.length;
    if(!p){
      profileCard.innerHTML = '<div class="muted">Aún no has guardado un perfil.</div>';
      return;
    }
    profileCard.innerHTML = `
      <strong>${escapeHtml(p.name)}</strong>
      <div class="muted">${escapeHtml(p.email)} ${p.age ? '• ' + escapeHtml(String(p.age)) + ' años' : ''}</div>
    `;
  }

  // Eventos de botones
  openRegisterBtn.addEventListener('click', ()=> {
    // si existe perfil, prellenar
    const p = loadProfile();
    if(p){
      formReg.name.value = p.name || '';
      formReg.email.value = p.email || '';
      formReg.age.value = p.age || '';
      // No rellenamos la contraseña
    } else {
      formReg.reset();
    }
    show(viewRegister);
  });

  cancelRegister.addEventListener('click', ()=> show(viewHome));

  openProfileBtn.addEventListener('click', ()=>{
    renderProfile();
    show(viewProfile);
  });

  backHome.addEventListener('click', ()=> show(viewHome));

  // Guardar desde el formulario
  formReg.addEventListener('submit', (e)=>{
    e.preventDefault();
    const data = {
      name: formReg.name.value.trim(),
      email: formReg.email.value.trim(),
      age: formReg.age.value ? Number(formReg.age.value) : null,
      password: formReg.password.value // demo: guardado local
    };
    // Validaciones simples
    if(!data.name || !validateEmail(data.email) || (data.password && data.password.length < 4)){
      msgReg.textContent = 'Por favor completa correctamente los campos.';
      return;
    }
    saveProfile(data);
    msgReg.textContent = 'Perfil guardado localmente. ✅';
    renderProfile();
    // mostrar perfil luego de 700ms para dar feedback
    setTimeout(()=> show(viewProfile), 700);
  });

  // Editar: abre el formulario con datos
  editProfile.addEventListener('click', ()=>{
    const p = loadProfile();
    if(!p) { alert('No hay perfil para editar.'); return; }
    formReg.name.value = p.name || '';
    formReg.email.value = p.email || '';
    formReg.age.value = p.age || '';
    formReg.password.value = ''; // pedir que vuelva a escribir
    show(viewRegister);
  });

  // Borrar perfil
  deleteProfile.addEventListener('click', ()=>{
    if(!confirm('¿Borrar perfil local? Esta acción elimina el perfil guardado (demo).')) return;
    localStorage.removeItem('userProfile');
    // opcional: también vaciar historial
    // localStorage.removeItem('userProfileHistory');
    renderProfile();
    alert('Perfil borrado.');
  });

  // Utilidades
  function validateEmail(email){
    // regex simple para demo
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }
  function escapeHtml(s){
    return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
  }

  // Inicializar vista
  (function init(){
    renderProfile();
    show(viewHome);
  })();
</script>

</body>
</html>
