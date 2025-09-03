<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>CulturaGo — Difunde tu patrimonio</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin="" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <style>
    :root{--bg:#fffaf6;--card:#fff;--muted:#6b6b6b;--accent:#4a7c59;--accent-2:#b96b3a;--radius:12px;--shadow:0 6px 18px rgba(20,20,20,0.08);}
    [data-theme="dark"]{--bg:#0b0f0b;--card:#0f1510;--muted:#cbd5c1;--accent:#8ccaa0;--accent-2:#e1a07a;}
    body{margin:0;font-family:Inter,sans-serif;background:var(--bg);color:var(--muted);}
    header, .card{background:var(--card);box-shadow:var(--shadow);border-radius:var(--radius);padding:12px;margin:12px;}
    #map{height:420px;border-radius:12px;width:100%;}
    .bottom-nav{position:fixed;left:0;right:0;bottom:0;height:66px;background:var(--card);display:flex;justify-content:space-around;align-items:center;border-top:1px solid rgba(0,0,0,0.05);}
    .tab button{background:none;border:none;font:inherit;color:var(--muted);cursor:pointer;}
    .active{color:var(--accent);font-weight:700;}
  </style>
</head>
<body>

<header>
  <h1>CulturaGo</h1>
</header>

<main>
  <section id="content"></section>
</main>

<div class="bottom-nav">
  <div class="tab"><button onclick="navigate('history')" id="btnHistory">Historia</button></div>
  <div class="tab"><button onclick="navigate('culture')" id="btnCulture">Cultura</button></div>
  <div class="tab"><button onclick="navigate('nature')" id="btnNature">Naturaleza</button></div>
  <div class="tab"><button onclick="navigate('map')" id="btnMap">Mapa</button></div>
</div>

<!-- Templates ocultos -->
<template id="historyTpl"><div class="card"><h2>Historia</h2><p>Contenido histórico aquí</p></div></template>
<template id="cultureTpl"><div class="card"><h2>Cultura</h2><p>Contenido cultural aquí</p></div></template>
<template id="natureTpl"><div class="card"><h2>Naturaleza</h2><p>Contenido de naturaleza aquí</p></div></template>
<template id="mapTpl"><div class="card"><h2>Mapa de referencia</h2><div id="map"></div></div></template>

<script>
  const state={current:'history'};
  const content=document.getElementById('content');

  const SITES=[
    {title:'Ex-Hacienda "Xochimancas"', desc:'Hacienda del siglo 16', coords:[18.78956468725144, -99.11555593515847]},
    {title:'Santo Domingo de Guzman, Ticumán, Mor.', desc:'Iglesia del siglo 16', coords:[18.76245673610714, -99.11886424684955]},
    {title:'Ojo De Agua"', desc:'Nacimiento de agua por temporada', coords:[18.761822167253584, -99.1228016246767]},
    {title:'El Jagüey', desc:'Presa', coords:[18.797554715797645, -99.10841172028054]},
    {title:'Acueducto "Los Arcos"', desc:'acueductos del siglo 16', coords:[18.804117879232237, -99.10153707196811]},
  ];

  function navigate(screen){
    state.current=screen;
    setActiveTab(screen);
    renderScreen(screen);
  }

  function setActiveTab(name){
    document.querySelectorAll('.tab button').forEach(b=>b.classList.remove('active'));
    const btn=document.getElementById('btn'+name.charAt(0).toUpperCase()+name.slice(1));
    if(btn) btn.classList.add('active');
  }

  function renderScreen(screen){
    content.innerHTML='';
    if(screen==='history'){content.appendChild(document.getElementById('historyTpl').content.cloneNode(true));}
    else if(screen==='culture'){content.appendChild(document.getElementById('cultureTpl').content.cloneNode(true));}
    else if(screen==='nature'){content.appendChild(document.getElementById('natureTpl').content.cloneNode(true));}
    else if(screen==='map'){renderMap();}
  }

  let mapObj=null;
  function renderMap(){
    content.appendChild(document.getElementById('mapTpl').content.cloneNode(true));
    const mapEl=document.getElementById('map');
    if(mapObj){mapObj.invalidateSize(); return;}
    mapObj=L.map(mapEl).setView([19.515,-99.115],13);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{attribution:'© OpenStreetMap contributors'}).addTo(mapObj);
    
    SITES.forEach(s=>{
      const link=`https://www.google.com/maps/search/?api=1&query=${s.coords[0]},${s.coords[1]}`;
      L.marker(s.coords).addTo(mapObj)
        .bindPopup(`<strong>${s.title}</strong><br>${s.desc}<br><a href="${link}" target="_blank">Ir aquí</a>`);
    });
  }

  navigate('history');
</script>

</body>
</html>
