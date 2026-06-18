<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Vigilance Météo France</title>
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
#map-col{flex:1;min-width:0;position:relative}
.sb{width:192px;flex-shrink:0;display:flex;flex-direction:column;gap:8px}
.card{background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:12px}
.stitle{font-size:10px;font-weight:600;text-transform:uppercase;letter-spacing:.08em;color:var(--t3);margin-bottom:8px}

/* Level rows */
.lvl-row-wrap{display:flex;align-items:center;gap:3px;margin-bottom:2px}
.lvl-btn{display:flex;align-items:center;gap:7px;flex:1;padding:5px 7px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.lvl-btn:hover{background:var(--surf2);color:var(--t1)}
.lvl-btn.on{border-color:var(--b2);background:var(--surf2);color:var(--t1)}
.lvl-dot{width:12px;height:12px;border-radius:3px;flex-shrink:0}
.lvl-btn.semi{margin-left:5px;border-left:2px dashed var(--b2);font-size:11px;opacity:.85}
/* ── Vue selector (Vigilance / Évolution probable) ── */
.view-tabs{display:flex;gap:4px;align-items:center}
.vtab{padding:5px 13px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.vtab:hover{background:var(--surf2);color:var(--t1)}
.vtab.on{background:#2a1f3d;color:#c084fc;border-color:#7c3aed}
.vtab-sep{width:1px;height:16px;background:var(--b2);margin:0 4px}

/* ── Sidebar probable ── */
.prob-sidebar-note{font-size:10px;color:var(--t3);line-height:1.5;padding:8px;background:rgba(124,58,237,.06);border:1px solid rgba(124,58,237,.15);border-radius:8px;margin-bottom:2px}
.prob-level-btn{display:flex;align-items:center;gap:8px;width:100%;padding:6px 8px;margin-bottom:3px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.prob-level-btn:hover{background:var(--surf2);color:var(--t1)}
.prob-level-btn.on{border-color:rgba(124,58,237,.4);background:rgba(124,58,237,.08);color:#c084fc}

/* ── Carte probable ── */
#map-prob-col{flex:1;min-width:0;position:relative}
#map-prob{width:100%;aspect-ratio:1/1.04;border-radius:12px;overflow:hidden;background:#1a1d27;border:1px solid rgba(124,58,237,.2);position:relative}
#map-prob svg{display:block;width:100%;height:100%}
.prob-legend-item{display:flex;align-items:center;gap:7px;padding:3px 0}
.prob-legend-dot{width:14px;height:14px;border-radius:3px;flex-shrink:0}

/* ── Supprime prob-btn de l'ancienne sidebar ── */
.prob-btn{display:none!important}

/* Picto grid */
.pgrid{display:grid;grid-template-columns:repeat(4,1fr);gap:3px}
.pbtn{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 2px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;transition:all .12s;font-size:9px;color:var(--t3);font-family:inherit}
.pbtn:hover{background:var(--surf2);color:var(--t2)}
.pbtn.on{border-color:var(--acc);background:rgba(77,124,254,.1);color:var(--acc)}
.pbtn svg{width:22px;height:22px;display:block}

/* Toggles */
.tog-row{display:flex;align-items:center;justify-content:space-between;gap:8px;cursor:pointer;padding:2px 0}
.tog{width:30px;height:16px;background:var(--surf2);border:1px solid var(--b2);border-radius:9px;position:relative;flex-shrink:0;transition:all .2s}
.tog.on{background:var(--acc);border-color:var(--acc)}
.tog-k{position:absolute;top:2px;left:2px;width:10px;height:10px;background:#fff;border-radius:50%;transition:transform .2s;pointer-events:none}
.tog.on .tog-k{transform:translateX(14px)}
.sub-lvls{display:flex;gap:3px;flex-wrap:wrap;margin-top:7px}
.slvl{flex:1;min-width:0;padding:4px 2px;border:1px solid var(--b1);border-radius:5px;cursor:pointer;background:transparent;font-size:10px;font-family:inherit;color:var(--t3);text-align:center;transition:all .12s;white-space:nowrap}
.slvl:hover{background:var(--surf2);color:var(--t2)}
.slvl.on{border-color:var(--b2);background:var(--surf2);color:var(--t1)}
.dv-tog{display:flex;align-items:center;justify-content:space-between;padding:4px 2px;border-radius:var(--rs);cursor:pointer;background:transparent;border:none;width:100%;transition:all .12s;font-family:inherit}
.dv-tog:hover{background:var(--surf2)}

/* Buttons */
.act-btn{display:flex;align-items:center;justify-content:center;gap:6px;width:100%;padding:8px;border:1px solid var(--b2);border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);transition:all .12s;font-weight:500}
.act-btn:hover{background:var(--surf2);color:var(--t1)}
.act-btn.primary{background:var(--acc);border-color:var(--acc);color:#fff}
.act-btn.primary:hover{background:#3d6de8}
.rst-btn{display:flex;align-items:center;justify-content:center;gap:5px;width:100%;padding:6px;border:1px solid var(--b1);border-radius:var(--rs);cursor:pointer;background:transparent;font-size:11px;font-family:inherit;color:var(--t3);transition:all .12s}
.rst-btn:hover{background:var(--surf2);color:var(--t2)}
.hint{font-size:10px;color:var(--t3);line-height:1.5;text-align:center}

/* Tooltip */
.tip{position:absolute;background:var(--surf);border:1px solid var(--b2);border-radius:8px;padding:5px 11px;font-size:11px;color:var(--t1);pointer-events:none;display:none;z-index:10;white-space:nowrap;box-shadow:0 4px 16px rgba(0,0,0,.5)}

/* Légende */
#leg{position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid var(--b2);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none}
.leg-sec{margin-bottom:7px}
.leg-sec:last-child{margin-bottom:0}
.leg-sec-t{font-size:9px;font-weight:600;text-transform:uppercase;letter-spacing:.07em;color:var(--t3);margin-bottom:5px}
.leg-r{display:flex;align-items:center;gap:7px;margin-bottom:3px}
.leg-dot{width:13px;height:13px;border-radius:3px;flex-shrink:0}
.leg-lbl{font-size:11px;color:var(--t2)}

/* Jauges */
#gauges{display:flex;flex-direction:column;gap:8px}
.g-wrap{background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:12px 16px;display:flex;flex-direction:column;gap:8px}
.g-wrap.dept{border-left:3px solid var(--acc)}
.g-hdr{display:flex;align-items:center;justify-content:space-between}
.g-title{font-size:12px;font-weight:600;display:flex;align-items:center;gap:7px}
.g-badge{font-size:9px;padding:2px 7px;border-radius:20px;background:rgba(77,124,254,.15);color:var(--acc);border:1px solid rgba(77,124,254,.3)}
.g-back{display:flex;align-items:center;gap:5px;padding:4px 10px;border:1px solid var(--b2);border-radius:20px;background:transparent;cursor:pointer;font-size:11px;font-family:inherit;color:var(--t2);transition:all .15s}
.g-back:hover{background:var(--surf2);color:var(--t1)}
.g-rows{display:flex;flex-direction:column;gap:5px}
.g-row{display:flex;align-items:center;gap:10px}
.g-picto{width:26px;height:26px;cursor:pointer;flex-shrink:0;border-radius:5px;border:1px dashed var(--b2);display:flex;align-items:center;justify-content:center;transition:all .12s;color:var(--t2)}
.g-picto:hover{border-color:var(--acc);background:rgba(77,124,254,.08)}
.g-picto svg{width:18px;height:18px;display:block}
.g-lbl{width:72px;font-size:11px;color:var(--t2);flex-shrink:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;font-weight:500}
.g-tl{flex:1;position:relative;height:22px;user-select:none;cursor:crosshair}
.g-bg{width:100%;height:100%;background:rgba(255,255,255,.06);border-radius:5px;position:absolute;top:0;left:0}
.g-tl svg{position:absolute;top:0;left:0;width:100%;height:100%;overflow:visible;pointer-events:none}
.g-del{border:none;background:transparent;cursor:pointer;color:var(--t3);font-size:15px;padding:0 2px;line-height:1;transition:color .12s;font-family:inherit}
.g-del:hover{color:#ff5f5f}
.g-ticks{display:flex;margin-top:1px;padding-left:112px}
.g-tick{font-size:9px;color:var(--t3);font-family:'JetBrains Mono',monospace;flex:1;text-align:center}
.g-tick:first-child{text-align:left}.g-tick:last-child{text-align:right}
.g-add{display:inline-flex;align-items:center;gap:5px;padding:4px 12px;border:1px dashed var(--b2);border-radius:20px;cursor:pointer;background:transparent;font-size:11px;font-family:inherit;color:var(--t3);transition:all .12s}
.g-add:hover{background:var(--surf2);color:var(--t2)}

/* Popups */
.pop{position:fixed;background:var(--surf);border:1px solid var(--b2);border-radius:10px;box-shadow:0 8px 32px rgba(0,0,0,.7);z-index:9999;display:none}
.pop.open{display:flex}
#lvl-pop{flex-direction:column;gap:1px;padding:6px;min-width:140px}
.lp-item{display:flex;align-items:center;gap:8px;padding:6px 10px;border-radius:6px;cursor:pointer;font-size:12px;color:var(--t2)}
.lp-item:hover{background:var(--surf2);color:var(--t1)}
.lp-dot{width:12px;height:12px;border-radius:3px;flex-shrink:0}
#phen-pop{flex-wrap:wrap;gap:3px;padding:8px;width:220px}
.pp-item{display:flex;flex-direction:column;align-items:center;gap:2px;padding:5px;border-radius:6px;cursor:pointer;font-size:9px;color:var(--t3);transition:all .12s}
.pp-item:hover{background:var(--surf2);color:var(--t2)}
.pp-item svg{width:20px;height:20px;display:block}
</style>
</head>
<body>
<div class="page">

  <div class="app-hdr">
    <div class="app-title"><span class="dot"></span>Carte Vigilance Météo France</div>
    <div style="display:flex;align-items:center;gap:12px">
      <div class="view-tabs">
        <button class="vtab on" id="vtab-vigil" onclick="switchView('vigilance')">Vigilance</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-prob" onclick="switchView('probable')" style="color:#7c6a9a;border-color:rgba(124,58,237,.3)">↗ Évolution probable</button>
      </div>
      <div class="day-tabs" id="day-tabs-wrap">
        <button class="dtab on" id="dtab-today" onclick="switchDay('today')">Aujourd'hui</button>
        <button class="dtab" id="dtab-tomorrow" onclick="switchDay('tomorrow')">Demain</button>
      </div>
    </div>
  </div>

  <div class="top" id="view-vigilance">
    <div id="map-col">
      <div id="map" style="width:100%;border-radius:12px;overflow:hidden;background:#1a1d27;border:1px solid rgba(255,255,255,.06)"></div>
      <div class="tip" id="tip"></div>
      <div id="leg"></div>
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
      <div class="card">
        <p class="stitle" style="margin-bottom:6px">Submersion marine</p>
        <div class="tog-row" onclick="toggleSub()">
          <span style="font-size:11px;color:var(--t2)">Zones côtières</span>
          <div class="tog" id="sub-tog"><div class="tog-k"></div></div>
        </div>
        <div class="sub-lvls" id="sub-lvls" style="display:none">
          <button class="slvl on" data-sl="vert" onclick="setSubLvl('vert')">Vert</button>
          <button class="slvl" data-sl="jaune" onclick="setSubLvl('jaune')">Jaune</button>
          <button class="slvl" data-sl="orange" onclick="setSubLvl('orange')">Orange</button>
          <button class="slvl" data-sl="rouge" onclick="setSubLvl('rouge')">Rouge</button>
        </div>
      </div>
      <div class="card">
        <p class="stitle" style="margin-bottom:6px">Double vigilance</p>
        <button class="dv-tog" onclick="toggleDV()">
          <span style="font-size:11px;color:var(--t2)">Activer 2ᵉ niveau</span>
          <div class="tog" id="dv-tog"><div class="tog-k"></div></div>
        </button>
        <div id="dv-opts" style="display:none;margin-top:8px">
          <p style="font-size:10px;color:var(--t3);margin-bottom:5px">2ᵉ niveau</p>
          <div id="dv-lvls"></div>
          <p style="font-size:10px;color:var(--t3);margin-top:6px;margin-bottom:4px">2ᵉ phénomène <span style="opacity:.5">(opt.)</span></p>
          <div class="pgrid" id="dv-pgrid"></div>
        </div>
      </div>
      <p class="hint">Clic = appliquer · Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetAll()">↺ Réinitialiser</button>
      <button class="act-btn primary" onclick="dlMap()">↓ Télécharger SVG</button>
    </div>
  </div>

  <div class="top" id="view-probable" style="display:none">
    <div id="map-prob-col">
      <div id="map-prob"></div>
      <div class="tip" id="tip-prob"></div>
      <div id="leg-prob" style="position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid rgba(124,58,237,.25);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none"></div>
    </div>
    <div class="sb">
      <div class="card" style="border-color:rgba(124,58,237,.2)">
        <p class="stitle" style="color:#7c6a9a">Niveau cible probable</p>
        <p class="prob-sidebar-note">Colorez les départements susceptibles de passer en vigilance supérieure. Les autres restent gris.</p>
        <div id="prob-lvl-container" style="margin-top:8px"></div>
      </div>
      <p class="hint" style="color:var(--t3)">Clic = appliquer · Re-clic = effacer<br>Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetProb()" style="border-color:rgba(124,58,237,.2);color:#7c6a9a">↺ Réinitialiser l'évolution</button>
      <button class="act-btn" onclick="dlMapProb()" style="border-color:rgba(124,58,237,.3);color:#c084fc">↓ Télécharger SVG</button>
    </div>
  </div>

  <div id="gauges"></div>
</div>

<div class="pop" id="lvl-pop"></div>
<div class="pop" id="phen-pop"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js"></script>
<script>
/* ── ICÔNES SVG (paths purs, net à toute résolution) ── */
/* URLs Météo France officielles (PNG haute qualité) */
const PICTO_URLS = {
  vent:         'https://vigilance.meteofrance.fr/images/picto/vent.png',
  orage:        'https://vigilance.meteofrance.fr/images/picto/orage.png',
  pluies:       'https://vigilance.meteofrance.fr/images/picto/pluie-inondation.png',
  inondation:   'https://vigilance.meteofrance.fr/images/picto/inondation.png',
  neige:        'https://vigilance.meteofrance.fr/images/picto/neige.png',
  verglas:      'https://vigilance.meteofrance.fr/images/picto/verglas.png',
  froid:        'https://vigilance.meteofrance.fr/images/picto/grand-froid.png',
  chaleur:      'https://vigilance.meteofrance.fr/images/picto/canicule.png',
  vagues:       'https://vigilance.meteofrance.fr/images/picto/vague-submersion.png',
  avalanches:   'https://vigilance.meteofrance.fr/images/picto/avalanche.png',
  neigeverglas: 'https://vigilance.meteofrance.fr/images/picto/neige-verglas.png'
};

/* Fallback Imgur si Météo France bloque (CORS) */
const PICTO_FALLBACK = {
  vent:         'https://i.imgur.com/w9U4uxl.png',
  orage:        'https://i.imgur.com/TqMfDUO.png',
  pluies:       'https://i.imgur.com/doeHp1Y.png',
  inondation:   'https://i.imgur.com/b6bctj5.png',
  neige:        'https://i.imgur.com/g0IsPe2.png',
  verglas:      'https://i.imgur.com/EJKnm3Q.png',
  froid:        'https://i.imgur.com/nCPwfFD.png',
  chaleur:      'https://i.imgur.com/EGeV2X0.png',
  vagues:       'https://i.imgur.com/u5zMvyy.png',
  avalanches:   'https://i.imgur.com/KZQXNzU.png',
  neigeverglas: 'https://i.imgur.com/59zrRen.png'
};

/* Cache de résolution URL (évite de retester à chaque render) */
const resolvedUrls = {};
async function resolveUrl(name) {
  if (resolvedUrls[name]) return resolvedUrls[name];
  const mf = PICTO_URLS[name];
  try {
    const r = await fetch(mf, { mode:'cors', cache:'force-cache' });
    if (r.ok) { resolvedUrls[name] = mf; return mf; }
  } catch(e) {}
  resolvedUrls[name] = PICTO_FALLBACK[name];
  return PICTO_FALLBACK[name];
}
/* Préchargement silencieux */
Object.keys(PICTO_FALLBACK).forEach(k => resolvedUrls[k] = PICTO_FALLBACK[k]);

/* Génère un <img> HTML avec rendu net */
function ico(name, sz) {
  const url = resolvedUrls[name] || PICTO_FALLBACK[name] || '';
  return `<img src="${url}" width="${sz}" height="${sz}" alt="${name}" style="image-rendering:auto;display:block;filter:drop-shadow(0 1px 3px rgba(0,0,0,.5));object-fit:contain">`;
}

/* ── NIVEAUX ── */
const LEVELS = {
  vert:         { color: '#31AA35', label: 'Vert' },
  'pre-alerte': { color: '#FFFF80', label: 'Pré-alerte' },
  jaune:        { color: '#FFF600', label: 'Jaune' },
  'jaune-orange':{ color: '#FFCC02', label: 'Jaune-Orange' },
  orange:       { color: '#FFB82B', label: 'Orange' },
  'orange-rouge':{ color: '#D13A10', label: 'Orange-Rouge' },
  rouge:        { color: '#CC0000', label: 'Rouge' },
  violet:       { color: '#9C27B0', label: 'Violet' }
};
const LVL_DEFS = [
  { k:'vert',          l:'Vert',          semi:false },
  { k:'pre-alerte',    l:'Pré-alerte',    semi:false },
  { k:'jaune',         l:'Jaune',         semi:false },
  { k:'jaune-orange',  l:'Semi J/O',      semi:true  },
  { k:'orange',        l:'Orange',        semi:false },
  { k:'orange-rouge',  l:'Semi O/R',      semi:true  },
  { k:'rouge',         l:'Rouge',         semi:false },
  { k:'violet',        l:'Violet',        semi:false }
];
const COASTAL = new Set(['Manche','Calvados','Seine-Maritime','Somme','Pas-de-Calais','Nord','Finistère',"Côtes-d'Armor",'Ille-et-Vilaine','Morbihan','Loire-Atlantique','Vendée','Charente-Maritime','Gironde','Landes','Hérault','Gard','Bouches-du-Rhône','Var','Alpes-Maritimes','Pyrénées-Orientales','Aude','Corse-du-Sud','Haute-Corse']);

const DFILL  = '#1e2535';
const DSTROKE= 'rgba(255,255,255,.18)';

/* ── ÉTAT ── */
let activeLevel  = 'vert';
let activePicto  = null;
let probLevel    = null;   // null = mode normal, sinon = niveau probable actif
let deptData     = { today: {}, tomorrow: {} };
let subVisible   = false;
let subLvl       = 'vert';
let dvActive     = false;
let dvLevel      = 'orange';
let dvPicto      = null;
let zoomedDept   = null;
let activeDay    = 'today';
let svgEl=null, svgSel=null, geoPath=null, layerSel=null;
let features=[], pathMap={}, hatchIds={};

/* ── ÉTAT CARTE PROBABLE ── */
let probData     = {};          // { deptName: { level, picto } }
let probActiveLevel = 'orange'; // niveau sélectionné dans sidebar probable
let probActivePicto = null;     // picto sélectionné dans la sidebar probable
let svgProbEl=null, svgProbSel=null, layerProbSel=null;
let zoomedDeptProb = null;
let activeView   = 'vigilance'; // 'vigilance' | 'probable'

/* ── SWITCH VUE ── */
function switchView(view) {
  activeView = view;
  document.getElementById('view-vigilance').style.display  = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('view-probable').style.display   = view === 'probable'  ? 'flex' : 'none';
  document.getElementById('day-tabs-wrap').style.display   = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('vtab-vigil').classList.toggle('on', view === 'vigilance');
  document.getElementById('vtab-prob').classList.toggle('on', view === 'probable');
  if (view === 'probable' && !svgProbEl) initProbMap();
  if (view === 'probable') redrawProbMap();
}
const gaugeData = {
  national: {
    today:    [{ picto: 'orage', segs: [{ s:6, e:15, lv:'jaune' }, { s:18, e:24, lv:'orange' }] }],
    tomorrow: [{ picto: null,    segs: [] }]
  }
};
function gRows(scope) {
  const k = scope || 'national';
  if (!gaugeData[k]) gaugeData[k] = { today:[{ picto:null, segs:[] }], tomorrow:[{ picto:null, segs:[] }] };
  return gaugeData[k][activeDay];
}

/* ── SIDEBAR PROBABLE ── */
function buildProbSidebar() {
  const c = document.getElementById('prob-lvl-container');
  // Niveaux cibles : jaune, jaune-orange, orange, orange-rouge, rouge, violet
  const probLevels = ['jaune','jaune-orange','orange','orange-rouge','rouge','violet'];
  probLevels.forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'prob-level-btn' + (k === probActiveLevel ? ' on' : '');
    btn.dataset.plk = k;
    const yc = (k === 'jaune' || k === 'jaune-orange') ? 'border:1px solid rgba(0,0,0,.4)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color};${yc}"></span>${LEVELS[k].label}`;
    btn.onclick = () => {
      probActiveLevel = k;
      document.querySelectorAll('[data-plk]').forEach(b => b.classList.toggle('on', b.dataset.plk === k));
    };
    c.appendChild(btn);
  });

  // Ajout de la grille de pictos dans la sidebar probable
  const pictoCard = document.createElement('div');
  pictoCard.className = 'card';
  pictoCard.style.borderColor = 'rgba(124,58,237,.2)';
  pictoCard.style.marginTop = '8px';
  const pTitle = document.createElement('p');
  pTitle.className = 'stitle';
  pTitle.style.color = '#7c6a9a';
  pTitle.textContent = 'Phénomène probable';
  pictoCard.appendChild(pTitle);
  const pGrid = document.createElement('div');
  pGrid.className = 'pgrid';
  pGrid.id = 'prob-pgrid';
  pictoCard.appendChild(pGrid);
  // Insérer après le conteneur de niveaux
  c.parentNode.insertBefore(pictoCard, c.nextSibling);

  // Remplir la grille de pictos
  const grid = document.getElementById('prob-pgrid');
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'pbtn';
    btn.dataset.ik = k;
    btn.innerHTML = ico(k, 26) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => {
      probActivePicto = probActivePicto === k ? null : k;
      document.querySelectorAll('#prob-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k));
    };
    grid.appendChild(btn);
  });
}

/* ── CARTE PROBABLE — init ── */
function initProbMap() {
  if (!features.length) return;
  const mapDiv = document.getElementById('map-prob');
  // Utiliser les dimensions du conteneur
  const W = 600;
  const H = 624;
  
  // Supprimer l'ancien SVG s'il existe
  d3.select(mapDiv).selectAll('svg').remove();
  
  const svg = d3.select(mapDiv).append('svg')
    .attr('viewBox', `0 0 ${W} ${H}`)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('display', 'block')
    .style('width', '100%')
    .style('height', '100%');
    
  svgProbEl = svg.node(); 
  svgProbSel = svg;
  svg.append('rect').attr('width',W).attr('height',H).attr('fill','#131823');
  layerProbSel = svg.append('g').attr('class','dept-layer-prob');
  layerProbSel.selectAll('path.dept-prob').data(features).join('path')
    .attr('class','dept-prob')
    .attr('d', d => geoPath(d))
    .attr('fill','#1e2535').attr('stroke','rgba(255,255,255,.14)').attr('stroke-width',0.6)
    .style('cursor','pointer')
    .on('mousemove', function(event, d) {
      const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
      const tip = document.getElementById('tip-prob');
      const info = probData[nm];
      tip.style.display = 'block';
      const r = mapDiv.getBoundingClientRect();
      tip.style.left = (event.clientX-r.left+14)+'px';
      tip.style.top  = (event.clientY-r.top-36)+'px';
      let txt = nm;
      if (info) {
        txt += ` — Probable : ${LEVELS[info.level].label}`;
        if (info.picto) txt += ` · ${info.picto}`;
      } else {
        txt += ' — Pas de changement prévu';
      }
      tip.textContent = txt;
      if (nm !== zoomedDeptProb) d3.select(this).attr('stroke','rgba(255,255,255,.5)').attr('stroke-width',1.1);
    })
    .on('mouseleave', function(event, d) {
      document.getElementById('tip-prob').style.display = 'none';
      const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
      if (nm !== zoomedDeptProb) redrawProbPath(d3.select(this), nm);
    })
    .on('click', function(event, d) {
      const nm = d.properties&&(d.properties.name||d.properties.NAME||''); if(!nm) return;
      const current = probData[nm];
      if (current && current.level === probActiveLevel && current.picto === (probActivePicto || null)) {
        delete probData[nm];
      } else {
        probData[nm] = { level: probActiveLevel, picto: probActivePicto || null };
      }
      redrawProbMap();
    })
    .on('dblclick', function(event, d) {
      event.stopPropagation();
      const nm = d.properties&&(d.properties.name||d.properties.NAME||''); if(!nm) return;
      if (zoomedDeptProb === nm) { unzoomProb(); return; }
      zoomToProb(d, nm);
    });
  svg.on('dblclick', e => { if (e.target===svgProbEl) unzoomProb(); });
  
  // Mettre à jour la légende
  updateLegProb();
}

function redrawProbPath(sel, nm) {
  const info = probData[nm];
  const isZoomed = nm === zoomedDeptProb;
  if (info) {
    sel.attr('fill', LEVELS[info.level].color)
       .attr('stroke', isZoomed ? '#fff' : 'rgba(0,0,0,.2)')
       .attr('stroke-width', isZoomed ? 2.5 : 0.6);
  } else {
    sel.attr('fill', '#1e2535')
       .attr('stroke', isZoomed ? '#fff' : 'rgba(255,255,255,.14)')
       .attr('stroke-width', isZoomed ? 2.5 : 0.6);
  }
}

function redrawProbMap() {
  if (!layerProbSel) return;
  // Mettre à jour les couleurs des départements
  layerProbSel.selectAll('path.dept-prob').each(function(d) {
    const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
    redrawProbPath(d3.select(this), nm);
  });

  // Ajouter les pictogrammes sur la carte probable
  layerProbSel.selectAll('.dept-picto-prob').remove();
  Object.entries(probData).forEach(([name, info]) => {
    if (!info.picto || !PICTO_FALLBACK[info.picto]) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === name);
    if (!feat) return;
    const b = geoPath.bounds(feat);
    const cx = (b[0][0] + b[1][0]) / 2;
    const cy = (b[0][1] + b[1][1]) / 2;
    const sz = Math.max(12, Math.min((Math.min(b[1][0]-b[0][0], b[1][1]-b[0][1])) * 0.52, 26));
    const url = resolvedUrls[info.picto] || PICTO_FALLBACK[info.picto];
    
    layerProbSel.append('image')
      .attr('class', 'dept-picto-prob')
      .attr('href', url)
      .attr('x', cx - sz/2).attr('y', cy - sz/2)
      .attr('width', sz).attr('height', sz)
      .attr('pointer-events', 'none')
      .style('image-rendering', 'auto');
  });

  updateLegProb();
}

function zoomToProb(feat, name) {
  const svgNode = svgProbEl;
  const viewBox = svgNode.getAttribute('viewBox').split(' ').map(Number);
  const W = viewBox[2] || 600, H = viewBox[3] || 624;
  zoomedDeptProb = name;
  layerProbSel.selectAll('path.dept-prob').attr('stroke','rgba(255,255,255,.14)').attr('stroke-width',0.6);
  layerProbSel.selectAll('path.dept-prob').filter(d=>(d.properties.name||d.properties.NAME)===name)
    .raise().attr('stroke','#fff').attr('stroke-width',2.5);
  const b = geoPath.bounds(feat);
  const sc = Math.min(0.82*W/(b[1][0]-b[0][0]), 0.82*H/(b[1][1]-b[0][1]), 6);
  const cx=(b[0][0]+b[1][0])/2, cy=(b[0][1]+b[1][1])/2;
  layerProbSel.transition().duration(480).ease(d3.easeCubicInOut)
    .attr('transform',`translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
}

function unzoomProb() {
  zoomedDeptProb = null;
  layerProbSel.selectAll('path.dept-prob').attr('stroke','rgba(255,255,255,.14)').attr('stroke-width',0.6);
  layerProbSel.transition().duration(400).attr('transform',null);
}

function resetProb() {
  probData = {};
  redrawProbMap();
}

function updateLegProb() {
  const leg = document.getElementById('leg-prob');
  const distinct = new Set(Object.values(probData).map(v => v.level));
  if (!distinct.size) { leg.style.display = 'none'; return; }
  leg.style.display = 'block';
  let html = '<div class="leg-sec"><div class="leg-sec-t">Évolution probable</div>';
  const order = ['jaune','jaune-orange','orange','orange-rouge','rouge','violet'];
  order.forEach(k => {
    if (distinct.has(k)) {
      html += `<div class="leg-r"><span class="leg-dot" style="background:${LEVELS[k].color}"></span><span class="leg-lbl">${LEVELS[k].label}</span></div>`;
    }
  });
  html += '</div>';
  leg.innerHTML = html;
}

function dlMapProb() {
  if (!svgProbEl) return;
  // Cloner le SVG pour inclure les styles nécessaires
  const clone = svgProbEl.cloneNode(true);
  clone.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
  // Injection basique de styles pour l'export
  const style = document.createElement('style');
  style.textContent = `
    path { stroke-linecap: round; stroke-linejoin: round; }
    image { image-rendering: auto; }
  `;
  clone.insertBefore(style, clone.firstChild);
  
  const blob = new Blob([new XMLSerializer().serializeToString(clone)], {type:'image/svg+xml;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `carte_evolution_probable_${new Date().toISOString().slice(0,10)}.svg`;
  a.click();
}

/* ── INITIALISATION COMPLÈTE VIGILANCE ── */
function curData() { return deptData[activeDay]; }

function switchDay(day) {
  activeDay = day;
  document.getElementById('dtab-today').classList.toggle('on', day==='today');
  document.getElementById('dtab-tomorrow').classList.toggle('on', day==='tomorrow');
  redrawMap();
  renderGauges();
}

function buildSidebar() {
  const container = document.getElementById('lvl-container');
  LVL_DEFS.forEach(d => {
    const wrap = document.createElement('div');
    wrap.className = 'lvl-row-wrap';
    const btn = document.createElement('button');
    btn.className = 'lvl-btn' + (d.k===activeLevel && !probLevel ? ' on' : '') + (d.semi ? ' semi' : '');
    btn.dataset.lk = d.k;
    const yc = (d.k==='jaune'||d.k==='jaune-orange') ? 'border:1px solid rgba(0,0,0,.4)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[d.k].color};${yc}"></span>${d.l}`;
    btn.onclick = () => {
      probLevel = null;
      activeLevel = d.k;
      document.querySelectorAll('.lvl-btn').forEach(b => b.classList.toggle('on', b.dataset.lk===d.k && !b.classList.contains('prob')));
      document.querySelectorAll('.lvl-btn.prob').forEach(b => b.classList.remove('on'));
    };
    wrap.appendChild(btn);
    container.appendChild(wrap);
  });

  const grid = document.getElementById('pgrid');
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'pbtn';
    btn.dataset.ik = k;
    btn.innerHTML = ico(k,22) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => {
      activePicto = activePicto===k ? null : k;
      document.querySelectorAll('#pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik===activePicto));
    };
    grid.appendChild(btn);
  });

  // Sidebar double vigilance
  const dvLvls = document.getElementById('dv-lvls');
  dvLvls.style.display = 'flex';
  dvLvls.style.gap = '3px';
  ['jaune','orange','rouge'].forEach(k => {
    const b = document.createElement('button');
    b.className = 'slvl' + (k===dvLevel ? ' on' : '');
    b.style.flex = '1';
    b.dataset.dlv = k;
    b.textContent = k.charAt(0).toUpperCase() + k.slice(1);
    b.onclick = () => {
      dvLevel = k;
      document.querySelectorAll('[data-dlv]').forEach(x => x.classList.toggle('on', x.dataset.dlv===k));
      updateDVBadges();
    };
    dvLvls.appendChild(b);
  });

  const dvGrid = document.getElementById('dv-pgrid');
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'pbtn';
    btn.dataset.dvik = k;
    btn.innerHTML = ico(k,20) + `<span>${k.slice(0,5)}</span>`;
    btn.onclick = () => {
      dvPicto = dvPicto===k ? null : k;
      document.querySelectorAll('#dv-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.dvik===dvPicto));
      updateDVBadges();
    };
    dvGrid.appendChild(btn);
  });
}

function toggleSub() {
  subVisible = !subVisible;
  document.getElementById('sub-tog').classList.toggle('on', subVisible);
  document.getElementById('sub-lvls').style.display = subVisible ? 'flex' : 'none';
  redrawMap();
}

function setSubLvl(l) {
  subLvl = l;
  document.querySelectorAll('[data-sl]').forEach(b => b.classList.toggle('on', b.dataset.sl===l));
  redrawMap();
}

function toggleDV() {
  dvActive = !dvActive;
  document.getElementById('dv-tog').classList.toggle('on', dvActive);
  document.getElementById('dv-opts').style.display = dvActive ? 'block' : 'none';
  if(!dvActive) updateDVBadges();
}

function updateDVBadges() {
  redrawMap();
}

function makeHatch(c1, c2) {
  const id = `h_${c1.replace('#','')}_${c2.replace('#','')}`;
  if (hatchIds[id]) return `url(#${id})`;
  const defs = svgSel.select('defs').node() ? svgSel.select('defs') : svgSel.append('defs');
  const pat = defs.append('pattern')
    .attr('id', id).attr('patternUnits', 'userSpaceOnUse')
    .attr('width', 10).attr('height', 10).attr('patternTransform', 'rotate(45)');
  pat.append('rect').attr('width', 10).attr('height', 10).attr('fill', c1);
  pat.append('line').attr('x1', 0).attr('y1', 0).attr('x2', 0).attr('y2', 10).attr('stroke', c2).attr('stroke-width', 4);
  hatchIds[id] = true;
  return `url(#${id})`;
}

function fillFor(nm) {
  const d = curData()[nm];
  if (subVisible && COASTAL.has(nm) && subLvl!=='vert') {
    const base = d ? LEVELS[d.level].color : LEVELS.vert.color;
    return makeHatch(base, LEVELS[subLvl].color);
  }
  if (!d) return DFILL;
  return LEVELS[d.level].color;
}

function redrawPath(sel, nm) {
  const isZoomed = nm===zoomedDept;
  sel.attr('fill', fillFor(nm))
     .attr('stroke', isZoomed ? '#fff' : DSTROKE)
     .attr('stroke-width', isZoomed ? 2.5 : 0.6);
}

function redrawMap() {
  if (!layerSel) return;
  layerSel.selectAll('path.dept').each(function(d) {
    const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
    redrawPath(d3.select(this), nm);
  });

  // Badges & pictos
  layerSel.selectAll('.dept-badge').remove();
  layerSel.selectAll('.dept-picto').remove();

  Object.entries(curData()).forEach(([name, info]) => {
    const feat = pathMap[name]; if (!feat) return;
    const b = geoPath.bounds(feat);
    const cx = (b[0][0]+b[1][0])/2;
    const cy = (b[0][1]+b[1][1])/2;
    const sz = Math.max(12, Math.min((Math.min(b[1][0]-b[0][0], b[1][1]-b[0][1])) * 0.52, 26));

    // Si double vigilance active sur ce département
    if (info.dv) {
      const bx = cx - sz/2, by = cy - sz/2;
      const bgg = layerSel.append('g').attr('class', 'dept-badge').attr('pointer-events','none');
      bgg.append('rect')
        .attr('x', bx).attr('y', by).attr('width', sz).attr('height', sz).attr('rx', 3)
        .attr('fill', LEVELS[info.dv.level].color).attr('stroke', '#rgba(0,0,0,.2)').attr('stroke-width', 0.5);
      if (info.dv.picto) {
        bgg.append('image')
          .attr('href', resolvedUrls[info.dv.picto] || PICTO_FALLBACK[info.dv.picto])
          .attr('x', bx+1).attr('y', by+1).attr('width', sz-2).attr('height', sz-2)
          .style('image-rendering','auto');
      } else {
        bgg.append('text')
          .attr('x', cx).attr('y', cy+3).attr('text-anchor','middle')
          .attr('fill', (info.dv.level==='jaune')?'#000':'#fff').style('font-size','8px').style('font-weight','700')
          .text('2');
      }

      // Décale le picto principal
      if (info.picto) {
        layerSel.append('image')
          .attr('class', 'dept-picto')
          .attr('href', resolvedUrls[info.picto] || PICTO_FALLBACK[info.picto])
          .attr('x', cx - sz*1.1).attr('y', cy - sz/2).attr('width', sz).attr('height', sz)
          .attr('pointer-events','none').style('image-rendering','auto');
      }
    } else if (info.picto) {
      layerSel.append('image')
        .attr('class', 'dept-picto')
        .attr('href', resolvedUrls[info.picto] || PICTO_FALLBACK[info.picto])
        .attr('x', cx - sz/2).attr('y', cy - sz/2).attr('width', sz).attr('height', sz)
        .attr('pointer-events','none').style('image-rendering','auto');
    }
  });

  updateLeg();
}

function updateLeg() {
  const leg = document.getElementById('leg');
  const active = new Set(Object.values(curData()).map(v => v.level));
  if (subVisible && subLvl!=='vert') active.add(subLvl);
  if (!active.size) { leg.style.display = 'none'; return; }
  leg.style.display = 'block';
  let html = '';
  if (subVisible && subLvl!=='vert') {
    html += `<div class="leg-sec"><div class="leg-sec-t">Submersion</div><div class="leg-r"><span class="leg-dot" style="background:${makeHatch('#222536', LEVELS[subLvl].color)}"></span><span class="leg-lbl">Littoral ${LEVELS[subLvl].label}</span></div></div>`;
  }
  html += '<div class="leg-sec"><div class="leg-sec-t">Vigilance</div>';
  const order = ['violet','rouge','orange-rouge','orange','jaune-orange','jaune','pre-alerte'];
  order.forEach(k => {
    if (active.has(k)) {
      html += `<div class="leg-r"><span class="leg-dot" style="background:${LEVELS[k].color}"></span><span class="leg-lbl">${LEVELS[k].label}</span></div>`;
    }
  });
  html += '</div>';
  leg.innerHTML = html;
}

function zoomTo(feat, name) {
  const svgNode = svgEl;
  const viewBox = svgNode.getAttribute('viewBox').split(' ').map(Number);
  const W = viewBox[2], H = viewBox[3];
  zoomedDept = name;
  layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6);
  layerSel.selectAll('path.dept').filter(d => (d.properties.name||d.properties.NAME)===name)
    .raise().attr('stroke', '#fff').attr('stroke-width', 2.5);
  const b = geoPath.bounds(feat);
  const sc = Math.min(0.85*W/(b[1][0]-b[0][0]), 0.85*H/(b[1][1]-b[0][1]), 6);
  const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
  layerSel.transition().duration(450).ease(d3.easeCubicInOut)
    .attr('transform', `translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
  renderGauges();
}

function unzoom() {
  zoomedDept = null;
  layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6);
  layerSel.transition().duration(350).attr('transform', null);
  renderGauges();
}

function resetAll() {
  deptData = { today:{}, tomorrow:{} };
  subVisible = false; subLvl = 'vert'; dvActive = false; dvPicto = null;
  document.getElementById('sub-tog').classList.remove('on');
  document.getElementById('sub-lvls').style.display = 'none';
  document.getElementById('dv-tog').classList.remove('on');
  document.getElementById('dv-opts').style.display = 'none';
  document.querySelectorAll('#pgrid .pbtn, #dv-pgrid .pbtn').forEach(b => b.classList.remove('on'));
  redrawMap();
  renderGauges();
}

function dlMap() {
  const clone = svgEl.cloneNode(true);
  clone.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
  const blob = new Blob([new XMLSerializer().serializeToString(clone)], {type:'image/svg+xml;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `carte_vigilance_${activeDay}_${new Date().toISOString().slice(0,10)}.html`;
  a.click();
}

/* ── TIMELINES / JAUGES CHRONOLOGIQUES ── */
let dragData = null;

function renderGauges() {
  const container = document.getElementById('gauges');
  container.innerHTML = '';
  const rows = gRows(zoomedDept);
  const wrap = document.createElement('div');
  wrap.className = 'g-wrap' + (zoomedDept ? ' dept' : '');
  
  let hhtml = `<div class="g-hdr"><div class="g-title">`;
  if (zoomedDept) {
    hhtml += `<button class="g-back" onclick="unzoom()">← National</button> Chronologie : ${zoomedDept}`;
  } else {
    hhtml += `<span class="lvl-dot" style="background:var(--acc)"></span>Chronologie Nationale`;
  }
  hhtml += `</div><span class="g-badge">${activeDay==='today'?'Aujourd\'hui':'Demain'}</span></div>`;
  hhtml += `<div class="g-rows" id="g-rows-list"></div>`;
  hhtml += `<div class="g-ticks">`;
  for(let i=0; i<=24; i+=3) hhtml += `<div class="g-tick">${i}h</div>`;
  hhtml += `</div><div style="margin-top:4px"><button class="g-add" onclick="addGaugeRow('${zoomedDept||'national'}')">+ Ajouter un phénomène</button></div>`;
  wrap.innerHTML = hhtml;
  container.appendChild(wrap);

  const list = wrap.querySelector('#g-rows-list');
  rows.forEach((r, idx) => {
    const row = document.createElement('div');
    row.className = 'g-row';
    row.innerHTML = `
      <button class="g-picto" data-gidx="${idx}">${r.picto ? ico(r.picto,18) : '?'}</button>
      <div class="g-lbl" title="${r.picto||'Global'}">${r.picto ? r.picto.toUpperCase() : 'Vigilance'}</div>
      <div class="g-tl" data-gidx="${idx}">
        <div class="g-bg"></div>
        <svg></svg>
      </div>
      <button class="g-del" onclick="delGaugeRow('${zoomedDept||'national'}',${idx})">×</button>
    `;
    
    // Clic picto -> ouvre popup choix phénomène
    row.querySelector('.g-picto').onclick = (e) => {
      e.stopPropagation();
      openPhenPop(e.currentTarget, zoomedDept||'national', idx);
    };

    list.appendChild(row);
    drawTimelineSegments(row.querySelector('.g-tl svg'), r.segs, idx);
    initTimelineEvents(row.querySelector('.g-tl'), idx);
  });
}

function drawTimelineSegments(svgEl, segs, rowIdx) {
  const svg = d3.select(svgEl);
  svg.selectAll('*').remove();
  const W = svgEl.parentNode.clientWidth;
  svgEl.setAttribute('width', W);
  svgEl.setAttribute('height', 22);

  segs.forEach((s) => {
    const x1 = (s.s / 24) * W;
    const x2 = (s.e / 24) * W;
    svg.append('rect')
      .attr('x', x1).attr('y', 0).attr('width', Math.max(2, x2-x1)).attr('height', 22).attr('rx', 4)
      .attr('fill', LEVELS[s.lv].color).attr('stroke', 'rgba(0,0,0,.2)').attr('stroke-width', 0.5);
  });
}

function initTimelineEvents(tlDiv, rowIdx) {
  let isMousedown = false;
  tlDiv.addEventListener('mousedown', (e) => {
    isMousedown = true;
    const rect = tlDiv.getBoundingClientRect();
    const t = Math.floor(((e.clientX - rect.left) / rect.width) * 24);
    const scope = zoomedDept || 'national';
    const row = gaugeData[scope][activeDay][rowIdx];
    
    // Si clic avec picto actif, on dessine/ajoute
    const lvl = probLevel || activeLevel || 'vert';
    const hr = Math.max(0, Math.min(23, t));
    
    // Supprime tout segment existant sur cette heure
    row.segs = row.segs.filter(s => !(hr >= s.s && hr < s.e));
    if (lvl !== 'vert') {
      row.segs.push({ s: hr, e: hr+1, lv: lvl });
      mergeSegments(row.segs);
    }
    drawTimelineSegments(tlDiv.querySelector('svg'), row.segs, rowIdx);
  });

  window.addEventListener('mouseup', () => { isMousedown = false; });
  tlDiv.addEventListener('mousemove', (e) => {
    if (!isMousedown) return;
    const rect = tlDiv.getBoundingClientRect();
    const t = Math.floor(((e.clientX - rect.left) / rect.width) * 24);
    const hr = Math.max(0, Math.min(23, t));
    const scope = zoomedDept || 'national';
    const row = gaugeData[scope][activeDay][rowIdx];
    const lvl = probLevel || activeLevel || 'vert';
    
    row.segs = row.segs.filter(s => !(hr >= s.s && hr < s.e));
    if (lvl !== 'vert') {
      row.segs.push({ s: hr, e: hr+1, lv: lvl });
      mergeSegments(row.segs);
    }
    drawTimelineSegments(tlDiv.querySelector('svg'), row.segs, rowIdx);
  });
}

function mergeSegments(segs) {
  if(!segs.length) return;
  segs.sort((a,b) => a.s - b.s);
  for(let i=0; i<segs.length-1; i++) {
    if(segs[i].e === segs[i+1].s && segs[i].lv === segs[i+1].lv) {
      segs[i].e = segs[i+1].e;
      segs.splice(i+1, 1);
      i--;
    }
  }
}

function addGaugeRow(scope) {
  if(!gaugeData[scope]) gaugeData[scope] = { today:[], tomorrow:[] };
  gaugeData[scope][activeDay].push({ picto: null, segs: [] });
  renderGauges();
}

function delGaugeRow(scope, idx) {
  gaugeData[scope][activeDay].splice(idx, 1);
  renderGauges();
}

function openPhenPop(btn, scope, idx) {
  const pop = document.getElementById('phen-pop');
  pop.innerHTML = '';
  pop.classList.add('open');
  const r = btn.getBoundingClientRect();
  pop.style.left = `${r.left + window.scrollX}px`;
  pop.style.top = `${r.bottom + window.scrollY + 4}px`;

  Object.keys(PICTO_FALLBACK).forEach(k => {
    const item = document.createElement('div');
    item.className = 'pp-item';
    item.innerHTML = ico(k,20) + `<span>${k.slice(0,6)}</span>`;
    item.onclick = () => {
      gaugeData[scope][activeDay][idx].picto = k;
      pop.classList.remove('open');
      renderGauges();
    };
    pop.appendChild(item);
  });

  const clear = document.createElement('div');
  clear.className = 'pp-item';
  clear.style.gridColumn = 'span 4';
  clear.style.padding = '4px';
  clear.style.textAlign = 'center';
  clear.textContent = 'Aucun (Global)';
  clear.onclick = () => {
    gaugeData[scope][activeDay][idx].picto = null;
    pop.classList.remove('open');
    renderGauges();
  };
  pop.appendChild(clear);

  setTimeout(() => {
    window.addEventListener('click', function cb() {
      pop.classList.remove('open');
      window.removeEventListener('click', cb);
    });
  }, 10);
}

/* ── CHARGEMENT DU FOND DE CARTE TOPOJSON DE LA FRANCE ── */
async function init() {
  buildSidebar();
  buildProbSidebar(); // Construire la sidebar pour l'évolution probable

  const mapDiv = document.getElementById('map');
  const W = 600;
  const H = 624;

  const svg = d3.select(mapDiv).append('svg')
    .attr('viewBox', `0 0 ${W} ${H}`)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('display', 'block')
    .style('width', '100%')
    .style('height', '100%');

  svgEl = svg.node();
  svgSel = svg;
  svg.append('rect').attr('width', W).attr('height', H).attr('fill', '#131823');
  layerSel = svg.append('g').attr('class', 'dept-layer');

  const tip = document.getElementById('tip');

  try {
    // Topojson allégé officiel des départements français
    const topo = await d3.json('https://raw.githubusercontent.com/gregoiredavid/france-geojson/master/departements.geojson');
    
    // Conversion GeoJSON direct si le fichier est déjà du GeoJSON pur
    features = topo.type === 'FeatureCollection' ? topo.features : topojson.feature(topo, topo.objects.departements).features;

    // Projection équi-rectangulaire centrée sur la France
    const proj = d3.geoConicConformal()
      .center([2.45, 46.28])
      .scale(2900)
      .translate([W / 2, H / 2]);

    geoPath = d3.geoPath().projection(proj);

    layerSel.selectAll('path.dept')
      .data(features)
      .join('path')
      .attr('class', 'dept')
      .attr('d', d => geoPath(d))
      .attr('fill', DFILL)
      .attr('stroke', DSTROKE)
      .attr('stroke-width', 0.6)
      .style('cursor', 'pointer')
      .on('mousemove', function(event, d) {
        const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
        tip.style.display = 'block';
        const r = mapDiv.getBoundingClientRect();
        tip.style.left = (event.clientX - r.left + 14) + 'px';
        tip.style.top = (event.clientY - r.top - 34) + 'px';
        
        const data = curData()[nm];
        let txt = nm;
        if(data) {
          txt += ` — Vigilance : ${LEVELS[data.level].label}`;
          if(data.picto) txt += ` · Phénomène : ${data.picto}`;
          if(data.dv) txt += ` (Double V. : ${LEVELS[data.dv.level].label})`;
        } else {
          txt += ' — Vigilance : Verte';
        }
        tip.textContent = txt;
        if (nm !== zoomedDept) d3.select(this).attr('stroke', 'rgba(255,255,255,.5)').attr('stroke-width', 1.1);
      })
      .on('mouseleave', function(event, d) {
        tip.style.display = 'none';
        const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
        if (nm !== zoomedDept) redrawPath(d3.select(this), nm);
      })
      .on('click', function(event, d) {
        const nm = d.properties&&(d.properties.name||d.properties.NAME||''); if(!nm) return;
        const data = curData(); const dd = data[nm];
        if (dvActive) {
          if (!dd) return;
          if (dd.dv && dd.dv.level===dvLevel && dd.dv.picto===(dvPicto||null)) delete dd.dv;
          else dd.dv = { level:dvLevel, picto:dvPicto||null };
          updateDVBadges(); return;
        }
        const isProb = !!probLevel;
        if (dd && dd.level===activeLevel && dd.picto===(activePicto||null) && !!dd.probable===isProb) {
          delete data[nm];
        } else {
          const prevDv = dd ? dd.dv : null;
          data[nm] = { level:activeLevel, picto:activePicto||null, dv:prevDv||null };
          if (isProb) data[nm].probable = true;
        }
        redrawMap();
      })
      .on('dblclick', function(event, d) {
        event.stopPropagation();
        const nm = d.properties&&(d.properties.name||d.properties.NAME||''); if(!nm) return;
        if (zoomedDept===nm) { unzoom(); return; }
        if (!gaugeData[nm]) gaugeData[nm] = { today:[{picto:null,segs:[]}], tomorrow:[{picto:null,segs:[]}] };
        zoomTo(d, nm);
      });

    svg.on('dblclick', (e) => { if(e.target === svgEl) unzoom(); });

    features.forEach(f => {
      const nm = f.properties&&(f.properties.name||f.properties.NAME||'');
      if(nm) pathMap[nm] = f;
    });

    redrawMap();
    renderGauges();

  } catch(err) {
    console.error('Erreur au chargement de la carte:', err);
  }
}

window.onload = init;
</script>
</body>
</html>
