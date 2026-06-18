<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Vigilance Météo France Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0f1117;--surf:#1a1d27;--surf2:#222536;
  --b1:rgba(255,255,255,.08);--b2:rgba(255,255,255,.14);
  --t1:#e8eaf0;--t2:#8b90a4;--t3:#555a6e;
  --acc:#4d7cfe;--r:12px;--rs:8px;
}
body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--t1);min-height:100vh}
.page{display:flex;flex-direction:column;gap:12px;padding:12px;max-width:1400px;margin:0 auto}

/* Header */
.app-hdr{display:flex;align-items:center;justify-content:space-between;padding:10px 16px;background:var(--surf);border:1px solid var(--b1);border-radius:var(--r)}
.app-title{font-size:14px;font-weight:600;display:flex;align-items:center;gap:8px}
.dot{width:8px;height:8px;border-radius:50%;background:var(--acc);box-shadow:0 0 8px rgba(77,124,254,.6)}
.day-tabs{display:flex;gap:4px}
.dtab{padding:5px 14px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.dtab:hover{background:var(--surf2);color:var(--t1)}
.dtab.on{background:var(--acc);color:#fff;border-color:var(--acc)}

/* Layout */
.top{display:flex;gap:12px;align-items:flex-start}
#map-col, #map-prob-col, #map-reg-col{flex:1;min-width:0;position:relative;height:624px;background:#131823;border-radius:12px;border:1px solid var(--b1);overflow:hidden}
.sb{width:220px;flex-shrink:0;display:flex;flex-direction:column;gap:8px}
.card{background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:12px}
.stitle{font-size:10px;font-weight:600;text-transform:uppercase;letter-spacing:.08em;color:var(--t3);margin-bottom:8px}

/* Level rows */
.lvl-row-wrap{display:flex;align-items:center;gap:3px;margin-bottom:2px}
.lvl-btn{display:flex;align-items:center;gap:7px;flex:1;padding:5px 7px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.lvl-btn:hover{background:var(--surf2);color:var(--t1)}
.lvl-btn.on{border-color:var(--b2);background:var(--surf2);color:var(--t1)}
.lvl-dot{width:12px;height:12px;border-radius:3px;flex-shrink:0}
.lvl-btn.semi{margin-left:5px;border-left:2px dashed var(--b2);font-size:11px;opacity:.85}

/* Vue selector */
.view-tabs{display:flex;gap:4px;align-items:center}
.vtab{padding:5px 13px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.vtab:hover{background:var(--surf2);color:var(--t1)}
.vtab.on{background:#2a1f3d;color:#c084fc;border-color:#7c3aed}
.vtab-sep{width:1px;height:16px;background:var(--b2);margin:0 4px}

/* Sidebar notes */
.prob-sidebar-note{font-size:10px;color:var(--t3);line-height:1.5;padding:8px;background:rgba(124,58,237,.06);border:1px solid rgba(124,58,237,.15);border-radius:8px;margin-bottom:2px}
.prob-level-btn{display:flex;align-items:center;gap:8px;width:100%;padding:6px 8px;margin-bottom:3px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.prob-level-btn:hover{background:var(--surf2);color:var(--t1)}
.prob-level-btn.on{border-color:rgba(124,58,237,.4);background:rgba(124,58,237,.08);color:#c084fc}

/* Picto grid */
.pgrid{display:grid;grid-template-columns:repeat(4,1fr);gap:3px}
.pbtn{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 2px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;transition:all .12s;font-size:9px;color:var(--t3);font-family:inherit}
.pbtn:hover{background:var(--surf2);color:var(--t2)}
.pbtn.on{border-color:var(--acc);background:rgba(77,124,254,.1);color:var(--acc)}
.pbtn svg, .pbtn img{width:22px;height:22px;display:block}

/* Toggles */
.tog-row{display:flex;align-items:center;justify-content:space-between;gap:8px;cursor:pointer;padding:2px 0}
.tog{width:30px;height:16px;background:var(--surf2);border:1px solid var(--b2);border-radius:9px;position:relative;flex-shrink:0;transition:all .2s}
.tog.on{background:var(--acc);border-color:var(--acc)}
.tog-k{position:absolute;top:2px;left:2px;width:10px;height:10px;background:#fff;border-radius:50%;transition:transform .2s;pointer-events:none}
.tog.on .tog-k{transform:translateX(14px)}

/* Buttons */
.act-btn{display:flex;align-items:center;justify-content:center;gap:6px;width:100%;padding:8px;border:1px solid var(--b2);border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);transition:all .12s;font-weight:500}
.act-btn:hover{background:var(--surf2);color:var(--t1)}
.act-btn.primary{background:var(--acc);border-color:var(--acc);color:#fff}
.act-btn.primary:hover{background:#3d6de8}
.rst-btn{display:flex;align-items:center;justify-content:center;gap:5px;width:100%;padding:6px;border:1px solid var(--b1);border-radius:var(--rs);cursor:pointer;background:transparent;font-size:11px;font-family:inherit;color:var(--t3);transition:all .12s}
.rst-btn:hover{background:var(--surf2);color:var(--t2)}
.hint{font-size:10px;color:var(--t3);line-height:1.5;text-align:center}

/* Tooltips & Legends */
.tip{position:absolute;background:var(--surf);border:1px solid var(--b2);border-radius:8px;padding:5px 11px;font-size:11px;color:var(--t1);pointer-events:none;display:none;z-index:10;white-space:nowrap;box-shadow:0 4px 16px rgba(0,0,0,.5)}
.leg{position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid var(--b2);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none;z-index:5}

/* Section Espace Cartes de Risque */
.risk-section{margin-top:20px;background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:16px;display:flex;flex-direction:column;gap:16px}
.gallery-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(180px, 1fr));gap:12px;margin-top:8px}
.gallery-item{background:var(--surf2);border:1px solid var(--b1);border-radius:var(--rs);padding:8px;display:flex;flex-direction:column;gap:6px;position:relative}
.gallery-item img{width:100%;aspect-ratio:1;object-fit:cover;border-radius:4px;background:#131823}
.gallery-item .title{font-size:11px;font-weight:500;color:var(--t1);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.upload-btn-wrapper{position:relative;overflow:hidden;display:inline-block}
.upload-btn-wrapper input[type=file]{font-size:100px;position:absolute;left:0;top:0;opacity:0;cursor:pointer}
</style>
</head>
<body>
<div class="page">

  <div class="app-hdr">
    <div class="app-title"><span class="dot"></span>Système Vigilance & Risques Météo</div>
    <div style="display:flex;align-items:center;gap:12px">
      <div class="view-tabs">
        <button class="vtab on" id="vtab-vigil" onclick="switchView('vigilance')">Vigilance</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-prob" onclick="switchView('probable')">↗ Évolution probable</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-reg" onclick="switchView('regions')">🗺️ Anciennes Régions</button>
      </div>
      <div class="day-tabs" id="day-tabs-wrap">
        <button class="dtab on" id="dtab-today" onclick="switchDay('today')">Aujourd'hui</button>
        <button class="dtab" id="dtab-tomorrow" onclick="switchDay('tomorrow')">Demain</button>
      </div>
    </div>
  </div>

  <div class="top" id="view-vigilance">
    <div id="map-col">
      <div id="map" style="width:100%;height:100%"></div>
      <div class="tip" id="tip"></div>
      <div class="leg" id="leg"></div>
    </div>
    <div class="sb">
      <div class="card">
        <p class="stitle">Niveau d'alerte</p>
        <div id="lvl-container"></div>
      </div>
      <div class="card">
        <p class="stitle">Phénomène</p>
        <div class="pgrid" id="pgrid"></div>
      </div>
      <button class="rst-btn" onclick="resetAll()">↺ Réinitialiser</button>
      <button class="act-btn primary" onclick="exportPNG('map')">↓ Télécharger PNG</button>
    </div>
  </div>

  <div class="top" id="view-probable" style="display:none">
    <div id="map-prob-col">
      <div id="map-prob" style="width:100%;height:100%"></div>
      <div class="tip" id="tip-prob"></div>
      <div class="leg" id="leg-prob"></div>
    </div>
    <div class="sb">
      <div class="card" style="border-color:rgba(124,58,237,.2)">
        <p class="stitle" style="color:#c084fc">Niveau cible probable</p>
        <p class="prob-sidebar-note">Départements susceptibles de passer en vigilance supérieure.</p>
        <div id="prob-lvl-container" style="margin-top:8px"></div>
      </div>
      <div class="card" style="border-color:rgba(124,58,237,.2)">
        <p class="stitle" style="color:#c084fc">Phénomène associé</p>
        <div class="pgrid" id="prob-pgrid"></div>
      </div>
      <button class="rst-btn" onclick="resetProb()">↺ Réinitialiser</button>
      <button class="act-btn primary" style="background:#7c3aed;border-color:#7c3aed" onclick="exportPNG('map-prob')">↓ Télécharger PNG</button>
    </div>
  </div>

  <div class="top" id="view-regions" style="display:none">
    <div id="map-reg-col">
      <div id="map-reg" style="width:100%;height:100%"></div>
      <div class="tip" id="tip-reg"></div>
      <div class="leg" id="leg-reg"></div>
    </div>
    <div class="sb">
      <div class="card" style="border-color:var(--acc)">
        <p class="stitle" style="color:var(--acc)">Intensité du Risque</p>
        <div id="reg-risk-container"></div>
      </div>
      <div class="card" style="border-color:var(--acc)">
        <p class="stitle" style="color:var(--acc)">Pictos Côtiers (Multiples)</p>
        <div class="pgrid" id="reg-pgrid"></div>
      </div>
      <button class="rst-btn" onclick="resetRegions()">↺ Réinitialiser</button>
      <button class="act-btn primary" onclick="exportPNG('map-reg')">↓ Télécharger PNG</button>
    </div>
  </div>

  <div class="risk-section">
    <div>
      <h3 style="font-size:14px;font-weight:600;margin-bottom:4px">📁 Espace « Cartes de Risque »</h3>
      <p style="font-size:11px;color:var(--t2)">Uploadez vos propres modèles de cartes ou consultez votre historique généré.</p>
    </div>
    <div style="display:flex;gap:10px;align-items:center">
      <div class="upload-btn-wrapper">
        <button class="act-btn" style="color:var(--acc);border-color:var(--acc)">📤 Importer une carte locale</button>
        <input type="file" id="risk-uploader" accept="image/*" onchange="handleMapUpload(event)">
      </div>
      <button class="act-btn" style="width:auto" onclick="snapshotToGallery()">📸 Enregistrer la configuration actuelle en galerie</button>
    </div>
    <div>
      <p class="stitle">Galerie de Cartes</p>
      <div class="gallery-grid" id="gallery-grid"></div>
    </div>
  </div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js"></script>
<script>
const PICTO_FALLBACK = {
  vent: 'https://i.imgur.com/w9U4uxl.png', orage: 'https://i.imgur.com/TqMfDUO.png',
  pluies: 'https://i.imgur.com/doeHp1Y.png', inondation: 'https://i.imgur.com/b6bctj5.png',
  neige: 'https://i.imgur.com/g0IsPe2.png', verglas: 'https://i.imgur.com/EJKnm3Q.png',
  froid: 'https://i.imgur.com/nCPwfFD.png', chaleur: 'https://i.imgur.com/EGeV2X0.png',
  vagues: 'https://i.imgur.com/u5zMvyy.png', avalanches: 'https://i.imgur.com/KZQXNzU.png'
};

const LEVELS = {
  vert: { color: '#31AA35', label: 'Vert' },
  jaune: { color: '#FFF600', label: 'Jaune' },
  orange: { color: '#FFB82B', label: 'Orange' },
  rouge: { color: '#CC0000', label: 'Rouge' }
};

// Échelle séquentielle foncée pour les anciennes régions
const RISK_SCALES = [
  { k: 'none', color: '#1e2535', label: 'Aucun Risque' },
  { k: 'faible', color: '#4a6fa5', label: 'Risque Faible' },
  { k: 'modere', color: '#3b5998', label: 'Risque Modéré' },
  { k: 'eleve', color: '#193473', label: 'Risque Élevé' },
  { k: 'critique', color: '#0b1d4f', label: 'Risque Critique' }
];

// Mapping des départements vers les anciennes régions françaises
const DEPT_TO_OLD_REG = {
  "Nord": "Nord-Pas-de-Calais", "Pas-de-Calais": "Nord-Pas-de-Calais",
  "Somme": "Picardie", "Aisne": "Picardie", "Oise": "Picardie",
  "Ardennes": "Champagne-Ardenne", "Marne": "Champagne-Ardenne", "Aube": "Champagne-Ardenne", "Haute-Marne": "Champagne-Ardenne",
  "Meuse": "Lorraine", "Meurthe-et-Moselle": "Lorraine", "Moselle": "Lorraine", "Vosges": "Lorraine",
  "Bas-Rhin": "Alsace", "Haut-Rhin": "Alsace",
  "Territoire de Belfort": "Franche-Comté", "Haute-Saône": "Franche-Comté", "Doubs": "Franche-Comté", "Jura": "Franche-Comté",
  "Côte-d'Or": "Bourgogne", "Nièvre": "Bourgogne", "Saône-et-Loire": "Bourgogne", "Yonne": "Bourgogne",
  "Seine-Maritime": "Haute-Normandie", "Eure": "Haute-Normandie",
  "Calvados": "Basse-Normandie", "Manche": "Basse-Normandie", "Orne": "Basse-Normandie",
  "Finistère": "Bretagne", "Côtes-d'Armor": "Bretagne", "Ille-et-Vilaine": "Bretagne", "Morbihan": "Bretagne",
  "Loire-Atlantique": "Pays de la Loire", "Vendée": "Pays de la Loire", "Maine-et-Loire": "Pays de la Loire", "Mayenne": "Pays de la Loire", "Sarthe": "Pays de la Loire",
  "Charente-Maritime": "Poitou-Charentes", "Charente": "Poitou-Charentes", "Deux-Sèvres": "Poitou-Charentes", "Vienne": "Poitou-Charentes",
  "Gironde": "Aquitaine", "Dordogne": "Aquitaine", "Landes": "Aquitaine", "Lot-et-Garonne": "Aquitaine", "Pyrénées-Atlantiques": "Aquitaine",
  "Hérault": "Languedoc-Roussillon", "Gard": "Languedoc-Roussillon", "Pyrénées-Orientales": "Languedoc-Roussillon", "Aude": "Languedoc-Roussillon", "Lozère": "Languedoc-Roussillon",
  "Bouches-du-Rhône": "Provence-Alpes-Côte d'Azur", "Var": "Provence-Alpes-Côte d'Azur", "Alpes-Maritimes": "Provence-Alpes-Côte d'Azur", "Vaucluse": "Provence-Alpes-Côte d'Azur", "Alpes-de-Haute-Provence": "Provence-Alpes-Côte d'Azur", "Hautes-Alpes": "Provence-Alpes-Côte d'Azur",
  "Corse-du-Sud": "Corse", "Haute-Corse": "Corse"
};

const COASTAL_REGIONS = new Set(["Nord-Pas-de-Calais", "Picardie", "Haute-Normandie", "Basse-Normandie", "Bretagne", "Pays de la Loire", "Poitou-Charentes", "Aquitaine", "Languedoc-Roussillon", "Provence-Alpes-Côte d'Azur", "Corse"]);

let features = [];
let activeView = 'vigilance';
let activeDay = 'today';

let activeLevel = 'vert';
let activePicto = null;
let deptData = { today: {}, tomorrow: {} };

let probActiveLevel = 'orange';
let probActivePicto = null;
let probData = {};

let regActiveRisk = 'none';
let regActivePictos = []; 
let regData = {}; // { regionName: { risk, pictos: [] } }

function ico(name) { return `<img src="${PICTO_FALLBACK[name]}" alt="${name}">`; }

function switchView(view) {
  activeView = view;
  d3.select('#view-vigilance').style('display', view === 'vigilance' ? 'flex' : 'none');
  d3.select('#view-probable').style('display', view === 'probable' ? 'flex' : 'none');
  d3.select('#view-regions').style('display', view === 'regions' ? 'flex' : 'none');
  d3.select('#day-tabs-wrap').style('display', view === 'vigilance' ? 'flex' : 'none');
  
  d3.selectAll('.vtab').classList?.remove('on');
  d3.select(`#vtab-${view === 'regions' ? 'reg' : view === 'probable' ? 'prob' : 'vigil'}`).node().classList.add('on');
}

function switchDay(day) {
  activeDay = day;
  d3.selectAll('.day-tabs .dtab').each(function() { this.classList.toggle('on', this.id === `dtab-${day}`); });
  renderMapVigilance();
}

// CONFIGURATION DES SIDEBARS
function initSidebars() {
  // Vue 1 : Vigilance
  const lvlC = d3.select('#lvl-container');
  Object.keys(LEVELS).forEach(k => {
    lvlC.append('button').className = 'lvl-btn' + (k==='vert'?' on':'');
    lvlC.select('button:last-child').html(`<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${LEVELS[k].label}`)
      .on('click', function() {
        activeLevel = k;
        lvlC.selectAll('.lvl-btn').each(function() { this.classList.toggle('on', this.textContent.trim() === LEVELS[k].label); });
      });
  });
  buildGrid('pgrid', k => {
    activePicto = activePicto === k ? null : k;
    d3.selectAll('#pgrid .pbtn').each(function() { this.classList.toggle('on', this.dataset.ik === activePicto); });
  });

  // Vue 2 : Probable
  const probLvlC = d3.select('#prob-lvl-container');
  ['jaune','orange','rouge'].forEach(k => {
    probLvlC.append('button').className = 'prob-level-btn' + (k==='orange'?' on':'');
    probLvlC.select('button:last-child').html(`<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${LEVELS[k].label}`)
      .on('click', function() {
        probActiveLevel = k;
        probLvlC.selectAll('.prob-level-btn').each(function() { this.classList.toggle('on', this.textContent.trim() === LEVELS[k].label); });
      });
  });
  buildGrid('prob-pgrid', k => {
    probActivePicto = probActivePicto === k ? null : k;
    d3.selectAll('#prob-pgrid .pbtn').each(function() { this.classList.toggle('on', this.dataset.ik === probActivePicto); });
  });

  // Vue 3 : Régions
  const regRiskC = d3.select('#reg-risk-container');
  RISK_SCALES.forEach(s => {
    regRiskC.append('button').className = 'prob-level-btn' + (s.k==='none'?' on':'');
    regRiskC.select('button:last-child').html(`<span class="lvl-dot" style="background:${s.color}"></span>${s.label}`)
      .on('click', function() {
        regActiveRisk = s.k;
        regRiskC.selectAll('.prob-level-btn').each(function() { this.classList.toggle('on', this.textContent.trim() === s.label); });
      });
  });
  buildGrid('reg-pgrid', k => {
    const idx = regActivePictos.indexOf(k);
    if(idx > -1) regActivePictos.splice(idx, 1);
    else regActivePictos.push(k);
    d3.selectAll('#reg-pgrid .pbtn').each(function() { this.classList.toggle('on', regActivePictos.includes(this.dataset.ik)); });
  });
}

function buildGrid(id, onClick) {
  const g = d3.select('#' + id);
  Object.keys(PICTO_FALLBACK).forEach(k => {
    g.append('button').className = 'pbtn').attr('data-ik', k).html(ico(k) + `<span>${k.slice(0,5)}</span>`).on('click', () => onClick(k));
  });
}

// RENDUS DES CARTES
function renderMapVigilance() {
  const data = deptData[activeDay];
  d3.selectAll('#map path.dept').attr('fill', d => {
    const nm = d.properties.name || d.properties.NAME;
    return data[nm] ? LEVELS[data[nm].level].color : '#1e2535';
  });
  
  d3.selectAll('#map .picto-group').remove();
  const g = d3.select('#map svg g');
  Object.entries(data).forEach(([nm, d]) => {
    if(!d.picto) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === nm);
    if(!feat) return;
    const b = d3.geoPath().projection(proj).bounds(feat);
    g.append('image').attr('class', 'picto-group')
      .attr('href', PICTO_FALLBACK[d.picto])
      .attr('x', (b[0][0]+b[1][0])/2 - 10).attr('y', (b[0][1]+b[1][1])/2 - 10)
      .attr('width', 20).attr('height', 20);
  });
}

function renderMapProbable() {
  d3.selectAll('#map-prob path.dept-p').attr('fill', d => {
    const nm = d.properties.name || d.properties.NAME;
    return probData[nm] ? LEVELS[probData[nm].level].color : '#1e2535';
  });

  d3.selectAll('#map-prob .picto-group').remove();
  const g = d3.select('#map-prob svg g');
  Object.entries(probData).forEach(([nm, d]) => {
    if(!d.picto) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === nm);
    const b = d3.geoPath().projection(proj).bounds(feat);
    g.append('image').attr('class', 'picto-group')
      .attr('href', PICTO_FALLBACK[d.picto])
      .attr('x', (b[0][0]+b[1][0])/2 - 10).attr('y', (b[0][1]+b[1][1])/2 - 10)
      .attr('width', 20).attr('height', 20);
  });
}

function renderMapRegions() {
  d3.selectAll('#map-reg path.dept-r').attr('fill', d => {
    const nm = d.properties.name || d.properties.NAME;
    const reg = DEPT_TO_OLD_REG[nm] || "Autre";
    const rData = regData[reg];
    if(rData) {
      return RISK_SCALES.find(s => s.k === rData.risk).color;
    }
    return '#1e2535';
  });

  d3.selectAll('#map-reg .picto-group').remove();
  const g = d3.select('#map-reg svg g');

  // Trouver des centres grossiers par ancienne région pour l'affichage multiple
  const centers = {};
  features.forEach(f => {
    const nm = f.properties.name || f.properties.NAME;
    const reg = DEPT_TO_OLD_REG[nm];
    if(reg && COASTAL_REGIONS.has(reg)) {
      const b = d3.geoPath().projection(proj).bounds(f);
      centers[reg] = [(b[0][0]+b[1][0])/2, (b[0][1]+b[1][1])/2];
    }
  });

  Object.entries(regData).forEach(([reg, d]) => {
    if(!d.pictos || d.pictos.length === 0 || !centers[reg]) return;
    const [cx, cy] = centers[reg];
    d.pictos.forEach((p, idx) => {
      g.append('image').attr('class', 'picto-group')
        .attr('href', PICTO_FALLBACK[p])
        .attr('x', cx - 10 + (idx * 16) - ((d.pictos.length*16)/2))
        .attr('y', cy - 10)
        .attr('width', 20).attr('height', 20);
    });
  });
}

// RESET FONCTIONS
function resetAll() { deptData[activeDay] = {}; renderMapVigilance(); }
function resetProb() { probData = {}; renderMapProbable(); }
function resetRegions() { regData = {}; renderMapRegions(); }

// EXPORTATION PNG (Canvas conversion de qualité)
function exportPNG(containerId) {
  const svgEl = document.querySelector(`#${containerId} svg`);
  if(!svgEl) return;
  const svgString = new XMLSerializer().serializeToString(svgEl);
  const svgBlob = new Blob([svgString], {type: "image/svg+xml;charset=utf-8"});
  const URL = window.URL || window.webkitURL || window;
  const blobURL = URL.createObjectURL(svgBlob);
  
  const image = new Image();
  image.onload = () => {
    const canvas = document.createElement('canvas');
    canvas.width = 600;
    canvas.height = 624;
    const context = canvas.getContext('2d');
    context.fillStyle = '#131823';
    context.fillRect(0, 0, canvas.width, canvas.height);
    context.drawImage(image, 0, 0, 600, 624);
    
    const png = canvas.toDataURL('image/png');
    const downloadLink = document.createElement('a');
    downloadLink.href = png;
    downloadLink.download = `${containerId}-${Date.now()}.png`;
    document.body.appendChild(downloadLink);
    downloadLink.click();
    document.body.removeChild(downloadLink);
  };
  image.src = blobURL;
}

// MANAGEMENT DE LA GALERIE DE CARTES
function handleMapUpload(event) {
  const file = event.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = function(e) {
    addCardToGallery(e.target.result, "Carte Importée");
  };
  reader.readAsDataURL(file);
}

function snapshotToGallery() {
  const svgEl = document.querySelector(`#view-${activeView} svg`);
  if(!svgEl) return;
  const svgString = new XMLSerializer().serializeToString(svgEl);
  const svgBlob = new Blob([svgString], {type: "image/svg+xml;charset=utf-8"});
  const blobURL = URL.createObjectURL(svgBlob);
  
  const image = new Image();
  image.onload = () => {
    const canvas = document.createElement('canvas');
    canvas.width = 300; canvas.height = 312;
    const context = canvas.getContext('2d');
    context.drawImage(image, 0, 0, 300, 312);
    addCardToGallery(canvas.toDataURL('image/png'), `Saisie ${activeView}`);
  };
  image.src = blobURL;
}

function addCardToGallery(src, title) {
  const grid = d3.select('#gallery-grid');
  const item = grid.append('div').className('gallery-item');
  item.html(`
    <img src="${src}">
    <div class="title">${title}</div>
    <button class="rst-btn" style="padding:2px;margin-top:2px;font-size:9px" onclick="this.parentElement.remove()">Supprimer</button>
  `);
}

let proj;
// INITIALISATION D3 & TOPOJSON
(async () => {
  const topo = await d3.json('https://cdn.jsdelivr.net/npm/datamaps@0.5.10/src/js/data/fra.topo.json');
  const key = Object.keys(topo.objects)[0];
  features = topojson.feature(topo, topo.objects[key]).features;
  
  const W = 600, H = 624;
  proj = d3.geoConicConformal().parallels([44,49]).rotate([-3,0]).center([0,46.5]).scale(1860).translate([300,312]);
  const path = d3.geoPath().projection(proj);

  // 1. Map Vigilance
  const svg1 = d3.select('#map').append('svg').attr('viewBox',`0 0 ${W} ${H}`).style('width','100%').style('height','100%');
  svg1.append('g').selectAll('path').data(features).join('path').className('dept').attr('d', path)
    .attr('fill', '#1e2535').attr('stroke', 'rgba(255,255,255,.14)').style('cursor','pointer')
    .on('click', function(e, d) {
      const nm = d.properties.name || d.properties.NAME;
      const cur = deptData[activeDay][nm];
      if(cur && cur.level === activeLevel && cur.picto === activePicto) delete deptData[activeDay][nm];
      else deptData[activeDay][nm] = { level: activeLevel, picto: activePicto };
      renderMapVigilance();
    });

  // 2. Map Probable (Cadrage fixe & Statique)
  const svg2 = d3.select('#map-prob').append('svg').attr('viewBox',`0 0 ${W} ${H}`).style('width','100%').style('height','100%');
  svg2.append('g').selectAll('path').data(features).join('path').className('dept-p').attr('d', path)
    .attr('fill', '#1e2535').attr('stroke', 'rgba(255,255,255,.14)').style('cursor','pointer')
    .on('click', function(e, d) {
      const nm = d.properties.name || d.properties.NAME;
      if(probData[nm]) delete probData[nm];
      else probData[nm] = { level: probActiveLevel, picto: probActivePicto };
      renderMapProbable();
    });

  // 3. Map Régions
  const svg3 = d3.select('#map-reg').append('svg').attr('viewBox',`0 0 ${W} ${H}`).style('width','100%').style('height','100%');
  svg3.append('g').selectAll('path').data(features).join('path').className('dept-r').attr('d', path)
    .attr('fill', '#1e2535').attr('stroke', 'rgba(255,255,255,.14)').style('cursor','pointer')
    .on('click', function(e, d) {
      const nm = d.properties.name || d.properties.NAME;
      const reg = DEPT_TO_OLD_REG[nm] || "Autre";
      if(!regData[reg]) regData[reg] = { risk: 'none', pictos: [] };
      
      regData[reg].risk = regActiveRisk;
      if(COASTAL_REGIONS.has(reg) && regActivePictos.length > 0) {
        regData[reg].pictos = [...regActivePictos];
      }
      renderMapRegions();
    });

  initSidebars();
})();
</script>
</body>
</html>
