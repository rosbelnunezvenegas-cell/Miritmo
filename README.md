# Miritmo
Una aplicación para la salud de todos 

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#172c63">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<title>MiRitmo</title>
<style>
:root{--azul:#172c63;--celeste:#3978c6;--fondo:#f5f7fc;--verde:#31866d;--texto:#17213a;--tarjeta:#fff;--linea:#e6e9f2;--suave:#eaf1ff}
@media (prefers-color-scheme:dark){:root{--fondo:#0f1428;--tarjeta:#1a2140;--texto:#eef1fb;--linea:#2c3560;--suave:#222b52;--azul:#4d74d6;--verde:#4fc3a0}}
*{box-sizing:border-box}
html,body{margin:0}
body{font-family:system-ui,-apple-system,Arial,sans-serif;background:var(--fondo);color:var(--texto);padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
main{max-width:480px;margin:auto;padding:12px}
header{text-align:center;padding:8px}
.logo{display:inline-grid;place-items:center;width:48px;height:48px;border-radius:15px;background:var(--azul);color:#fff;font-size:28px}
h1{margin:6px 0 2px;color:var(--azul);font-size:28px}
h2{font-size:17px;margin:0 0 4px}
.sub{font-size:14px;opacity:.8}
.card{background:var(--tarjeta);padding:14px;margin:10px 0;border-radius:16px;box-shadow:0 3px 12px #0000001a}
.calendario{text-align:center;background:var(--suave)}
.fecha{font-size:18px;font-weight:bold;margin:10px 0;text-transform:capitalize}
button{background:var(--azul);color:#fff;border:none;border-radius:10px;padding:10px 12px;margin:3px;font-weight:bold;font-size:14px;cursor:pointer}
button.sec{background:transparent;color:var(--azul);border:2px solid var(--azul)}
button:focus-visible,input:focus-visible{outline:3px solid orange}
.puntos{background:var(--azul);color:#fff;text-align:center}
.numero{font-size:44px;font-weight:bold;line-height:1.1}
.fila2{display:flex;justify-content:space-around;margin-top:8px;font-size:13px}
.fila2 b{display:block;font-size:20px}
.barra{height:10px;border-radius:6px;background:#ffffff33;margin-top:10px;overflow:hidden}
.barra i{display:block;height:100%;background:#fff;width:0;transition:width .3s}
.campo{display:flex;align-items:center;gap:8px;padding:10px 0;border-bottom:1px solid var(--linea)}
.campo:last-child{border-bottom:none}
.campo label{flex:1;font-size:14px}
.campo small{display:block;opacity:.7;font-size:12px}
input[type=time],input[type=number],input[type=text]{font-size:16px;padding:8px;border:1px solid var(--linea);border-radius:8px;background:var(--fondo);color:var(--texto)}
input[type=number]{width:78px}
input[type=time]{width:112px}
.pts{color:var(--verde);font-size:12px;font-weight:bold;min-width:58px;text-align:right}
.tarea{display:flex;align-items:center;gap:9px;padding:12px 0;border-bottom:1px solid var(--linea)}
.tarea input{width:22px;height:22px;accent-color:var(--verde)}
.nombre{flex:1;font-size:14px}
.completada .nombre{text-decoration:line-through;opacity:.55}
.del{background:transparent;color:#c0392b;padding:4px 8px;font-size:16px;margin:0}
.nueva{display:flex;gap:6px;margin-top:12px}
.nueva input[type=text]{flex:1;min-width:0}
.nueva input[type=number]{width:64px}
.semana{display:flex;align-items:flex-end;justify-content:space-between;height:110px;gap:6px;margin-top:10px}
.col{flex:1;text-align:center;font-size:11px}
.col i{display:block;background:var(--celeste);border-radius:6px 6px 0 0;min-height:3px}
.col.hoy i{background:var(--verde)}
footer{text-align:center;font-size:11px;opacity:.6;padding:10px}
</style>
</head>
<body>
<main>

<header>
  <div class="logo">↗</div>
  <h1>MiRitmo</h1>
  <div class="sub">Tu día, a tu ritmo</div>
</header>

<section class="card calendario">
  <strong id="tipoDia">HOY</strong>
  <div class="fecha" id="fecha"></div>
  <button onclick="cambiarDia(-1)">◀ Ayer</button>
  <button onclick="irHoy()">Hoy</button>
  <button onclick="cambiarDia(1)">Mañana ▶</button>
</section>

<section class="card puntos">
  <div>PUNTOS DEL DÍA</div>
  <div class="numero" id="puntos">0</div>
  <div id="progreso"></div>
  <div class="barra"><i id="barra"></i></div>
  <div class="fila2">
    <div><b id="totalPts">0</b>puntos en total</div>
    <div><b id="rachaDias">0</b>días con puntos</div>
  </div>
</section>

<section class="card">
  <h2>🌙 Sueño y levantarse</h2>
  <div class="campo">
    <label for="wake">Hora de levantarse<small>A las 9:00 o antes: +5 pts</small></label>
    <input type="time" id="wake">
    <span class="pts" id="ptsWake"></span>
  </div>
  <div class="campo">
    <label for="sleep">Horas de sueño<small>7 horas o más: +10 pts</small></label>
    <input type="number" id="sleep" min="0" max="24" step="0.5" inputmode="decimal" placeholder="0">
    <span class="pts" id="ptsSleep"></span>
  </div>
</section>

<section class="card">
  <h2>🏃 Ejercicio</h2>
  <div class="campo">
    <label for="ex">Minutos de ejercicio<small>20 minutos o más: +15 pts</small></label>
    <input type="number" id="ex" min="0" max="600" step="5" inputmode="numeric" placeholder="0">
    <span class="pts" id="ptsEx"></span>
  </div>
  <div class="campo">
    <label for="exTipo">¿Qué hiciste?</label>
    <input type="text" id="exTipo" maxlength="40" placeholder="Caminar, correr, bici…" style="flex:1;min-width:0">
  </div>
</section>

<section class="card">
  <h2>✅ Actividades del día</h2>
  <div class="sub">Marca cada actividad al terminarla para ganar puntos.</div>
  <div id="listaTareas"></div>
  <div class="nueva">
    <input type="text" id="nuevaNombre" maxlength="40" placeholder="Nueva actividad" aria-label="Nombre de la nueva actividad">
    <input type="number" id="nuevaPts" min="1" max="100" value="10" inputmode="numeric" aria-label="Puntos">
    <button onclick="agregarTarea()">Agregar</button>
  </div>
</section>

<section class="card">
  <h2>📊 Últimos 7 días</h2>
  <div class="semana" id="semana"></div>
</section>

<footer>MiRitmo · Organiza tus días y alcanza tus metas.</footer>

</main>

<script>
const CLAVE = "miritmo:v2";
const REGLAS = { wakeLimite: "09:00", wakePts: 5, sleepMin: 7, sleepPts: 10, exMin: 20, exPts: 15 };

const TAREAS_INICIALES = [
  { id: "t1", nombre: "Aseo personal", puntos: 10 },
  { id: "t2", nombre: "Tender la cama", puntos: 15 },
  { id: "t3", nombre: "Hacer la tarea", puntos: 15 },
  { id: "t4", nombre: "Ordenar mi cuarto", puntos: 10 },
  { id: "t5", nombre: "Lavar los platos", puntos: 10 },
  { id: "t6", nombre: "Sacar la basura", puntos: 5 },
  { id: "t7", nombre: "Ayudar a preparar comida", puntos: 15 },
  { id: "t8", nombre: "Preparar mi mochila", puntos: 5 }
];

let datos = cargar();

function cargar() {
  try {
    const t = JSON.parse(localStorage.getItem(CLAVE));
    if (t && Array.isArray(t.tareas) && t.dias) return t;
  } catch (e) {}
  return { tareas: TAREAS_INICIALES.slice(), dias: {} };
}

function guardar() {
  try { localStorage.setItem(CLAVE, JSON.stringify(datos)); } catch (e) {}
}

function fechaTexto(f) {
  return [f.getFullYear(), String(f.getMonth() + 1).padStart(2, "0"), String(f.getDate()).padStart(2, "0")].join("-");
}
function fechaLocal() { return fechaTexto(new Date()); }
function aFecha(t) { const [a, m, d] = t.split("-").map(Number); return new Date(a, m - 1, d, 12); }
function desplazar(t, n) { const f = aFecha(t); f.setDate(f.getDate() + n); return fechaTexto(f); }
function formato(t) {
  return new Intl.DateTimeFormat("es-MX", { weekday: "long", day: "numeric", month: "long", year: "numeric" }).format(aFecha(t));
}

let dia = fechaLocal();

function registro(f) {
  if (!datos.dias[f]) datos.dias[f] = { hechas: {}, wake: "", sleep: "", ex: "", exTipo: "" };
  return datos.dias[f];
}

function puntosDe(f) {
  const r = datos.dias[f];
  if (!r) return { total: 0, wake: 0, sleep: 0, ex: 0, tareas: 0, hechas: 0 };
  let tareas = 0, hechas = 0;
  datos.tareas.forEach(t => { if (r.hechas[t.id]) { tareas += t.puntos; hechas++; } });
  const wake = r.wake && r.wake <= REGLAS.wakeLimite ? REGLAS.wakePts : 0;
  const sleep = parseFloat(r.sleep) >= REGLAS.sleepMin ? REGLAS.sleepPts : 0;
  const ex = parseFloat(r.ex) >= REGLAS.exMin ? REGLAS.exPts : 0;
  return { total: tareas + wake + sleep + ex, wake, sleep, ex, tareas, hechas };
}

function maximoDia() {
  return datos.tareas.reduce((s, t) => s + t.puntos, 0) + REGLAS.wakePts + REGLAS.sleepPts + REGLAS.exPts;
}

function cambiarDia(n) { dia = desplazar(dia, n); pintar(); }
function irHoy() { dia = fechaLocal(); pintar(); }

function agregarTarea() {
  const n = document.getElementById("nuevaNombre");
  const p = document.getElementById("nuevaPts");
  const nombre = n.value.trim();
  const puntos = Math.max(1, Math.min(100, parseInt(p.value, 10) || 0));
  if (!nombre) { n.focus(); return; }
  datos.tareas.push({ id: "t" + Date.now(), nombre, puntos });
  n.value = "";
  guardar();
  pintar();
}

function borrarTarea(id) {
  const t = datos.tareas.find(x => x.id === id);
  if (!t || !confirm("¿Quitar \"" + t.nombre + "\" de la lista?")) return;
  datos.tareas = datos.tareas.filter(x => x.id !== id);
  guardar();
  pintar();
}

function pintar() {
  const hoy = fechaLocal();
  document.getElementById("fecha").textContent = formato(dia);
  let tipo = "DÍA SELECCIONADO";
  if (dia === hoy) tipo = "HOY";
  else if (dia === desplazar(hoy, 1)) tipo = "MAÑANA";
  else if (dia === desplazar(hoy, -1)) tipo = "AYER";
  document.getElementById("tipoDia").textContent = tipo;

  const r = datos.dias[dia] || { hechas: {}, wake: "", sleep: "", ex: "", exTipo: "" };
  const wakeEl = document.getElementById("wake");
  const sleepEl = document.getElementById("sleep");
  const exEl = document.getElementById("ex");
  const exTipoEl = document.getElementById("exTipo");
  if (document.activeElement !== wakeEl) wakeEl.value = r.wake || "";
  if (document.activeElement !== sleepEl) sleepEl.value = r.sleep || "";
  if (document.activeElement !== exEl) exEl.value = r.ex || "";
  if (document.activeElement !== exTipoEl) exTipoEl.value = r.exTipo || "";

  const lista = document.getElementById("listaTareas");
  lista.replaceChildren();
  datos.tareas.forEach(t => {
    const hecha = !!r.hechas[t.id];
    const fila = document.createElement("label");
    fila.className = "tarea" + (hecha ? " completada" : "");
    const c = document.createElement("input");
    c.type = "checkbox";
    c.checked = hecha;
    c.setAttribute("aria-label", t.nombre);
    c.addEventListener("change", () => {
      registro(dia).hechas[t.id] = c.checked;
      guardar();
      pintar();
    });
    const nom = document.createElement("span");
    nom.className = "nombre";
    nom.textContent = t.nombre;
    const v = document.createElement("span");
    v.className = "pts";
    v.textContent = "+" + t.puntos + " pts";
    const d = document.createElement("button");
    d.className = "del";
    d.textContent = "✕";
    d.setAttribute("aria-label", "Quitar " + t.nombre);
    d.addEventListener("click", e => { e.preventDefault(); borrarTarea(t.id); });
    fila.append(c, nom, v, d);
    lista.appendChild(fila);
  });

  const p = puntosDe(dia);
  document.getElementById("puntos").textContent = p.total;
  document.getElementById("progreso").textContent = p.hechas + " de " + datos.tareas.length + " actividades completadas";
  document.getElementById("barra").style.width = Math.min(100, Math.round(p.total / maximoDia() * 100)) + "%";
  document.getElementById("ptsWake").textContent = p.wake ? "+" + p.wake + " pts" : "";
  document.getElementById("ptsSleep").textContent = p.sleep ? "+" + p.sleep + " pts" : "";
  document.getElementById("ptsEx").textContent = p.ex ? "+" + p.ex + " pts" : "";

  let total = 0, dias = 0;
  Object.keys(datos.dias).forEach(f => { const x = puntosDe(f).total; total += x; if (x > 0) dias++; });
  document.getElementById("totalPts").textContent = total;
  document.getElementById("rachaDias").textContent = dias;

  const sem = document.getElementById("semana");
  sem.replaceChildren();
  const max = Math.max(maximoDia(), 1);
  for (let i = 6; i >= 0; i--) {
    const f = desplazar(hoy, -i);
    const pts = puntosDe(f).total;
    const col = document.createElement("div");
    col.className = "col" + (i === 0 ? " hoy" : "");
    const barra = document.createElement("i");
    barra.style.height = Math.round(pts / max * 80) + "px";
    const num = document.createElement("div");
    num.textContent = pts;
    const lbl = document.createElement("div");
    lbl.textContent = new Intl.DateTimeFormat("es-MX", { weekday: "short" }).format(aFecha(f)).slice(0, 3);
    col.append(num, barra, lbl);
    sem.appendChild(col);
  }
}

function enlazar(id, campo) {
  const el = document.getElementById(id);
  el.addEventListener("input", () => {
    registro(dia)[campo] = el.value;
    guardar();
    const p = puntosDe(dia);
    document.getElementById("puntos").textContent = p.total;
    document.getElementById("ptsWake").textContent = p.wake ? "+" + p.wake + " pts" : "";
    document.getElementById("ptsSleep").textContent = p.sleep ? "+" + p.sleep + " pts" : "";
    document.getElementById("ptsEx").textContent = p.ex ? "+" + p.ex + " pts" : "";
  });
  el.addEventListener("change", pintar);
}
enlazar("wake", "wake");
enlazar("sleep", "sleep");
enlazar("ex", "ex");
enlazar("exTipo", "exTipo");

document.addEventListener("visibilitychange", () => { if (!document.hidden) pintar(); });
pintar();
</script>
</body>
</html>