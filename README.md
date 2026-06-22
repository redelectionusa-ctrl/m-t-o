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
.page{display:flex;flex-direction:column;gap:12px;padding:12px;max-width:1680px;margin:0 auto}

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
/* ── Vue Vigilance prochain jour (anciennes régions) ── */
#map-next-col{flex:1;min-width:0;position:relative}
#map-next{width:100%;aspect-ratio:1/1.04;border-radius:12px;overflow:hidden;background:#1a1d27;border:1px solid rgba(20,184,196,.2);position:relative}
.next-sidebar-note{font-size:10px;color:var(--t3);line-height:1.5;padding:8px;background:rgba(20,184,196,.07);border:1px solid rgba(20,184,196,.18);border-radius:8px;margin-bottom:2px}

.lvl-btn:disabled{opacity:.3;cursor:not-allowed;filter:grayscale(.7)}
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
      <!-- Sélecteur de vue -->
      <div class="view-tabs">
        <button class="vtab on" id="vtab-vigil" onclick="switchView('vigilance')">Vigilance</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-next" onclick="switchView('nextday')" style="color:#14b8c4;border-color:rgba(20,184,196,.3)">→ Prochain jour (régions)</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-prob" onclick="switchView('probable')" style="color:#7c6a9a;border-color:rgba(124,58,237,.3)">↗ Évolution probable</button>
      </div>
      <!-- Sélecteur de jour (masqué en mode probable) -->
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
      <div class="edit-zone">
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
      </div>
      <button class="act-btn primary" onclick="dlMap()">↓ Télécharger SVG</button>
      <button class="act-btn" onclick="exportPng(svgEl,'vigilance-france.png')">↓ Télécharger PNG</button>
    </div>
  </div>

  <!-- VUE VIGILANCE PROCHAIN JOUR (anciennes régions, risque % par phénomène) -->
  <div class="top" id="view-nextday" style="display:none">
    <div id="map-next-col">
      <div id="map-next"></div>
      <div class="tip" id="tip-next"></div>
      <div id="leg-next" style="position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid rgba(20,184,196,.25);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none"></div>
    </div>
    <div class="sb">
      <div class="edit-zone">
      <div class="card" style="border-color:rgba(20,184,196,.2)">
        <p class="stitle" style="color:#14b8c4">Risque prochain jour</p>
        <p class="next-sidebar-note">Probabilité d'occurrence du phénomène par ancienne région (avant 2016) pour le lendemain. Plus la teinte est foncée, plus le risque est élevé.</p>
      </div>
      <div class="card" style="border-color:rgba(20,184,196,.2)">
        <p class="stitle" style="color:#14b8c4">Phénomène</p>
        <div class="pgrid" id="next-pgrid"></div>
      </div>
      <div class="card" style="border-color:rgba(20,184,196,.2)">
        <p class="stitle" style="color:#14b8c4">Risque (%)</p>
        <div id="next-lvl-container" style="margin-top:8px"></div>
      </div>
      <p class="hint">Clic = appliquer · Re-clic = effacer<br>Jusqu'à 2 phénomènes/région · Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetNext()" style="border-color:rgba(20,184,196,.2);color:#14b8c4">↺ Réinitialiser</button>
      </div>
      <button class="act-btn" onclick="dlMapNext()" style="border-color:rgba(20,184,196,.3);color:#14b8c4">↓ Télécharger SVG</button>
      <button class="act-btn" onclick="exportPng(svgNextEl,'vigilance-prochain-jour.png')" style="border-color:rgba(20,184,196,.3);color:#14b8c4">↓ Télécharger PNG</button>
    </div>
  </div>

  <!-- VUE ÉVOLUTION PROBABLE -->
  <div class="top" id="view-probable" style="display:none">
    <div id="map-prob-col">
      <div id="map-prob"></div>
      <div class="tip" id="tip-prob"></div>
      <div id="leg-prob" style="position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid rgba(124,58,237,.25);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none"></div>
    </div>
    <div class="sb">
      <div class="edit-zone">
      <div class="card" style="border-color:rgba(124,58,237,.2)">
        <p class="stitle" style="color:#7c6a9a">Niveau cible probable</p>
        <p class="prob-sidebar-note">Colorez les départements susceptibles de passer en vigilance supérieure. Les autres restent gris.</p>
        <div id="prob-lvl-container" style="margin-top:8px"></div>
      </div>
      <p class="hint" style="color:var(--t3)">Clic = appliquer · Re-clic = effacer<br>Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetProb()" style="border-color:rgba(124,58,237,.2);color:#7c6a9a">↺ Réinitialiser l'évolution</button>
      </div>
      <button class="act-btn" onclick="dlMapProb()" style="border-color:rgba(124,58,237,.3);color:#c084fc">↓ Télécharger SVG</button>
      <button class="act-btn" onclick="exportPng(svgProbEl,'evolution-probable.png')" style="border-color:rgba(124,58,237,.3);color:#c084fc">↓ Télécharger PNG</button>
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

/* Pictos personnalisés (vent, froid, chaleur, inondation, avalanches) — utilisés partout
   (carte principale + prochain jour + légendes). Repli auto vers le picto Météo France/Imgur
   habituel si le lien perso échoue (onerror), donc un lien mort ne casse jamais l'affichage. */
const NEXT_PICTO_OVERRIDE = {
  froid:      'https://i.postimg.cc/vTnvg3J0/Capture-d-ecran-2026-06-21-182611-preview-rev-1.png',
  vent:       'https://i.postimg.cc/C584Bm3v/Capture-d-ecran-2026-06-21-182356-preview-rev-1.png',
  inondation: 'https://i.postimg.cc/mk9Sz8vV/Capture-d-ecran-2026-06-21-182401-preview-rev-1.png',
  avalanches: 'https://i.postimg.cc/J0XQDPfc/Capture-d-ecran-2026-06-21-182621-preview-rev-1.png',
  chaleur:    'https://i.postimg.cc/hvm0zpkp/Capture-d-ecran-2026-06-21-182607-preview-rev-1.png'
};
const NEXT_PICTO_SCALE = 1.3; // agrandissement (taille réelle, pas de transform CSS = pas de flou)
function nextIcoUrl(name) { return NEXT_PICTO_OVERRIDE[name] || resolvedUrls[name] || PICTO_FALLBACK[name] || ''; }

/* Génère un <img> HTML avec rendu net, agrandi + repli auto pour les pictos perso. */
function ico(name, sz) {
  const isCustom = !!NEXT_PICTO_OVERRIDE[name];
  const realSz = isCustom ? Math.round(sz * NEXT_PICTO_SCALE) : sz;
  const fb = resolvedUrls[name] || PICTO_FALLBACK[name] || '';
  const onerr = isCustom ? `this.onerror=null;this.width=${sz};this.height=${sz};this.src='${fb}';` : `this.onerror=null;`;
  return `<img src="${nextIcoUrl(name)}" width="${realSz}" height="${realSz}" alt="${name}" onerror="${onerr}" style="image-rendering:auto;display:block;filter:drop-shadow(0 1px 3px rgba(0,0,0,.5));object-fit:contain">`;
}

/* Place un picto (SVG <image>, agrandi + net si perso) centré en (cx,cy) dans `sel`, avec repli
   auto si le lien perso échoue. Partagé par les 3 cartes pour éviter la duplication. */
function placeIcoImg(sel, name, cx, cy, sz) {
  const isCustom = !!NEXT_PICTO_OVERRIDE[name];
  const drawSz = isCustom ? sz * NEXT_PICTO_SCALE : sz;
  const fb = resolvedUrls[name] || PICTO_FALLBACK[name];
  return sel.append('image')
    .attr('href', nextIcoUrl(name))
    .attr('x', cx - drawSz/2).attr('y', cy - drawSz/2).attr('width', drawSz).attr('height', drawSz)
    .attr('pointer-events', 'none').style('image-rendering', 'auto')
    .on('error', function() {
      if (this.getAttribute('href') !== fb) d3.select(this).attr('href', fb).attr('x', cx-sz/2).attr('y', cy-sz/2).attr('width', sz).attr('height', sz);
    });
}

/* ── EXPORT PNG (partagé par les 3 cartes) ──
   Inline les pictos en data-URI (sinon le canvas est "tainted" par les images externes et le
   PNG sort vide) et passe les frontières dépt/région en noir, uniquement dans l'export. */
async function imgToDataURL(url) {
  try {
    const blob = await (await fetch(url, { mode: 'cors' })).blob();
    return await new Promise((res, rej) => { const fr = new FileReader(); fr.onload = () => res(fr.result); fr.onerror = rej; fr.readAsDataURL(blob); });
  } catch { return null; }
}
async function exportPng(svgEl, filename) {
  if (!svgEl) return;
  const c = svgEl.cloneNode(true);
  c.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
  c.querySelectorAll('path.dept, path.dept-prob, path.region-next, path.region-split')
    .forEach(p => p.setAttribute('stroke', '#000'));
  for (const im of [...c.querySelectorAll('image')]) {
    const data = await imgToDataURL(im.getAttribute('href'));
    if (data) im.setAttribute('href', data); else im.remove();
  }
  const svgUrl = URL.createObjectURL(new Blob([new XMLSerializer().serializeToString(c)], { type: 'image/svg+xml;charset=utf-8' }));
  const img = new Image();
  await new Promise((res, rej) => { img.onload = res; img.onerror = rej; img.src = svgUrl; });
  const cv = document.createElement('canvas'), scale = 2;
  cv.width = img.width * scale; cv.height = img.height * scale;
  cv.getContext('2d').drawImage(img, 0, 0, cv.width, cv.height);
  URL.revokeObjectURL(svgUrl);
  cv.toBlob(blob => {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob); a.download = filename; a.click();
    URL.revokeObjectURL(a.href);
  }, 'image/png');
}

/* ── BANDES DE RISQUE (%) — vue "Prochain jour" ── */
const RISK_BANDS = [
  { k:'r1', label:'< 15 %',    color:'#fee2e2' },
  { k:'r2', label:'15 – 30 %', color:'#fca5a5' },
  { k:'r3', label:'30 – 50 %', color:'#f87171' },
  { k:'r4', label:'50 – 70 %', color:'#ef4444' },
  { k:'r5', label:'70 – 90 %', color:'#b91c1c' },
  { k:'r6', label:'> 90 %',    color:'#7f1d1d' }
];
const RISK_MAP = Object.fromEntries(RISK_BANDS.map(b => [b.k, b]));

/* ── NIVEAUX ── */
/* Tracé des 22 anciennes régions métropolitaines (avant le redécoupage de 2015), simplifié et
   embarqué directement (pas de dépendance réseau supplémentaire) — source IGN / gregoiredavid/france-geojson. */
const REGIONS_OLD_GEOJSON = {"type":"FeatureCollection","features":[{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[3.4852,48.8519],[3.3971,48.7614],[3.4674,48.7386],[3.4427,48.6725],[3.5086,48.6056],[3.4838,48.5471],[3.3841,48.478],[3.4148,48.3903],[3.3612,48.3726],[3.0463,48.3573],[3.0157,48.3073],[3.0437,48.2718],[3.006,48.2094],[2.9363,48.1634],[2.8024,48.1315],[2.599,48.1316],[2.5063,48.1564],[2.5062,48.2385],[2.4217,48.2639],[2.4208,48.2993],[2.2071,48.3449],[2.1616,48.2984],[1.979,48.288],[1.9772,48.3992],[1.9045,48.4403],[1.8031,48.4666],[1.7872,48.5537],[1.702,48.585],[1.5822,48.6961],[1.583,48.8575],[1.5015,48.9411],[1.4868,49.0518],[1.6201,49.097],[1.7044,49.2322],[1.7427,49.1798],[1.8826,49.1625],[1.9994,49.1756],[2.0767,49.2084],[2.2416,49.1516],[2.3218,49.1844],[2.4588,49.1395],[2.5905,49.0797],[2.6317,49.1051],[2.7031,49.0647],[2.7917,49.0901],[3.0617,49.0865],[3.1652,49.0997],[3.1716,49.0125],[3.2695,48.9375],[3.3617,48.9196],[3.3833,48.8725],[3.4852,48.8519]]]},"properties":{"nom":"Île-de-France"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[3.4148,48.3903],[3.3841,48.478],[3.4838,48.5471],[3.5086,48.6056],[3.4427,48.6725],[3.4674,48.7386],[3.3971,48.7614],[3.4852,48.8519],[3.5286,48.9121],[3.6212,48.966],[3.65,49.0414],[3.6245,49.1514],[3.7487,49.1585],[3.6767,49.2073],[3.6476,49.316],[3.8031,49.3588],[3.9248,49.4077],[4.0355,49.3599],[4.0409,49.5084],[4.0768,49.5707],[4.0263,49.6199],[4.1157,49.6333],[4.1265,49.6782],[4.2254,49.7273],[4.2063,49.7796],[4.2557,49.904],[4.2331,49.9578],[4.3106,49.9686],[4.4458,49.9372],[4.596,49.986],[4.6893,49.9958],[4.7001,50.0926],[4.8025,50.1511],[4.8749,50.1534],[4.8724,50.0922],[4.7909,49.9584],[4.8829,49.8984],[4.8515,49.8627],[4.9314,49.7867],[5.0897,49.7648],[5.1662,49.6929],[5.2684,49.696],[5.3935,49.6171],[5.2627,49.5418],[5.2141,49.573],[5.0977,49.5336],[5.06,49.5012],[5.1099,49.4563],[5.0886,49.3696],[5.0271,49.3356],[5.0566,49.2934],[4.968,49.2488],[4.9417,49.1838],[5.0329,49.0257],[5.0309,48.9543],[4.9136,48.8977],[4.9355,48.8405],[4.8888,48.8172],[4.9896,48.742],[5.0334,48.6138],[5.2726,48.5143],[5.3981,48.4732],[5.4471,48.415],[5.4098,48.3926],[5.5335,48.3251],[5.6409,48.2424],[5.7108,48.2199],[5.6329,48.0844],[5.6921,48.0755],[5.7764,48.0224],[5.8847,47.926],[5.8056,47.8473],[5.7612,47.8593],[5.6766,47.7792],[5.692,47.6872],[5.5968,47.6716],[5.4828,47.6846],[5.4065,47.6736],[5.3741,47.6045],[5.2374,47.6203],[4.9714,47.6902],[4.959,47.7619],[4.9861,47.8036],[4.9541,47.8668],[4.7894,48.0079],[4.6122,48.0301],[4.56,47.9714],[4.447,47.9562],[4.3128,47.9625],[4.2934,47.9257],[4.1673,47.96],[4.0946,47.9283],[3.9024,47.9383],[3.9135,47.9721],[3.822,48.0439],[3.8057,48.1005],[3.7455,48.1675],[3.6679,48.1392],[3.6152,48.2725],[3.5044,48.3655],[3.4148,48.3903]]]},"properties":{"nom":"Champagne-Ardenne"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[3.4852,48.8519],[3.3833,48.8725],[3.3617,48.9196],[3.2695,48.9375],[3.1716,49.0125],[3.1652,49.0997],[3.0617,49.0865],[2.7917,49.0901],[2.7031,49.0647],[2.6317,49.1051],[2.5905,49.0797],[2.4588,49.1395],[2.3218,49.1844],[2.2416,49.1516],[2.0767,49.2084],[1.9994,49.1756],[1.8826,49.1625],[1.7427,49.1798],[1.7044,49.2322],[1.7724,49.2937],[1.7261,49.4381],[1.7681,49.512],[1.6937,49.6011],[1.7241,49.6718],[1.6886,49.6937],[1.7592,49.7804],[1.6785,49.9181],[1.4523,50.0416],[1.3797,50.065],[1.4513,50.1071],[1.5056,50.1966],[1.617,50.2194],[1.5383,50.2803],[1.5545,50.3586],[1.6415,50.3522],[1.6676,50.3307],[1.8047,50.3596],[1.9456,50.2877],[2.0778,50.2487],[2.0966,50.2066],[2.3617,50.2071],[2.4521,50.2302],[2.3755,50.1093],[2.5149,50.1408],[2.6916,50.092],[2.7809,50.1112],[2.917,50.0355],[3.012,50.058],[3.1236,50.0245],[3.2804,50.0152],[3.3531,50.0357],[3.4901,50.019],[3.5418,50.052],[3.6141,50.0251],[3.7152,50.0693],[3.9498,50.0269],[4.0114,49.9861],[4.1409,49.9788],[4.2331,49.9578],[4.2557,49.904],[4.2063,49.7796],[4.2254,49.7273],[4.1265,49.6782],[4.1157,49.6333],[4.0263,49.6199],[4.0768,49.5707],[4.0409,49.5084],[4.0355,49.3599],[3.9248,49.4077],[3.8031,49.3588],[3.6476,49.316],[3.6767,49.2073],[3.7487,49.1585],[3.6245,49.1514],[3.65,49.0414],[3.6212,48.966],[3.5286,48.9121],[3.4852,48.8519]]]},"properties":{"nom":"Picardie"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[1.7044,49.2322],[1.6201,49.097],[1.4868,49.0518],[1.5015,48.9411],[1.4642,48.8761],[1.3598,48.8312],[1.3319,48.7622],[1.1583,48.7695],[1.063,48.759],[1.011,48.7281],[0.8148,48.6702],[0.7305,48.7856],[0.6079,48.8325],[0.6089,48.8757],[0.4574,48.8798],[0.3861,48.9105],[0.4461,49.019],[0.3789,49.0707],[0.4324,49.1422],[0.3936,49.2067],[0.3375,49.2312],[0.2972,49.4299],[0.1143,49.4706],[0.0663,49.5154],[0.2052,49.713],[0.3204,49.7415],[0.5814,49.852],[0.7639,49.8717],[1.0218,49.9163],[1.2024,49.9718],[1.3797,50.065],[1.4523,50.0416],[1.6785,49.9181],[1.7592,49.7804],[1.6886,49.6937],[1.7241,49.6718],[1.6937,49.6011],[1.7681,49.512],[1.7261,49.4381],[1.7724,49.2937],[1.7044,49.2322]]]},"properties":{"nom":"Haute-Normandie"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[1.5015,48.9411],[1.583,48.8575],[1.5822,48.6961],[1.702,48.585],[1.7872,48.5537],[1.8031,48.4666],[1.9045,48.4403],[1.9772,48.3992],[1.979,48.288],[2.1616,48.2984],[2.2071,48.3449],[2.4208,48.2993],[2.4217,48.2639],[2.5062,48.2385],[2.5063,48.1564],[2.599,48.1316],[2.8024,48.1315],[2.9363,48.1634],[3.0132,48.1435],[3.0903,48.0539],[3.1281,47.9725],[3.0118,47.9048],[3.0238,47.7864],[2.8565,47.7617],[2.849,47.7169],[2.9242,47.6825],[2.9718,47.5679],[2.8469,47.5415],[2.9319,47.4368],[2.8702,47.3423],[2.9817,47.2625],[3.0283,47.1284],[3.0222,47.0637],[3.0748,47.03],[3.0783,46.9534],[3.0502,46.9092],[3.0694,46.8523],[3.0321,46.7949],[2.9599,46.8039],[2.8275,46.7353],[2.7306,46.748],[2.5963,46.6367],[2.6133,46.5546],[2.5367,46.5197],[2.4829,46.5327],[2.3637,46.5172],[2.2847,46.4514],[2.281,46.4204],[2.0725,46.4201],[1.8185,46.4314],[1.7476,46.45],[1.6411,46.3856],[1.5254,46.4267],[1.4152,46.3472],[1.3594,46.3977],[1.3105,46.3744],[1.1773,46.384],[1.1491,46.5022],[1.0901,46.5376],[1.0204,46.5371],[0.894,46.6287],[0.9271,46.6936],[0.8134,46.7917],[0.808,46.8291],[0.7047,46.9029],[0.6902,46.975],[0.5026,46.9578],[0.4387,46.9296],[0.3106,46.9397],[0.2984,47.054],[0.0779,47.1271],[0.0537,47.1646],[0.0824,47.2869],[0.1821,47.3819],[0.1809,47.4532],[0.2201,47.502],[0.2001,47.5432],[0.23,47.6084],[0.3221,47.5952],[0.5889,47.6701],[0.5803,47.7124],[0.7684,47.8311],[0.7598,47.8981],[0.8456,47.9539],[0.8204,47.988],[0.843,48.0726],[0.8406,48.1655],[0.7977,48.1945],[0.8294,48.2114],[0.7573,48.3008],[0.7854,48.3404],[0.9073,48.3701],[0.9732,48.4362],[0.9403,48.4602],[0.9628,48.5242],[0.8493,48.5859],[0.8148,48.6702],[1.011,48.7281],[1.063,48.759],[1.1583,48.7695],[1.3319,48.7622],[1.3598,48.8312],[1.4642,48.8761],[1.5015,48.9411]]]},"properties":{"nom":"Centre"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[0.2972,49.4299],[0.3375,49.2312],[0.3936,49.2067],[0.4324,49.1422],[0.3789,49.0707],[0.4461,49.019],[0.3861,48.9105],[0.4574,48.8798],[0.6089,48.8757],[0.6079,48.8325],[0.7305,48.7856],[0.8148,48.6702],[0.8493,48.5859],[0.9628,48.5242],[0.9403,48.4602],[0.9732,48.4362],[0.9073,48.3701],[0.7854,48.3404],[0.7573,48.3008],[0.8294,48.2114],[0.7977,48.1945],[0.7386,48.1892],[0.6525,48.2634],[0.5468,48.2502],[0.3839,48.3329],[0.3632,48.4516],[0.2668,48.4846],[0.1116,48.4323],[0.0231,48.3802],[-0.0514,48.3782],[-0.0498,48.4472],[-0.1489,48.4588],[-0.2073,48.5625],[-0.3538,48.4965],[-0.5093,48.5088],[-0.5532,48.473],[-0.7565,48.4369],[-0.8458,48.4978],[-0.9539,48.5168],[-1.003,48.4891],[-1.0702,48.5085],[-1.2542,48.5433],[-1.279,48.5092],[-1.383,48.4569],[-1.4899,48.4894],[-1.5711,48.6264],[-1.4487,48.6235],[-1.4007,48.6591],[-1.5049,48.6874],[-1.5746,48.7542],[-1.5846,48.8494],[-1.5596,48.9127],[-1.5561,49.0249],[-1.6102,49.0836],[-1.5948,49.1408],[-1.6986,49.3057],[-1.8089,49.3739],[-1.8497,49.5067],[-1.8605,49.6503],[-1.9455,49.6775],[-1.9439,49.7224],[-1.6236,49.6468],[-1.5311,49.6553],[-1.427,49.7019],[-1.2707,49.6816],[-1.2302,49.6255],[-1.3096,49.5484],[-1.1709,49.4121],[-1.0739,49.3895],[-0.9253,49.3925],[-0.8323,49.3587],[-0.6252,49.3406],[-0.4026,49.3338],[-0.2257,49.2818],[-0.0925,49.2982],[0.0106,49.3306],[0.1279,49.4021],[0.2972,49.4299]]]},"properties":{"nom":"Basse-Normandie"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[2.9363,48.1634],[3.006,48.2094],[3.0437,48.2718],[3.0157,48.3073],[3.0463,48.3573],[3.3612,48.3726],[3.4148,48.3903],[3.5044,48.3655],[3.6152,48.2725],[3.6679,48.1392],[3.7455,48.1675],[3.8057,48.1005],[3.822,48.0439],[3.9135,47.9721],[3.9024,47.9383],[4.0946,47.9283],[4.1673,47.96],[4.2934,47.9257],[4.3128,47.9625],[4.447,47.9562],[4.56,47.9714],[4.6122,48.0301],[4.7894,48.0079],[4.9541,47.8668],[4.9861,47.8036],[4.959,47.7619],[4.9714,47.6902],[5.2374,47.6203],[5.3741,47.6045],[5.4786,47.6054],[5.4966,47.5471],[5.4364,47.4904],[5.4385,47.4442],[5.4947,47.3381],[5.4742,47.2131],[5.3776,47.0793],[5.2835,47.0463],[5.2627,46.9377],[5.4632,46.8286],[5.3286,46.8129],[5.3901,46.7672],[5.4406,46.6379],[5.3629,46.5179],[5.42,46.4809],[5.3106,46.4468],[5.2151,46.4684],[5.2012,46.5078],[5.0548,46.4843],[4.9802,46.5152],[4.9159,46.4882],[4.8875,46.4027],[4.8113,46.2606],[4.7362,46.2332],[4.6387,46.3014],[4.5865,46.2684],[4.504,46.2671],[4.4106,46.2953],[4.3884,46.2131],[4.2824,46.1568],[4.2062,46.1945],[4.1038,46.1984],[4.0275,46.1696],[3.8901,46.2145],[3.8995,46.2759],[3.9857,46.3175],[3.9776,46.3979],[4.001,46.462],[3.958,46.4905],[3.8653,46.4896],[3.7327,46.5477],[3.7346,46.6038],[3.6228,46.7409],[3.4742,46.6535],[3.4342,46.7122],[3.3181,46.6884],[3.2685,46.7159],[3.2044,46.6788],[3.0496,46.7577],[3.0321,46.7949],[3.0694,46.8523],[3.0502,46.9092],[3.0783,46.9534],[3.0748,47.03],[3.0222,47.0637],[3.0283,47.1284],[2.9817,47.2625],[2.8702,47.3423],[2.9319,47.4368],[2.8469,47.5415],[2.9718,47.5679],[2.9242,47.6825],[2.849,47.7169],[2.8565,47.7617],[3.0238,47.7864],[3.0118,47.9048],[3.1281,47.9725],[3.0903,48.0539],[3.0132,48.1435],[2.9363,48.1634]]]},"properties":{"nom":"Bourgogne"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[4.1409,49.9788],[4.0114,49.9861],[3.9498,50.0269],[3.7152,50.0693],[3.6141,50.0251],[3.5418,50.052],[3.4901,50.019],[3.3531,50.0357],[3.2804,50.0152],[3.1236,50.0245],[3.012,50.058],[2.917,50.0355],[2.7809,50.1112],[2.6916,50.092],[2.5149,50.1408],[2.3755,50.1093],[2.4521,50.2302],[2.3617,50.2071],[2.0966,50.2066],[2.0778,50.2487],[1.9456,50.2877],[1.8047,50.3596],[1.6676,50.3307],[1.6415,50.3522],[1.556,50.3988],[1.5855,50.536],[1.5614,50.6994],[1.605,50.7655],[1.5785,50.8552],[1.7253,50.9363],[1.9424,50.9881],[2.1129,51.005],[2.1465,51.0269],[2.3504,51.0602],[2.3839,51.049],[2.546,51.0894],[2.635,50.8128],[2.7198,50.8121],[2.8133,50.7169],[2.9004,50.6933],[2.9521,50.7518],[3.1471,50.7899],[3.2618,50.676],[3.2865,50.5276],[3.3773,50.491],[3.475,50.5329],[3.4978,50.4978],[3.6079,50.4965],[3.661,50.4576],[3.665,50.3488],[3.8545,50.3505],[3.8861,50.3271],[4.0252,50.3579],[4.2208,50.2543],[4.1664,50.2151],[4.1532,50.1604],[4.2282,50.0667],[4.1356,50.0186],[4.1409,49.9788]]]},"properties":{"nom":"Nord-Pas-de-Calais"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[5.8847,47.926],[5.7764,48.0224],[5.6921,48.0755],[5.6329,48.0844],[5.7108,48.2199],[5.6409,48.2424],[5.5335,48.3251],[5.4098,48.3926],[5.4471,48.415],[5.3981,48.4732],[5.2726,48.5143],[5.0334,48.6138],[4.9896,48.742],[4.8888,48.8172],[4.9355,48.8405],[4.9136,48.8977],[5.0309,48.9543],[5.0329,49.0257],[4.9417,49.1838],[4.968,49.2488],[5.0566,49.2934],[5.0271,49.3356],[5.0886,49.3696],[5.1099,49.4563],[5.06,49.5012],[5.0977,49.5336],[5.2141,49.573],[5.2627,49.5418],[5.3935,49.6171],[5.4472,49.5179],[5.5938,49.5214],[5.6608,49.5524],[5.8362,49.5418],[5.864,49.5013],[5.9454,49.4996],[6.0422,49.4478],[6.1568,49.5028],[6.2762,49.5035],[6.3338,49.4669],[6.4682,49.4647],[6.5354,49.4342],[6.5654,49.3493],[6.6678,49.2804],[6.7302,49.1692],[6.8345,49.1514],[6.9348,49.2221],[7.0337,49.1883],[7.1588,49.1208],[7.2934,49.1152],[7.3662,49.172],[7.4913,49.1685],[7.5312,49.0971],[7.6353,49.0542],[7.5799,48.9616],[7.5363,48.9335],[7.4475,48.966],[7.3269,48.9434],[7.2939,48.9726],[7.1736,49.0061],[7.1064,49.0453],[7.0549,49.0306],[7.0322,48.956],[6.9575,48.8942],[7.055,48.8649],[7.0751,48.788],[7.1489,48.8446],[7.2909,48.7939],[7.2621,48.6978],[7.2563,48.5887],[7.1188,48.4837],[7.076,48.3526],[7.1665,48.3393],[7.1936,48.302],[7.1433,48.2584],[7.059,48.1392],[7.0842,48.1292],[7.0026,48.0233],[6.9435,47.9987],[6.8983,47.8887],[6.92,47.8503],[6.8462,47.8229],[6.6452,47.9041],[6.606,47.9445],[6.4783,47.8854],[6.3884,47.96],[6.1821,47.9569],[6.1314,48.0241],[6.0369,48.0015],[6.0023,47.9561],[5.9192,47.9679],[5.8847,47.926]]]},"properties":{"nom":"Lorraine"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[6.8462,47.8229],[6.92,47.8503],[6.8983,47.8887],[6.9435,47.9987],[7.0026,48.0233],[7.0842,48.1292],[7.059,48.1392],[7.1433,48.2584],[7.1936,48.302],[7.1665,48.3393],[7.076,48.3526],[7.1188,48.4837],[7.2563,48.5887],[7.2621,48.6978],[7.2909,48.7939],[7.1489,48.8446],[7.0751,48.788],[7.055,48.8649],[6.9575,48.8942],[7.0322,48.956],[7.0549,49.0306],[7.1064,49.0453],[7.1736,49.0061],[7.2939,48.9726],[7.3269,48.9434],[7.4475,48.966],[7.5363,48.9335],[7.5799,48.9616],[7.6353,49.0542],[7.8674,49.0335],[7.937,49.0562],[8.0914,48.9893],[8.2214,48.9752],[8.1416,48.8961],[8.1065,48.8251],[7.893,48.6646],[7.8002,48.5832],[7.8052,48.5135],[7.7684,48.4899],[7.7335,48.3987],[7.7446,48.3369],[7.6939,48.3021],[7.6662,48.2211],[7.5995,48.1556],[7.5686,48.0363],[7.6222,47.9737],[7.5562,47.8776],[7.5302,47.7834],[7.5483,47.7387],[7.5218,47.663],[7.593,47.6008],[7.5095,47.4996],[7.3863,47.432],[7.2455,47.4202],[7.1722,47.4455],[7.1303,47.503],[7.0748,47.5989],[7.0067,47.6015],[7.0453,47.6704],[7.0117,47.7416],[6.865,47.7846],[6.8462,47.8229]]]},"properties":{"nom":"Alsace"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[5.3741,47.6045],[5.4065,47.6736],[5.4828,47.6846],[5.5968,47.6716],[5.692,47.6872],[5.6766,47.7792],[5.7612,47.8593],[5.8056,47.8473],[5.8847,47.926],[5.9192,47.9679],[6.0023,47.9561],[6.0369,48.0015],[6.1314,48.0241],[6.1821,47.9569],[6.3884,47.96],[6.4783,47.8854],[6.606,47.9445],[6.6452,47.9041],[6.8462,47.8229],[6.865,47.7846],[7.0117,47.7416],[7.0453,47.6704],[7.0067,47.6015],[7.0748,47.5989],[7.1303,47.503],[6.9829,47.4943],[6.9899,47.4481],[6.8854,47.3745],[6.9205,47.3556],[7.0482,47.3605],[6.9551,47.2432],[6.7406,47.107],[6.6339,46.9986],[6.506,46.9661],[6.433,46.9278],[6.4645,46.8899],[6.4286,46.7562],[6.2609,46.672],[6.113,46.5751],[6.1555,46.5456],[6.0738,46.4644],[6.064,46.4162],[5.8692,46.265],[5.7215,46.2632],[5.7146,46.3081],[5.6351,46.3333],[5.5393,46.2689],[5.4754,46.2652],[5.4371,46.3151],[5.3091,46.4103],[5.3106,46.4468],[5.42,46.4809],[5.3629,46.5179],[5.4406,46.6379],[5.3901,46.7672],[5.3286,46.8129],[5.4632,46.8286],[5.2627,46.9377],[5.2835,47.0463],[5.3776,47.0793],[5.4742,47.2131],[5.4947,47.3381],[5.4385,47.4442],[5.4364,47.4904],[5.4966,47.5471],[5.4786,47.6054],[5.3741,47.6045]]]},"properties":{"nom":"Franche-Comté"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[0.7977,48.1945],[0.8406,48.1655],[0.843,48.0726],[0.8204,47.988],[0.8456,47.9539],[0.7598,47.8981],[0.7684,47.8311],[0.5803,47.7124],[0.5889,47.6701],[0.3221,47.5952],[0.23,47.6084],[0.2001,47.5432],[0.2201,47.502],[0.1809,47.4532],[0.1821,47.3819],[0.0824,47.2869],[0.0537,47.1646],[-0.0117,47.1562],[-0.0442,47.0932],[-0.2404,47.1051],[-0.4008,47.0708],[-0.5151,47.0779],[-0.6007,46.9975],[-0.7145,46.986],[-0.7869,47.0051],[-0.8778,46.9591],[-0.8306,46.9311],[-0.7778,46.841],[-0.6991,46.8089],[-0.7266,46.7688],[-0.6558,46.7003],[-0.614,46.6201],[-0.6021,46.5333],[-0.6444,46.5109],[-0.6082,46.4542],[-0.6028,46.3596],[-0.6556,46.3158],[-0.7505,46.3043],[-0.8364,46.3414],[-0.9339,46.3126],[-0.9783,46.3512],[-1.1294,46.3103],[-1.2237,46.2749],[-1.3647,46.3479],[-1.4654,46.3428],[-1.5052,46.3994],[-1.6193,46.413],[-1.7827,46.4943],[-1.8126,46.4943],[-1.8561,46.6084],[-1.9417,46.6917],[-2.142,46.819],[-2.1546,46.8882],[-1.9804,47.0289],[-2.0537,47.0942],[-2.2438,47.1352],[-2.1666,47.1674],[-2.1685,47.2678],[-2.2946,47.2347],[-2.362,47.2746],[-2.4212,47.2591],[-2.5015,47.3177],[-2.5259,47.3626],[-2.4585,47.4481],[-2.3161,47.4625],[-2.0989,47.5334],[-2.097,47.6314],[-1.9771,47.6917],[-1.8583,47.7083],[-1.7297,47.6993],[-1.6381,47.7222],[-1.6196,47.7636],[-1.3812,47.8214],[-1.2459,47.7767],[-1.1892,47.8677],[-1.1525,47.9663],[-1.0217,47.9954],[-1.0987,48.2513],[-1.0458,48.3297],[-1.0795,48.4139],[-1.0702,48.5085],[-1.003,48.4891],[-0.9539,48.5168],[-0.8458,48.4978],[-0.7565,48.4369],[-0.5532,48.473],[-0.5093,48.5088],[-0.3538,48.4965],[-0.2073,48.5625],[-0.1489,48.4588],[-0.0498,48.4472],[-0.0514,48.3782],[0.0231,48.3802],[0.1116,48.4323],[0.2668,48.4846],[0.3632,48.4516],[0.3839,48.3329],[0.5468,48.2502],[0.6525,48.2634],[0.7386,48.1892],[0.7977,48.1945]]]},"properties":{"nom":"Pays de la Loire"}},{"type":"Feature","geometry":{"type":"MultiPolygon","coordinates":[[[[-1.5711,48.6264],[-1.4899,48.4894],[-1.383,48.4569],[-1.279,48.5092],[-1.2542,48.5433],[-1.0702,48.5085],[-1.0795,48.4139],[-1.0458,48.3297],[-1.0987,48.2513],[-1.0217,47.9954],[-1.1525,47.9663],[-1.1892,47.8677],[-1.2459,47.7767],[-1.3812,47.8214],[-1.6196,47.7636],[-1.6381,47.7222],[-1.7297,47.6993],[-1.8583,47.7083],[-1.9771,47.6917],[-2.097,47.6314],[-2.0989,47.5334],[-2.3161,47.4625],[-2.4585,47.4481],[-2.4668,47.5119],[-2.5353,47.5258],[-2.6791,47.4945],[-2.8194,47.4886],[-2.8694,47.5504],[-2.7347,47.5416],[-2.7228,47.602],[-2.8112,47.6212],[-2.9737,47.5722],[-3.0548,47.5714],[-3.1967,47.624],[-3.2879,47.701],[-3.3427,47.7159],[-3.4567,47.6978],[-3.5263,47.7645],[-3.7987,47.7889],[-3.8555,47.8034],[-3.913,47.8681],[-4.0393,47.848],[-4.0807,47.8709],[-4.1639,47.8486],[-4.181,47.8027],[-4.3714,47.7981],[-4.3466,47.8425],[-4.4207,47.9606],[-4.5371,48.0162],[-4.633,48.0294],[-4.6215,48.0684],[-4.3639,48.1084],[-4.2696,48.1334],[-4.3164,48.2038],[-4.6083,48.2608],[-4.5531,48.2946],[-4.3672,48.2777],[-4.3786,48.3268],[-4.4507,48.3289],[-4.4336,48.3964],[-4.6267,48.3375],[-4.6789,48.3555],[-4.7645,48.3282],[-4.7935,48.4159],[-4.7773,48.4985],[-4.6999,48.5696],[-4.6002,48.572],[-4.5639,48.6211],[-4.4628,48.6271],[-4.3491,48.6765],[-4.2451,48.6498],[-4.133,48.6953],[-3.9736,48.7039],[-3.8976,48.6468],[-3.838,48.7063],[-3.7222,48.7055],[-3.6227,48.6848],[-3.5494,48.7457],[-3.5853,48.7714],[-3.4798,48.8358],[-3.4406,48.7983],[-3.3202,48.8372],[-3.1991,48.8246],[-3.1,48.868],[-3.0788,48.8309],[-2.9506,48.7659],[-2.9473,48.7237],[-2.8294,48.6558],[-2.8241,48.5992],[-2.6506,48.5246],[-2.5487,48.5971],[-2.409,48.6359],[-2.3142,48.6826],[-2.2054,48.5789],[-2.1492,48.629],[-2.0502,48.6359],[-1.9337,48.6972],[-1.8473,48.6942],[-1.8706,48.6437],[-1.7677,48.6024],[-1.5711,48.6264]]],[[[-3.2437,47.387],[-3.1558,47.3608],[-3.0902,47.281],[-3.2167,47.2943],[-3.2609,47.3569],[-3.2437,47.387]]]]},"properties":{"nom":"Bretagne"}},{"type":"Feature","geometry":{"type":"MultiPolygon","coordinates":[[[[0.0537,47.1646],[0.0779,47.1271],[0.2984,47.054],[0.3106,46.9397],[0.4387,46.9296],[0.5026,46.9578],[0.6902,46.975],[0.7047,46.9029],[0.808,46.8291],[0.8134,46.7917],[0.9271,46.6936],[0.894,46.6287],[1.0204,46.5371],[1.0901,46.5376],[1.1491,46.5022],[1.1773,46.384],[1.0272,46.3434],[1.0059,46.281],[0.9012,46.2875],[0.8133,46.1977],[0.8459,46.1384],[0.8179,46.0479],[0.9238,46.0083],[0.9409,45.9592],[0.8274,45.8827],[0.7702,45.7885],[0.6297,45.7146],[0.6004,45.6717],[0.5038,45.6126],[0.5065,45.5543],[0.4328,45.5016],[0.3108,45.4588],[0.2494,45.3636],[0.2706,45.3146],[0.1455,45.2145],[0.0512,45.2254],[-0.0367,45.1406],[-0.1435,45.0904],[-0.2568,45.1148],[-0.2757,45.1416],[-0.4195,45.207],[-0.4185,45.2736],[-0.5677,45.2974],[-0.569,45.3312],[-0.7149,45.3277],[-0.7532,45.4225],[-0.856,45.5136],[-0.9678,45.5608],[-1.0134,45.6023],[-1.2097,45.6956],[-1.241,45.7856],[-1.1329,45.8039],[-1.0675,45.9498],[-1.0923,45.9934],[-1.0656,46.0493],[-1.1295,46.1263],[-1.2263,46.1503],[-1.1997,46.2126],[-1.1113,46.2612],[-1.1294,46.3103],[-0.9783,46.3512],[-0.9339,46.3126],[-0.8364,46.3414],[-0.7505,46.3043],[-0.6556,46.3158],[-0.6028,46.3596],[-0.6082,46.4542],[-0.6444,46.5109],[-0.6021,46.5333],[-0.614,46.6201],[-0.6558,46.7003],[-0.7266,46.7688],[-0.6991,46.8089],[-0.7778,46.841],[-0.8306,46.9311],[-0.8778,46.9591],[-0.7869,47.0051],[-0.7145,46.986],[-0.6007,46.9975],[-0.5151,47.0779],[-0.4008,47.0708],[-0.2404,47.1051],[-0.0442,47.0932],[-0.0117,47.1562],[0.0537,47.1646]]],[[[-1.3012,45.8999],[-1.3865,45.9533],[-1.3693,46.0296],[-1.247,45.9897],[-1.2334,45.9272],[-1.189,45.8858],[-1.1976,45.8305],[-1.3012,45.8999]]],[[[-1.4118,46.1842],[-1.5344,46.2036],[-1.513,46.2578],[-1.4248,46.2052],[-1.2957,46.1882],[-1.308,46.143],[-1.4118,46.1842]]]]},"properties":{"nom":"Poitou-Charentes"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[-0.7149,45.3277],[-0.569,45.3312],[-0.5677,45.2974],[-0.4185,45.2736],[-0.4195,45.207],[-0.2757,45.1416],[-0.2568,45.1148],[-0.1435,45.0904],[-0.0367,45.1406],[0.0512,45.2254],[0.1455,45.2145],[0.2706,45.3146],[0.2494,45.3636],[0.3108,45.4588],[0.4328,45.5016],[0.5065,45.5543],[0.5038,45.6126],[0.6004,45.6717],[0.6297,45.7146],[0.6621,45.6875],[0.7756,45.6677],[0.7506,45.6169],[0.84,45.5813],[0.9958,45.6127],[1.1851,45.4551],[1.2883,45.4335],[1.2852,45.3521],[1.2271,45.2718],[1.2336,45.2222],[1.2857,45.176],[1.4131,45.1249],[1.3996,45.0612],[1.4483,45.0193],[1.4091,45.0067],[1.4416,44.9188],[1.4168,44.8709],[1.3012,44.7977],[1.3161,44.7404],[1.1526,44.6362],[1.1032,44.5717],[0.9815,44.5444],[1.0575,44.4277],[1.0605,44.3663],[0.9505,44.3595],[0.8749,44.3207],[0.9488,44.2721],[0.8691,44.1265],[0.7966,44.1451],[0.7389,44.0625],[0.6625,44.0246],[0.6225,44.0642],[0.4598,44.0554],[0.4425,44.0288],[0.3028,43.9914],[0.2147,44.0228],[0.1641,43.9771],[0.076,43.9831],[0.0751,43.9147],[0.0292,43.902],[-0.0736,43.9446],[-0.179,43.9365],[-0.1941,43.737],[-0.2474,43.7092],[-0.2747,43.616],[-0.2389,43.583],[-0.0968,43.5824],[-0.0401,43.5126],[-0.0696,43.4342],[0.0288,43.3467],[-0.0446,43.3028],[-0.0184,43.2693],[-0.1975,43.0998],[-0.1857,43.0518],[-0.2877,43.0056],[-0.2792,42.9391],[-0.3243,42.9033],[-0.3134,42.8494],[-0.3889,42.8005],[-0.5269,42.7946],[-0.6794,42.8829],[-0.7226,42.8897],[-0.7528,42.967],[-0.8119,42.9513],[-0.9431,42.9539],[-1.1124,43.0216],[-1.2475,43.0424],[-1.3452,43.0931],[-1.3566,43.029],[-1.4397,43.046],[-1.4273,43.1173],[-1.385,43.1881],[-1.3856,43.255],[-1.6696,43.3143],[-1.7312,43.2987],[-1.7792,43.3711],[-1.6443,43.4067],[-1.6005,43.4347],[-1.4933,43.5705],[-1.4467,43.6475],[-1.3164,44.1288],[-1.2753,44.3324],[-1.251,44.5063],[-1.2598,44.5444],[-1.1938,44.6582],[-1.0843,44.6412],[-1.0378,44.6955],[-1.1619,44.7749],[-1.2613,44.6495],[-1.2149,44.9173],[-1.1645,45.2851],[-1.1544,45.4812],[-1.0684,45.515],[-0.8033,45.344],[-0.747,45.2268],[-0.6901,45.2364],[-0.7149,45.3277]]]},"properties":{"nom":"Aquitaine"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[-0.3134,42.8494],[-0.3243,42.9033],[-0.2792,42.9391],[-0.2877,43.0056],[-0.1857,43.0518],[-0.1975,43.0998],[-0.0184,43.2693],[-0.0446,43.3028],[0.0288,43.3467],[-0.0696,43.4342],[-0.0401,43.5126],[-0.0968,43.5824],[-0.2389,43.583],[-0.2747,43.616],[-0.2474,43.7092],[-0.1941,43.737],[-0.179,43.9365],[-0.0736,43.9446],[0.0292,43.902],[0.0751,43.9147],[0.076,43.9831],[0.1641,43.9771],[0.2147,44.0228],[0.3028,43.9914],[0.4425,44.0288],[0.4598,44.0554],[0.6225,44.0642],[0.6625,44.0246],[0.7389,44.0625],[0.7966,44.1451],[0.8691,44.1265],[0.9488,44.2721],[0.8749,44.3207],[0.9505,44.3595],[1.0605,44.3663],[1.0575,44.4277],[0.9815,44.5444],[1.1032,44.5717],[1.1526,44.6362],[1.3161,44.7404],[1.3012,44.7977],[1.4168,44.8709],[1.4416,44.9188],[1.4091,45.0067],[1.4483,45.0193],[1.5403,45.0447],[1.6277,45.0333],[1.7488,44.9568],[1.8276,44.9307],[1.9272,44.9788],[2.0629,44.9765],[2.0845,44.8884],[2.1716,44.7903],[2.1481,44.7229],[2.1694,44.638],[2.3241,44.6692],[2.351,44.6412],[2.4847,44.6526],[2.5562,44.7222],[2.6042,44.8431],[2.6819,44.9073],[2.7559,44.9322],[2.8028,44.8738],[2.8515,44.8719],[2.9233,44.7286],[2.9817,44.6447],[3.0835,44.5603],[3.0689,44.5027],[3.1355,44.4555],[3.1342,44.3345],[3.1605,44.2463],[3.2121,44.1958],[3.3556,44.2008],[3.3238,44.1089],[3.3517,44.0503],[3.451,44.0225],[3.3517,43.9377],[3.2741,43.8985],[3.249,43.8296],[3.2053,43.813],[3.0857,43.8356],[3.0486,43.8015],[3.0607,43.6928],[2.9821,43.708],[2.9185,43.662],[2.7591,43.6149],[2.6536,43.65],[2.6156,43.6005],[2.668,43.4701],[2.5615,43.421],[2.4944,43.4369],[2.3987,43.4171],[2.2567,43.4536],[2.2225,43.4283],[2.0728,43.3957],[1.9024,43.4088],[1.8565,43.4435],[1.805,43.3598],[1.6884,43.2736],[1.7396,43.1843],[1.7838,43.1545],[1.9484,43.1203],[1.9855,43.0235],[1.985,42.8709],[1.8803,42.8527],[1.9503,42.7377],[2.0581,42.7555],[2.1755,42.6827],[2.0184,42.6537],[1.9733,42.6184],[1.8654,42.5799],[1.7861,42.5736],[1.6832,42.6245],[1.6027,42.6264],[1.5492,42.6557],[1.4797,42.6509],[1.4389,42.6043],[1.3574,42.7194],[1.165,42.7095],[1.0729,42.7828],[0.9258,42.7897],[0.8538,42.8267],[0.7084,42.8614],[0.6592,42.8387],[0.6732,42.6913],[0.5264,42.7026],[0.4257,42.6908],[0.3589,42.7209],[0.2955,42.675],[0.2599,42.7158],[0.1753,42.7358],[-0.0112,42.6847],[-0.1056,42.7222],[-0.1548,42.7824],[-0.3134,42.8494]]]},"properties":{"nom":"Midi-Pyrénées"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[1.1773,46.384],[1.3105,46.3744],[1.3594,46.3977],[1.4152,46.3472],[1.5254,46.4267],[1.6411,46.3856],[1.7476,46.45],[1.8185,46.4314],[2.0725,46.4201],[2.281,46.4204],[2.3146,46.3346],[2.3861,46.332],[2.4702,46.2863],[2.5592,46.1744],[2.5547,46.0761],[2.5992,46.0355],[2.5658,45.9569],[2.4922,45.864],[2.3893,45.824],[2.5216,45.7095],[2.5179,45.6399],[2.4635,45.5947],[2.5184,45.5189],[2.4875,45.4182],[2.4416,45.3845],[2.377,45.4143],[2.3534,45.3298],[2.2697,45.2899],[2.1925,45.2198],[2.2104,45.1472],[2.1728,45.0813],[2.0952,45.056],[2.1409,45.0054],[2.0629,44.9765],[1.9272,44.9788],[1.8276,44.9307],[1.7488,44.9568],[1.6277,45.0333],[1.5403,45.0447],[1.4483,45.0193],[1.3996,45.0612],[1.4131,45.1249],[1.2857,45.176],[1.2336,45.2222],[1.2271,45.2718],[1.2852,45.3521],[1.2883,45.4335],[1.1851,45.4551],[0.9958,45.6127],[0.84,45.5813],[0.7506,45.6169],[0.7756,45.6677],[0.6621,45.6875],[0.6297,45.7146],[0.7702,45.7885],[0.8274,45.8827],[0.9409,45.9592],[0.9238,46.0083],[0.8179,46.0479],[0.8459,46.1384],[0.8133,46.1977],[0.9012,46.2875],[1.0059,46.281],[1.0272,46.3434],[1.1773,46.384]]]},"properties":{"nom":"Limousin"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[3.8995,46.2759],[3.8901,46.2145],[4.0275,46.1696],[4.1038,46.1984],[4.2062,46.1945],[4.2824,46.1568],[4.3884,46.2131],[4.4106,46.2953],[4.504,46.2671],[4.5865,46.2684],[4.6387,46.3014],[4.7362,46.2332],[4.8113,46.2606],[4.8875,46.4027],[4.9159,46.4882],[4.9802,46.5152],[5.0548,46.4843],[5.2012,46.5078],[5.2151,46.4684],[5.3106,46.4468],[5.3091,46.4103],[5.4371,46.3151],[5.4754,46.2652],[5.5393,46.2689],[5.6351,46.3333],[5.7146,46.3081],[5.7215,46.2632],[5.8692,46.265],[6.064,46.4162],[6.1667,46.3641],[6.1024,46.2848],[6.1225,46.2533],[5.9744,46.2147],[5.9647,46.1447],[6.1266,46.1405],[6.2945,46.2249],[6.2388,46.2753],[6.2773,46.3491],[6.3491,46.3674],[6.3908,46.341],[6.5442,46.3951],[6.717,46.4079],[6.8637,46.2798],[6.8036,46.2031],[6.8152,46.1293],[6.9511,46.0496],[7.0201,45.9813],[7.0421,45.9283],[6.9929,45.8706],[6.8047,45.8167],[6.8094,45.7259],[6.9148,45.6517],[6.9991,45.6382],[6.9789,45.5886],[7,45.5048],[7.1018,45.4685],[7.1842,45.4035],[7.1107,45.3265],[7.1351,45.2546],[6.9663,45.2065],[6.8543,45.1286],[6.7695,45.1593],[6.63,45.1093],[6.5765,45.1231],[6.3969,45.062],[6.3342,45.1229],[6.2293,45.1068],[6.2031,45.0124],[6.3133,45.0039],[6.3582,44.9416],[6.3554,44.8548],[6.1304,44.8625],[5.9495,44.8045],[5.9196,44.7528],[5.848,44.7515],[5.7906,44.6533],[5.6403,44.6478],[5.5972,44.5433],[5.6332,44.5021],[5.6036,44.4655],[5.4578,44.4982],[5.4929,44.3371],[5.6132,44.3158],[5.6756,44.2759],[5.6859,44.1972],[5.5757,44.1863],[5.5421,44.1333],[5.4547,44.1192],[5.3832,44.1553],[5.3845,44.2012],[5.1756,44.2209],[5.1495,44.3007],[5.0602,44.3076],[4.8253,44.2284],[4.8046,44.3039],[4.6799,44.3207],[4.6492,44.2704],[4.5061,44.3403],[4.4036,44.2882],[4.3919,44.3466],[4.3293,44.3324],[4.2451,44.2681],[4.1429,44.3134],[4.0479,44.3344],[4.0684,44.4051],[3.9982,44.4598],[3.9488,44.5729],[3.8941,44.6153],[3.8625,44.7439],[3.9986,44.8236],[4.0389,44.8728],[4.1566,44.874],[4.2239,44.9629],[4.2921,44.9939],[4.3976,45.1181],[4.4726,45.1799],[4.4831,45.2364],[4.4197,45.2754],[4.3364,45.3662],[4.1444,45.3841],[4.0802,45.3555],[3.9783,45.3757],[3.9598,45.4342],[3.984,45.4951],[3.9045,45.5989],[3.8236,45.632],[3.7558,45.7466],[3.7004,45.7836],[3.753,45.8893],[3.694,45.9307],[3.7096,45.9741],[3.8282,45.9971],[3.8213,46.0902],[3.7901,46.1534],[3.8076,46.2574],[3.8995,46.2759]],[[4.8929,44.3648],[4.8895,44.304],[4.9836,44.295],[5.0476,44.3821],[4.9708,44.4301],[4.8929,44.3648]]]},"properties":{"nom":"Rhône-Alpes"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[2.281,46.4204],[2.2847,46.4514],[2.3637,46.5172],[2.4829,46.5327],[2.5367,46.5197],[2.6133,46.5546],[2.5963,46.6367],[2.7306,46.748],[2.8275,46.7353],[2.9599,46.8039],[3.0321,46.7949],[3.0496,46.7577],[3.2044,46.6788],[3.2685,46.7159],[3.3181,46.6884],[3.4342,46.7122],[3.4742,46.6535],[3.6228,46.7409],[3.7346,46.6038],[3.7327,46.5477],[3.8653,46.4896],[3.958,46.4905],[4.001,46.462],[3.9776,46.3979],[3.9857,46.3175],[3.8995,46.2759],[3.8076,46.2574],[3.7901,46.1534],[3.8213,46.0902],[3.8282,45.9971],[3.7096,45.9741],[3.694,45.9307],[3.753,45.8893],[3.7004,45.7836],[3.7558,45.7466],[3.8236,45.632],[3.9045,45.5989],[3.984,45.4951],[3.9598,45.4342],[3.9783,45.3757],[4.0802,45.3555],[4.1444,45.3841],[4.3364,45.3662],[4.4197,45.2754],[4.4831,45.2364],[4.4726,45.1799],[4.3976,45.1181],[4.2921,44.9939],[4.2239,44.9629],[4.1566,44.874],[4.0389,44.8728],[3.9986,44.8236],[3.8625,44.7439],[3.7439,44.8378],[3.6299,44.8798],[3.5688,44.8342],[3.4562,44.8312],[3.4032,44.9569],[3.3379,44.9559],[3.1903,44.8625],[3.1428,44.9023],[3.0479,44.8039],[2.9817,44.6447],[2.9233,44.7286],[2.8515,44.8719],[2.8028,44.8738],[2.7559,44.9322],[2.6819,44.9073],[2.6042,44.8431],[2.5562,44.7222],[2.4847,44.6526],[2.351,44.6412],[2.3241,44.6692],[2.1694,44.638],[2.1481,44.7229],[2.1716,44.7903],[2.0845,44.8884],[2.0629,44.9765],[2.1409,45.0054],[2.0952,45.056],[2.1728,45.0813],[2.2104,45.1472],[2.1925,45.2198],[2.2697,45.2899],[2.3534,45.3298],[2.377,45.4143],[2.4416,45.3845],[2.4875,45.4182],[2.5184,45.5189],[2.4635,45.5947],[2.5179,45.6399],[2.5216,45.7095],[2.3893,45.824],[2.4922,45.864],[2.5658,45.9569],[2.5992,46.0355],[2.5547,46.0761],[2.5592,46.1744],[2.4702,46.2863],[2.3861,46.332],[2.3146,46.3346],[2.281,46.4204]]]},"properties":{"nom":"Auvergne"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[1.7861,42.5736],[1.8654,42.5799],[1.9733,42.6184],[2.0184,42.6537],[2.1755,42.6827],[2.0581,42.7555],[1.9503,42.7377],[1.8803,42.8527],[1.985,42.8709],[1.9855,43.0235],[1.9484,43.1203],[1.7838,43.1545],[1.7396,43.1843],[1.6884,43.2736],[1.805,43.3598],[1.8565,43.4435],[1.9024,43.4088],[2.0728,43.3957],[2.2225,43.4283],[2.2567,43.4536],[2.3987,43.4171],[2.4944,43.4369],[2.5615,43.421],[2.668,43.4701],[2.6156,43.6005],[2.6536,43.65],[2.7591,43.6149],[2.9185,43.662],[2.9821,43.708],[3.0607,43.6928],[3.0486,43.8015],[3.0857,43.8356],[3.2053,43.813],[3.249,43.8296],[3.2741,43.8985],[3.3517,43.9377],[3.451,44.0225],[3.3517,44.0503],[3.3238,44.1089],[3.3556,44.2008],[3.2121,44.1958],[3.1605,44.2463],[3.1342,44.3345],[3.1355,44.4555],[3.0689,44.5027],[3.0835,44.5603],[2.9817,44.6447],[3.0479,44.8039],[3.1428,44.9023],[3.1903,44.8625],[3.3379,44.9559],[3.4032,44.9569],[3.4562,44.8312],[3.5688,44.8342],[3.6299,44.8798],[3.7439,44.8378],[3.8625,44.7439],[3.8941,44.6153],[3.9488,44.5729],[3.9982,44.4598],[4.0684,44.4051],[4.0479,44.3344],[4.1429,44.3134],[4.2451,44.2681],[4.3293,44.3324],[4.3919,44.3466],[4.4036,44.2882],[4.5061,44.3403],[4.6492,44.2704],[4.6999,44.2161],[4.7529,44.088],[4.8394,44.014],[4.8409,43.9859],[4.6927,43.8846],[4.6417,43.8673],[4.6502,43.7809],[4.5978,43.6868],[4.4908,43.7004],[4.4272,43.6258],[4.4046,43.5605],[4.2447,43.5013],[4.2303,43.4602],[4.1646,43.4722],[4.101,43.5544],[4.0048,43.551],[3.9038,43.5153],[3.7945,43.44],[3.6195,43.3676],[3.5078,43.2724],[3.4004,43.2873],[3.2677,43.2313],[3.1487,43.1393],[3.0845,43.0562],[3.0428,42.9602],[3.0598,42.9179],[3.0435,42.8382],[3.0355,42.6442],[3.0583,42.5367],[3.1336,42.5173],[3.1741,42.4352],[3.0812,42.4282],[3.0404,42.4737],[2.8399,42.459],[2.7984,42.4199],[2.6739,42.4047],[2.6633,42.341],[2.5619,42.3571],[2.4849,42.3401],[2.4103,42.3922],[2.2571,42.4385],[2.1266,42.4104],[2.0859,42.3637],[2.0128,42.3494],[1.9698,42.3767],[1.9338,42.4542],[1.7325,42.4932],[1.7401,42.5565],[1.7861,42.5736]]]},"properties":{"nom":"Languedoc-Roussillon"}},{"type":"Feature","geometry":{"type":"MultiPolygon","coordinates":[[[[4.6492,44.2704],[4.6799,44.3207],[4.8046,44.3039],[4.8253,44.2284],[5.0602,44.3076],[5.1495,44.3007],[5.1756,44.2209],[5.3845,44.2012],[5.3832,44.1553],[5.4547,44.1192],[5.5421,44.1333],[5.5757,44.1863],[5.6859,44.1972],[5.6756,44.2759],[5.6132,44.3158],[5.4929,44.3371],[5.4578,44.4982],[5.6036,44.4655],[5.6332,44.5021],[5.5972,44.5433],[5.6403,44.6478],[5.7906,44.6533],[5.848,44.7515],[5.9196,44.7528],[5.9495,44.8045],[6.1304,44.8625],[6.3554,44.8548],[6.3582,44.9416],[6.3133,45.0039],[6.2031,45.0124],[6.2293,45.1068],[6.3342,45.1229],[6.3969,45.062],[6.5765,45.1231],[6.63,45.1093],[6.6757,45.0209],[6.7395,45.0169],[6.7642,44.9665],[6.751,44.9055],[6.8648,44.8506],[6.9358,44.8612],[7.0033,44.8403],[6.9994,44.7897],[7.076,44.6847],[6.9862,44.6883],[6.9483,44.6548],[6.9306,44.5746],[6.8552,44.5303],[6.8959,44.3727],[7.007,44.2378],[7.187,44.2003],[7.2628,44.148],[7.3409,44.1451],[7.4223,44.1127],[7.501,44.1422],[7.616,44.1498],[7.7161,44.08],[7.6633,44.0286],[7.6527,43.976],[7.5717,43.9472],[7.5613,43.8996],[7.4966,43.8509],[7.5298,43.784],[7.3577,43.7213],[7.226,43.6612],[7.1646,43.6563],[7.1141,43.5657],[6.9704,43.5448],[6.8901,43.428],[6.7353,43.4078],[6.7138,43.3459],[6.6682,43.3143],[6.6653,43.2115],[6.6163,43.1626],[6.557,43.1882],[6.4941,43.1509],[6.3681,43.1356],[6.3261,43.0925],[6.2718,43.121],[6.201,43.1154],[6.1271,43.0743],[5.9979,43.1047],[5.929,43.105],[5.83,43.0499],[5.7711,43.1168],[5.646,43.188],[5.6048,43.1624],[5.5047,43.2003],[5.342,43.2133],[5.3729,43.2597],[5.3353,43.3478],[5.2242,43.3283],[5.0543,43.3266],[4.9252,43.4322],[4.8567,43.4033],[4.8571,43.3287],[4.7713,43.3492],[4.579,43.3622],[4.587,43.4266],[4.5255,43.4533],[4.2303,43.4602],[4.2447,43.5013],[4.4046,43.5605],[4.4272,43.6258],[4.4908,43.7004],[4.5978,43.6868],[4.6502,43.7809],[4.6417,43.8673],[4.6927,43.8846],[4.8409,43.9859],[4.8394,44.014],[4.7529,44.088],[4.6999,44.2161],[4.6492,44.2704]],[[5.0145,43.5555],[5.0068,43.4697],[5.0621,43.4024],[5.1363,43.4004],[5.1495,43.4579],[5.106,43.5256],[5.0145,43.5555]]],[[[4.8929,44.3648],[4.9708,44.4301],[5.0476,44.3821],[4.9836,44.295],[4.8895,44.304],[4.8929,44.3648]]]]},"properties":{"nom":"Provence-Alpes-Côte d'Azur"}},{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[9.4023,41.8587],[9.4067,41.7678],[9.3851,41.6534],[9.348,41.5638],[9.2984,41.5417],[9.2873,41.4838],[9.2259,41.4425],[9.2196,41.368],[9.0916,41.4001],[9.0558,41.463],[8.9243,41.4913],[8.7946,41.5565],[8.7927,41.6291],[8.8761,41.6507],[8.9123,41.691],[8.7861,41.7034],[8.7713,41.8112],[8.8002,41.8896],[8.611,41.9121],[8.5939,41.9636],[8.743,42.0431],[8.5796,42.1576],[8.5634,42.2378],[8.6889,42.2635],[8.6605,42.3023],[8.5526,42.3649],[8.6489,42.4139],[8.6795,42.4673],[8.667,42.5134],[8.727,42.5662],[8.8826,42.6273],[9.0176,42.6426],[9.1079,42.7245],[9.2218,42.7342],[9.2869,42.6759],[9.3428,42.7328],[9.312,42.834],[9.3595,42.9229],[9.3436,42.9977],[9.4257,43.0091],[9.4606,42.9859],[9.4915,42.7975],[9.4481,42.6655],[9.5275,42.565],[9.5434,42.4279],[9.5323,42.3821],[9.5598,42.2827],[9.5493,42.1039],[9.4153,41.9584],[9.4023,41.8587]]]},"properties":{"nom":"Corse"}}]};

/* ══════════════════════════════════════════════════════════════════════════════
   Pas de système de connexion — toutes les fonctionnalités sont accessibles.
   ══════════════════════════════════════════════════════════════════════════════ */

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
/* ── ÉTAT CARTE PROCHAIN JOUR (anciennes régions) ── */
let nextData         = {};      // { regionName: { band, phenom } }  band = clé RISK_BANDS, phenom = clé PICTO_FALLBACK
let nextActiveBand   = 'r3';
let nextActivePhenom = 'orage';
let svgNextEl=null, svgNextSel=null, layerNextSel=null;
let zoomedRegionNext = null;
let regionFeatures   = [];
let activeView   = 'vigilance'; // 'vigilance' | 'nextday' | 'probable'

/* ── SWITCH VUE ── */
function switchView(view) {
  activeView = view;
  document.getElementById('view-vigilance').style.display  = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('view-nextday').style.display    = view === 'nextday'   ? 'flex' : 'none';
  document.getElementById('view-probable').style.display   = view === 'probable'  ? 'flex' : 'none';
  document.getElementById('day-tabs-wrap').style.display   = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('vtab-vigil').classList.toggle('on', view === 'vigilance');
  document.getElementById('vtab-next').classList.toggle('on', view === 'nextday');
  document.getElementById('vtab-prob').classList.toggle('on', view === 'probable');
  if (view === 'nextday' && !svgNextEl) initNextMap();
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
    const b  = geoPath.bounds(feat);
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
  layerProbSel.transition().duration(450).ease(d3.easeCubicInOut).attr('transform','');
  redrawProbMap();
}

function resetProb() { probData = {}; redrawProbMap(); }

function dlMapProb() {
  if (!svgProbEl) return;
  // Créer une copie du SVG en s'assurant que les images sont bien incluses
  const c = svgProbEl.cloneNode(true);
  c.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
  // Ajouter le style pour s'assurer que tout est visible
  const style = document.createElementNS('http://www.w3.org/2000/svg', 'style');
  style.textContent = 'path { cursor: pointer; }';
  c.prepend(style);
  const blob = new Blob([c.outerHTML], {type:'image/svg+xml;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'evolution-probable.svg';
  a.click();
  URL.revokeObjectURL(a.href);
}

function updateLegProb() {
  const el = document.getElementById('leg-prob');
  const used = Object.values(probData);
  if (!used.length) { el.style.display='none'; return; }
  el.style.display = 'block';
  let h = `<div style="font-size:9px;font-weight:600;text-transform:uppercase;letter-spacing:.07em;color:#7c6a9a;margin-bottom:6px">Évolution probable</div>`;
  const lvls = [...new Set(used.map(d => d.level))];
  lvls.forEach(lv => {
    h += `<div class="prob-legend-item">
      <span class="prob-legend-dot" style="background:${LEVELS[lv].color}"></span>
      <span style="font-size:11px;color:var(--t2)">${LEVELS[lv].label}</span>
    </div>`;
  });
  const pics = [...new Set(used.filter(d => d.picto).map(d => d.picto))];
  if (pics.length) {
    h += `<div style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)">
      <div style="font-size:8px;text-transform:uppercase;letter-spacing:.06em;color:var(--t3);margin-bottom:4px">Phénomènes</div>
      <div style="display:flex;flex-wrap:wrap;gap:6px">`;
    pics.forEach(p => {
      h += `<div style="display:flex;flex-direction:column;align-items:center;gap:1px;width:28px">${ico(p,20)}<span style="font-size:7px;color:var(--t3)">${p.slice(0,5)}</span></div>`;
    });
    h += `</div></div>`;
  }
  const neutral = features.length - Object.keys(probData).length;
  if (neutral > 0) {
    h += `<div class="prob-legend-item" style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)">
      <span class="prob-legend-dot" style="background:#1e2535;border:1px solid rgba(255,255,255,.15)"></span>
      <span style="font-size:10px;color:var(--t3)">Pas de changement</span>
    </div>`;
  }
  el.innerHTML = h;
}

/* ══════════════════════════════════════════════════════════════════
   CARTE VIGILANCE PROCHAIN JOUR — anciennes régions (avant 2016)
   Même logique d'interaction que la carte principale (clic = peindre
   niveau + phénomène) mais appliquée aux 22 anciennes régions plutôt
   qu'aux départements, en mode "régions" pour le lendemain.
   ══════════════════════════════════════════════════════════════════ */
function buildNextSidebar() {
  const c = document.getElementById('next-lvl-container');
  RISK_BANDS.forEach(({ k, label, color }) => {
    const btn = document.createElement('button');
    btn.className = 'lvl-btn' + (k === nextActiveBand ? ' on' : '');
    btn.dataset.nlk = k;
    const lightBorder = (k === 'r1' || k === 'r2' || k === 'r3') ? 'border:1px solid rgba(0,0,0,.35)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${color};${lightBorder}"></span>${label}`;
    btn.onclick = () => {
      nextActiveBand = k;
      document.querySelectorAll('[data-nlk]').forEach(b => b.classList.toggle('on', b.dataset.nlk === k));
    };
    c.appendChild(btn);
  });
  buildNextPictoGrid();
}

function buildNextPictoGrid() {
  const g = document.getElementById('next-pgrid');
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'pbtn' + (k === nextActivePhenom ? ' on' : '');
    btn.dataset.ik = k;
    btn.innerHTML = ico(k, 26) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => {
      nextActivePhenom = k;
      document.querySelectorAll('#next-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k));
    };
    g.appendChild(btn);
  });
}

function initNextMap() {
  regionFeatures = REGIONS_OLD_GEOJSON.features;
  const mapDiv = document.getElementById('map-next');
  const W = 600, H = 624;
  d3.select(mapDiv).selectAll('svg').remove();
  const svg = d3.select(mapDiv).append('svg')
    .attr('viewBox', `0 0 ${W} ${H}`)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('display', 'block').style('width', '100%').style('height', '100%');
  svgNextEl = svg.node(); svgNextSel = svg;
  svg.append('rect').attr('width', W).attr('height', H).attr('fill', '#131823');
  // Lueur "néon" + légère élévation, une fois par bande de risque (réutilisée, donc léger).
  const defsNext = svg.append('defs');
  RISK_BANDS.forEach(b => {
    defsNext.append('filter').attr('id', 'nglow-'+b.k).attr('x','-70%').attr('y','-70%').attr('width','240%').attr('height','240%')
      .html(`<feDropShadow dx="0" dy="2" stdDeviation="2.4" flood-color="${b.color}" flood-opacity=".95"/><feDropShadow dx="0" dy="1.5" stdDeviation="1" flood-color="#000" flood-opacity=".55"/>`);
  });
  layerNextSel = svg.append('g').attr('class', 'region-layer-next');
  layerNextSel.selectAll('path.region-next').data(regionFeatures).join('path')
    .attr('class', 'region-next')
    .attr('d', d => geoPath(d))
    .attr('fill', DFILL).attr('stroke', 'rgba(255,255,255,.22)').attr('stroke-width', 0.9)
    .style('cursor', 'pointer')
    .on('mousemove', function(event, d) {
      const nm = d.properties.nom;
      const tip = document.getElementById('tip-next');
      const entries = nextData[nm];
      tip.style.display = 'block';
      const r = mapDiv.getBoundingClientRect();
      tip.style.left = (event.clientX - r.left + 14) + 'px';
      tip.style.top  = (event.clientY - r.top - 36) + 'px';
      tip.textContent = nm + (entries && entries.length
        ? ' — ' + entries.map(e => e.phenom + ' ' + RISK_MAP[e.band].label).join(' · ')
        : '');
      if (nm !== zoomedRegionNext) d3.select(this).attr('stroke', 'rgba(255,255,255,.6)').attr('stroke-width', 1.4);
    })
    .on('mouseleave', function(event, d) {
      document.getElementById('tip-next').style.display = 'none';
      const nm = d.properties.nom;
      if (nm !== zoomedRegionNext) redrawNextPath(d3.select(this), nm);
    })
    .on('click', function(event, d) {
      const nm = d.properties.nom; if (!nm) return;
      const cur = nextData[nm] || [];
      const idx = cur.findIndex(e => e.band === nextActiveBand && e.phenom === nextActivePhenom);
      let updated;
      if (idx !== -1) {
        // Déjà présent avec exactement la même bande + phénomène → un clic l'annule.
        updated = cur.filter((_, i) => i !== idx);
      } else if (cur.length < 2) {
        // Un clic ajoute ; une région peut porter jusqu'à 2 éléments simultanément.
        updated = [...cur, { band: nextActiveBand, phenom: nextActivePhenom }];
      } else {
        // Déjà 2 éléments différents : le nouveau remplace le plus ancien (FIFO).
        updated = [cur[1], { band: nextActiveBand, phenom: nextActivePhenom }];
      }
      if (updated.length) nextData[nm] = updated; else delete nextData[nm];
      redrawNextMap();
    })
    .on('dblclick', function(event, d) {
      event.stopPropagation();
      const nm = d.properties.nom; if (!nm) return;
      if (zoomedRegionNext === nm) { unzoomNext(); return; }
      zoomToNext(d, nm);
    });
  svg.on('dblclick', e => { if (e.target === svgNextEl) unzoomNext(); });
  updateLegNext();
}

function redrawNextPath(sel, nm) {
  const entries = nextData[nm];
  const isZoomed = nm === zoomedRegionNext;
  if (entries && entries.length) {
    // Couleur de base = 1er élément (recouverte par le partage visuel si 2 éléments).
    sel.attr('fill', RISK_MAP[entries[0].band].color)
       .attr('stroke', isZoomed ? '#fff' : 'rgba(0,0,0,.3)')
       .attr('stroke-width', isZoomed ? 2.5 : 0.9);
  } else {
    sel.attr('fill', DFILL)
       .attr('stroke', isZoomed ? '#fff' : 'rgba(255,255,255,.22)')
       .attr('stroke-width', isZoomed ? 2.5 : 0.9);
  }
}

/* Place un picto (agrandi + net si perso) centré en (cx,cy). Le détail (%) reste dans
   l'infobulle au survol et la légende, pour ne pas charger visuellement la carte. */
function placeNextItem(phenom, band, cx, cy, sz) {
  placeIcoImg(layerNextSel, phenom, cx, cy, sz).attr('class', 'region-picto-next');
}

function redrawNextMap() {
  if (!layerNextSel) return;
  layerNextSel.selectAll('path.region-next').each(function(d) {
    redrawNextPath(d3.select(this), d.properties.nom);
  });
  layerNextSel.selectAll('.region-split, .region-picto-next, .region-clipdef').remove();

  Object.entries(nextData).forEach(([name, entries]) => {
    if (!entries || !entries.length) return;
    const feat = regionFeatures.find(f => f.properties.nom === name);
    if (!feat) return;
    const b  = geoPath.bounds(feat);
    const x0 = b[0][0], y0 = b[0][1], x1 = b[1][0], y1 = b[1][1];
    const w  = x1 - x0, h = y1 - y0;

    if (entries.length === 1) {
      const sz = Math.max(14, Math.min(Math.min(w, h) * 0.38, 28));
      placeNextItem(entries[0].phenom, entries[0].band, x0 + w/2, y0 + h/2, sz);
      return;
    }

    /* 2 éléments : pas de ligne — un écart entre les deux zones, alignées côte à côte si la
       région est plus large que haute ("horizontale"), empilées sinon ("verticale"), chacune
       surélevée avec une lueur néon de sa couleur de risque. */
    const sideBySide = w >= h;
    const cid = 'nclip-' + name.normalize('NFD').replace(/[^a-zA-Z0-9]/g, '');
    const defs = layerNextSel.append('g').attr('class', 'region-clipdef');
    const gapFrac = 0.10; // proportion de l'axe partagé laissée vide entre les deux zones
    let rectA, rectB, centA, centB;
    if (sideBySide) {
      const g2 = w * gapFrac / 2;
      rectA = { x:x0,            y:y0, w:w/2 - g2, h:h };
      rectB = { x:x0+w/2 + g2,   y:y0, w:w/2 - g2, h:h };
      centA = { x:x0+w*0.25, y:y0+h/2 };
      centB = { x:x0+w*0.75, y:y0+h/2 };
    } else {
      const g2 = h * gapFrac / 2;
      rectA = { x:x0, y:y0,            w:w, h:h/2 - g2 };
      rectB = { x:x0, y:y0+h/2 + g2,   w:w, h:h/2 - g2 };
      centA = { x:x0+w/2, y:y0+h*0.25 };
      centB = { x:x0+w/2, y:y0+h*0.75 };
    }
    defs.append('clipPath').attr('id', cid+'-a').append('rect')
      .attr('x', rectA.x).attr('y', rectA.y).attr('width', rectA.w).attr('height', rectA.h);
    defs.append('clipPath').attr('id', cid+'-b').append('rect')
      .attr('x', rectB.x).attr('y', rectB.y).attr('width', rectB.w).attr('height', rectB.h);

    layerNextSel.append('path').attr('class', 'region-split')
      .attr('d', geoPath(feat)).attr('fill', RISK_MAP[entries[0].band].color)
      .attr('stroke', 'rgba(255,255,255,.5)').attr('stroke-width', 1)
      .attr('clip-path', `url(#${cid}-a)`).attr('filter', `url(#nglow-${entries[0].band})`).attr('pointer-events', 'none');
    layerNextSel.append('path').attr('class', 'region-split')
      .attr('d', geoPath(feat)).attr('fill', RISK_MAP[entries[1].band].color)
      .attr('stroke', 'rgba(255,255,255,.5)').attr('stroke-width', 1)
      .attr('clip-path', `url(#${cid}-b)`).attr('filter', `url(#nglow-${entries[1].band})`).attr('pointer-events', 'none');

    const halfMin = sideBySide ? Math.min(w/2, h) : Math.min(w, h/2);
    const sz = Math.max(10, Math.min(halfMin * 0.42, 22));
    placeNextItem(entries[0].phenom, entries[0].band, centA.x, centA.y, sz);
    placeNextItem(entries[1].phenom, entries[1].band, centB.x, centB.y, sz);
  });
  updateLegNext();
}

function zoomToNext(feat, name) {
  const W = 600, H = 624;
  zoomedRegionNext = name;
  layerNextSel.selectAll('path.region-next').attr('stroke', 'rgba(255,255,255,.22)').attr('stroke-width', 0.9);
  layerNextSel.selectAll('path.region-next').filter(d => d.properties.nom === name)
    .raise().attr('stroke', '#fff').attr('stroke-width', 2.5);
  const b = geoPath.bounds(feat);
  const sc = Math.min(0.82*W/(b[1][0]-b[0][0]), 0.82*H/(b[1][1]-b[0][1]), 6);
  const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
  layerNextSel.transition().duration(480).ease(d3.easeCubicInOut)
    .attr('transform', `translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
}
function unzoomNext() {
  zoomedRegionNext = null;
  layerNextSel.selectAll('path.region-next').attr('stroke', 'rgba(255,255,255,.22)').attr('stroke-width', 0.9);
  layerNextSel.transition().duration(450).ease(d3.easeCubicInOut).attr('transform', '');
  redrawNextMap();
}
function resetNext() { nextData = {}; redrawNextMap(); }

function dlMapNext() {
  if (!svgNextEl) return;
  const c = svgNextEl.cloneNode(true);
  c.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
  const style = document.createElementNS('http://www.w3.org/2000/svg', 'style');
  style.textContent = 'path { cursor: pointer; }';
  c.prepend(style);
  const blob = new Blob([c.outerHTML], {type: 'image/svg+xml;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'vigilance-prochain-jour-regions.svg';
  a.click();
  URL.revokeObjectURL(a.href);
}

function updateLegNext() {
  const el = document.getElementById('leg-next');
  const used = Object.values(nextData).flat();
  if (!used.length) { el.style.display = 'none'; return; }
  el.style.display = 'block';
  let h = `<div style="font-size:9px;font-weight:600;text-transform:uppercase;letter-spacing:.07em;color:#14b8c4;margin-bottom:6px">Risque prochain jour</div>`;
  const bands = [...new Set(used.map(d => d.band))].sort();
  bands.forEach(bk => {
    h += `<div class="prob-legend-item">
      <span class="prob-legend-dot" style="background:${RISK_MAP[bk].color};border:1px solid rgba(0,0,0,.3)"></span>
      <span style="font-size:11px;color:var(--t2)">${RISK_MAP[bk].label}</span>
    </div>`;
  });
  const pics = [...new Set(used.map(d => d.phenom))];
  if (pics.length) {
    h += `<div style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)">
      <div style="font-size:8px;text-transform:uppercase;letter-spacing:.06em;color:var(--t3);margin-bottom:4px">Phénomènes</div>
      <div style="display:flex;flex-wrap:wrap;gap:6px">`;
    pics.forEach(p => {
      h += `<div style="display:flex;flex-direction:column;align-items:center;gap:1px;width:28px">${ico(p,20)}<span style="font-size:7px;color:var(--t3)">${p.slice(0,5)}</span></div>`;
    });
    h += `</div></div>`;
  }
  const neutral = regionFeatures.length - Object.keys(nextData).length;
  if (neutral > 0) {
    h += `<div class="prob-legend-item" style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)">
      <span class="prob-legend-dot" style="background:#1e2535;border:1px solid rgba(255,255,255,.15)"></span>
      <span style="font-size:10px;color:var(--t3)">Pas de risque renseigné</span>
    </div>`;
  }
  el.innerHTML = h;
}

/* ── SIDEBAR PROBABLE BUILD ── */
function buildSidebar() {
  // Niveaux
  const c = document.getElementById('lvl-container');
  LVL_DEFS.forEach(({ k, l, semi }) => {
    const row = document.createElement('div');
    row.className = 'lvl-row-wrap';
    const btn = document.createElement('button');
    btn.className = 'lvl-btn' + (k === 'vert' ? ' on' : '') + (semi ? ' semi' : '');
    btn.dataset.lk = k;
    const yc = (k === 'pre-alerte' || k === 'jaune' || k === 'jaune-orange') ? 'border:1px solid rgba(0,0,0,.4)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color};${yc}"></span>${l}`;
    btn.onclick = () => selectLevel(k);
    row.appendChild(btn);
    if (k !== 'vert') {
      const pb = document.createElement('button');
      pb.className = 'prob-btn';
      pb.title = 'Passage probable vers ce niveau';
      pb.dataset.pk = k;
      // Icône : flèche montante avec tirets (= incertain)
      pb.innerHTML = `<svg viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
        <polyline points="2,7 5,3 8,7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <line x1="5" y1="8.5" x2="5" y2="8.5" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/>
      </svg>`;
      pb.style.color = 'var(--t3)';
      pb.onclick = e => { e.stopPropagation(); toggleProb(k); };
      row.appendChild(pb);
    }
    c.appendChild(row);
  });

  // Pictos
  buildPictoGrid('pgrid', k => selectPicto(k), 'pbtn');
  // DV niveaux
  const dvc = document.getElementById('dv-lvls');
  LVL_DEFS.filter(d => d.k !== 'vert').forEach(({ k, l }) => {
    const btn = document.createElement('button');
    btn.className = 'lvl-btn' + (k === dvLevel ? ' on' : '');
    btn.dataset.dvk = k;
    btn.style.fontSize = '11px';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${l}`;
    btn.onclick = () => { dvLevel = k; document.querySelectorAll('[data-dvk]').forEach(b => b.classList.toggle('on', b.dataset.dvk === k)); };
    dvc.appendChild(btn);
  });
  buildPictoGrid('dv-pgrid', k => { dvPicto = dvPicto === k ? null : k; document.querySelectorAll('#dv-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k)); }, 'pbtn dv-pbtn');
}

function buildPictoGrid(id, onclick, cls) {
  const g = document.getElementById(id);
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button');
    btn.className = cls;
    btn.dataset.ik = k;
    btn.innerHTML = ico(k, 26) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => onclick(k);
    g.appendChild(btn);
  });
}

/* ── CONTRÔLES ── */
function selectLevel(l) {
  activeLevel = l;
  if (probLevel && probLevel !== l) { probLevel = null; document.querySelectorAll('.prob-btn').forEach(b => b.classList.remove('on')); }
  document.querySelectorAll('[data-lk]').forEach(b => b.classList.toggle('on', b.dataset.lk === l));
}
function selectPicto(k) {
  activePicto = activePicto === k ? null : k;
  document.querySelectorAll('#pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k));
}
function toggleProb(level) {
  if (probLevel === level) {
    probLevel = null;
    document.querySelectorAll('.prob-btn').forEach(b => b.classList.remove('on'));
  } else {
    probLevel = level;
    document.querySelectorAll('.prob-btn').forEach(b => b.classList.toggle('on', b.dataset.pk === level));
    selectLevel(level);
  }
}
function switchDay(d) {
  activeDay = d;
  document.getElementById('dtab-today').classList.toggle('on', d === 'today');
  document.getElementById('dtab-tomorrow').classList.toggle('on', d === 'tomorrow');
  redrawMap();
  renderGauges(zoomedDept);
}
function toggleSub() {
  subVisible = !subVisible;
  document.getElementById('sub-tog').classList.toggle('on', subVisible);
  document.getElementById('sub-lvls').style.display = subVisible ? 'flex' : 'none';
  drawSub();
}
function setSubLvl(l) {
  subLvl = l;
  document.querySelectorAll('[data-sl]').forEach(b => b.classList.toggle('on', b.dataset.sl === l));
  if (subVisible) drawSub();
}
function toggleDV() {
  dvActive = !dvActive;
  document.getElementById('dv-tog').classList.toggle('on', dvActive);
  document.getElementById('dv-opts').style.display = dvActive ? 'block' : 'none';
}
function resetAll() {
  deptData[activeDay] = {};
  redrawMap();
}

/* ── RENDU CARTE ── */
function curData() { return deptData[activeDay]; }

function redrawMap() {
  if (!layerSel) return;
  const data = curData();

  layerSel.selectAll('path.dept')
    .attr('fill', d => {
      const n = d.properties && (d.properties.name || d.properties.NAME || '');
      const dd = data[n];
      if (!dd) return DFILL;
      if (dd.probable) {
        // Fond très léger = couleur à 15% opacité simulée via mélange avec bg
        return hexBlend(LEVELS[dd.level].color, '#1e2535', 0.18);
      }
      return LEVELS[dd.level].color;
    })
    .attr('stroke', d => {
      const n = d.properties && (d.properties.name || d.properties.NAME || '');
      const dd = data[n];
      if (dd && dd.probable) return LEVELS[dd.level].color;
      if (n === zoomedDept) return '#fff';
      return DSTROKE;
    })
    .attr('stroke-width', d => {
      const n = d.properties && (d.properties.name || d.properties.NAME || '');
      const dd = data[n];
      if (dd && dd.probable) return 2;
      if (n === zoomedDept) return 2.5;
      return 0.6;
    })
    .attr('stroke-dasharray', d => {
      const n = d.properties && (d.properties.name || d.properties.NAME || '');
      const dd = data[n];
      return (dd && dd.probable) ? '4,3' : 'none';
    });

  // Pictos sur la carte via <image> SVG natif (net, pas de foreignObject)
  layerSel.selectAll('.dept-picto-g').remove();
  Object.entries(data).forEach(([name, dd]) => {
    if (!dd.picto || !PICTO_FALLBACK[dd.picto]) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === name);
    if (!feat) return;
    const b  = geoPath.bounds(feat);
    const cx = (b[0][0] + b[1][0]) / 2;
    const cy = (b[0][1] + b[1][1]) / 2;
    const sz = Math.max(12, Math.min((Math.min(b[1][0]-b[0][0], b[1][1]-b[0][1])) * 0.52, 26));
    placeIcoImg(layerSel, dd.picto, cx, cy, sz).attr('class', 'dept-picto-g');
  });

  updateDVBadges();
  updateLeg();
}

function updateDVBadges() {
  if (!layerSel) return;
  layerSel.selectAll('.dv-badge').remove();
  const data = curData();
  Object.entries(data).forEach(([name, dd]) => {
    if (!dd.dv) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === name);
    if (!feat) return;
    const b  = geoPath.bounds(feat);
    const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
    const w2 = b[1][0]-b[0][0], h2 = b[1][1]-b[0][1];
    // Taille proportionnelle au département (plutôt qu'un rayon fixe) : reste discret
    // sur les petits départements et lisible sur les grands → moins d'effet "plein de ronds".
    const s  = Math.max(9, Math.min(Math.min(w2,h2) * 0.32, 15));
    const bx = cx+w2*0.25, by = cy-h2*0.25;
    const g = layerSel.append('g').attr('class','dv-badge').attr('pointer-events','none')
      .attr('filter','url(#dvShadow)');
    // Carré arrondi (cohérent avec les puces de niveau du reste de l'UI) au lieu d'un rond plein.
    g.append('rect')
      .attr('x', bx-s/2).attr('y', by-s/2).attr('width', s).attr('height', s)
      .attr('rx', s*0.3).attr('ry', s*0.3)
      .attr('fill', LEVELS[dd.dv.level].color)
      .attr('stroke', 'rgba(15,17,23,.5)').attr('stroke-width', 1);
    if (dd.dv.picto && PICTO_FALLBACK[dd.dv.picto]) {
      const isz = s*0.72;
      placeIcoImg(g, dd.dv.picto, bx, by, isz);
    } else {
      g.append('text').attr('x',bx).attr('y',by+0.5).attr('text-anchor','middle').attr('dominant-baseline','central')
        .attr('font-size',s*0.62).attr('font-weight',700).attr('fill','white').text('2');
    }
  });
}

function drawSub() {
  if (!svgSel) return;
  svgSel.selectAll('.sub-halo').remove();
  svgSel.selectAll('#sub-defs-el').remove();
  if (!subVisible) return;
  const color = LEVELS[subLvl].color;
  const ns = 'http://www.w3.org/2000/svg';
  const svgNode = svgSel.node();
  const [,, W, H] = svgEl.getAttribute('viewBox').split(' ').map(Number);
  const bW = W * 0.038;
  const defs = document.createElementNS(ns,'defs'); defs.id = 'sub-defs-el';
  const mask = document.createElementNS(ns,'mask'); mask.id = 'sub-mask';
  const mr = document.createElementNS(ns,'rect');
  mr.setAttribute('x',String(-W)); mr.setAttribute('y',String(-H));
  mr.setAttribute('width',String(W*3)); mr.setAttribute('height',String(H*3));
  mr.setAttribute('fill','white'); mask.appendChild(mr);
  features.forEach(d => {
    const nm = d.properties && (d.properties.name || d.properties.NAME || '');
    const pd = pathMap[nm]; if (!pd) return;
    const p = document.createElementNS(ns,'path');
    p.setAttribute('d',pd); p.setAttribute('fill','black'); p.setAttribute('stroke','black'); p.setAttribute('stroke-width','3');
    mask.appendChild(p);
  });
  defs.appendChild(mask); svgNode.insertBefore(defs, svgNode.firstChild);
  const g = document.createElementNS(ns,'g');
  g.setAttribute('class','sub-halo'); g.setAttribute('mask','url(#sub-mask)'); g.style.pointerEvents='none';
  features.forEach(d => {
    const nm = d.properties && (d.properties.name || d.properties.NAME || '');
    if (!COASTAL.has(nm)) return;
    const pd = pathMap[nm]; if (!pd) return;
    const p = document.createElementNS(ns,'path');
    p.setAttribute('d',pd); p.setAttribute('fill',color); p.setAttribute('stroke',color);
    p.setAttribute('stroke-width',String(bW)); p.setAttribute('stroke-linejoin','round'); p.setAttribute('stroke-linecap','round');
    g.appendChild(p);
  });
  svgNode.insertBefore(g, layerSel.node());
}

/* ── LÉGENDE ── */
function updateLeg() {
  const el = document.getElementById('leg');
  const data = curData();
  const vals = Object.values(data);
  const lvls = [...new Set(vals.filter(d=>!d.probable).map(d=>d.level).filter(l=>l!=='vert'))];
  const probs = [...new Set(vals.filter(d=>d.probable).map(d=>d.level))];
  const pics = [...new Set(vals.map(d=>d.picto).filter(Boolean))];
  if (!lvls.length && !probs.length && !pics.length && !subVisible) { el.style.display='none'; return; }
  el.style.display = 'block';
  let h = '';
  if (lvls.length) {
    h += `<div class="leg-sec"><div class="leg-sec-t">Vigilance</div>`;
    lvls.forEach(lv => { h += `<div class="leg-r"><span class="leg-dot" style="background:${LEVELS[lv].color}"></span><span class="leg-lbl">${LEVELS[lv].label}</span></div>`; });
    h += `</div>`;
  }
  if (probs.length) {
    h += `<div class="leg-sec"><div class="leg-sec-t" style="color:#ffb400">Probable</div>`;
    probs.forEach(lv => {
      const c = LEVELS[lv].color;
      h += `<div class="leg-r">
        <span style="width:13px;height:13px;border-radius:3px;flex-shrink:0;display:inline-block;
          background:repeating-linear-gradient(45deg,${c}2a 0,${c}2a 2px,${c}99 2px,${c}99 5px,${c}2a 5px,${c}2a 8px);
          border:1px solid ${c}88"></span>
        <span class="leg-lbl" style="color:#ffb400">~ ${LEVELS[lv].label}</span>
      </div>`;
    });
    h += `</div>`;
  }
  if (pics.length) {
    h += `<div class="leg-sec"><div class="leg-sec-t">Phénomènes</div><div style="display:flex;flex-wrap:wrap;gap:4px">`;
    pics.forEach(pk => { h += `<div style="display:flex;flex-direction:column;align-items:center;gap:2px;width:32px">${ico(pk,22)}<span style="font-size:8px;color:var(--t3)">${pk.slice(0,5)}</span></div>`; });
    h += `</div></div>`;
  }
  if (subVisible) {
    h += `<div class="leg-sec"><div class="leg-sec-t">Submersion</div><div class="leg-r"><span class="leg-dot" style="background:${LEVELS[subLvl].color}"></span><span class="leg-lbl">Zone côtière</span></div></div>`;
  }
  el.innerHTML = h;
}

/* ── ZOOM ── */
function zoomTo(feat, name) {
  if (!svgSel || !geoPath) return;
  const [,, W, H] = svgEl.getAttribute('viewBox').split(' ').map(Number);
  zoomedDept = name;
  layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6);
  layerSel.selectAll('path.dept').filter(d => (d.properties.name || d.properties.NAME) === name)
    .raise().attr('stroke','#fff').attr('stroke-width', 2.5);
  const b = geoPath.bounds(feat);
  const dx = b[1][0]-b[0][0], dy = b[1][1]-b[0][1];
  const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
  const sc = Math.min(0.82*W/dx, 0.82*H/dy, 6);
  layerSel.transition().duration(480).ease(d3.easeCubicInOut)
    .attr('transform', `translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
  renderGauges(name);
}
function unzoom() {
  if (!svgSel) return;
  zoomedDept = null;
  layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6);
  layerSel.transition().duration(450).ease(d3.easeCubicInOut).attr('transform','');
  renderGauges(null);
}

/* ── DOWNLOAD ── */
function dlMap() {
  if (!svgEl) return;
  const c = svgEl.cloneNode(true);
  c.setAttribute('xmlns','http://www.w3.org/2000/svg');
  // Ajouter le style pour s'assurer que tout est visible
  const style = document.createElementNS('http://www.w3.org/2000/svg', 'style');
  style.textContent = 'path { cursor: pointer; }';
  c.prepend(style);
  const blob = new Blob([c.outerHTML], {type:'image/svg+xml;charset=utf-8'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'vigilance-france.svg';
  a.click();
  URL.revokeObjectURL(a.href);
}

/* ── JAUGES RENDER ── */
function renderGauges(scope) {
  const sec = document.getElementById('gauges');
  sec.innerHTML = '';
  const rows = gRows(scope);
  const wrap = document.createElement('div');
  wrap.className = 'g-wrap' + (scope ? ' dept' : '');

  // Header
  const hdr = document.createElement('div'); hdr.className = 'g-hdr';
  const ttl = document.createElement('div'); ttl.className = 'g-title';
  const dayLabel = activeDay === 'today' ? "Aujourd'hui" : 'Demain';
  ttl.innerHTML = scope
    ? `${scope} <span class="g-badge">Département</span>`
    : `France <span class="g-badge">National · ${dayLabel}</span>`;
  hdr.appendChild(ttl);
  if (scope) {
    const bk = document.createElement('button'); bk.className = 'g-back';
    bk.textContent = '\u2190 Vue nationale'; bk.onclick = () => unzoom();
    hdr.appendChild(bk);
  }
  wrap.appendChild(hdr);

  // Ticks horaires
  const ticks = document.createElement('div'); ticks.className = 'g-ticks';
  [0,3,6,9,12,15,18,21,24].forEach(h => {
    const t = document.createElement('span'); t.className = 'g-tick'; t.textContent = h+'h'; ticks.appendChild(t);
  });
  wrap.appendChild(ticks);

  // Rows de phénomènes
  const rowsEl = document.createElement('div'); rowsEl.className = 'g-rows';
  rows.forEach((row, ri) => rowsEl.appendChild(buildGRow(scope, ri, row, rows.length > 1)));
  wrap.appendChild(rowsEl);

  const addBtn = document.createElement('button'); addBtn.className = 'g-add';
  addBtn.textContent = '+ Phénomène';
  addBtn.onclick = () => { gRows(scope).push({ picto: null, segs: [] }); renderGauges(scope); };
  wrap.appendChild(addBtn);
  sec.appendChild(wrap);
}

function buildGRow(scope, ri, row, canDel) {
  const el = document.createElement('div'); el.className = 'g-row';

  // Picto cliquable
  const pw = document.createElement('div'); pw.className = 'g-picto';
  pw.innerHTML = row.picto ? ico(row.picto, 20) : '<span style="font-size:13px;color:var(--t3)">+</span>';
  pw.onclick = e => openPhenPop(e, scope, ri);
  el.appendChild(pw);

  // Label
  const lbl = document.createElement('div'); lbl.className = 'g-lbl';
  lbl.textContent = row.picto || '—'; el.appendChild(lbl);

  // Timeline
  const tl = document.createElement('div'); tl.className = 'g-tl';
  const bg = document.createElement('div'); bg.className = 'g-bg'; tl.appendChild(bg);
  const ns = 'http://www.w3.org/2000/svg';
  const svgT = document.createElementNS(ns,'svg');
  svgT.style.cssText = 'position:absolute;top:0;left:0;width:100%;height:100%;overflow:visible;pointer-events:none';
  svgT.setAttribute('preserveAspectRatio','none');
  tl.appendChild(svgT);
  row.segs.forEach((seg, si) => drawSeg(svgT, tl, scope, ri, si, seg));
  // Clic pour ajouter segment
  tl.addEventListener('mousedown', e => {
    if (e.target !== tl && e.target !== bg) return;
    const r = tl.getBoundingClientRect();
    const h = Math.round(((e.clientX-r.left)/r.width)*24*4)/4;
    gRows(scope)[ri].segs.push({ s: Math.max(0,h-1), e: Math.min(24,h+1), lv: activeLevel });
    gRows(scope)[ri].segs.sort((a,b) => a.s-b.s);
    renderGauges(zoomedDept);
  });
  el.appendChild(tl);

  if (canDel) {
    const d = document.createElement('button'); d.className = 'g-del'; d.textContent = '×';
    d.onclick = () => { gRows(scope).splice(ri,1); renderGauges(zoomedDept); };
    el.appendChild(d);
  }
  return el;
}

function drawSeg(svgT, tlEl, scope, ri, si, seg) {
  const ns = 'http://www.w3.org/2000/svg';
  const pL = (seg.s/24)*100, pR = (seg.e/24)*100, wPct = pR-pL;
  const color = LEVELS[seg.lv].color;
  const g = document.createElementNS(ns,'g');
  g.style.pointerEvents = 'all'; g.style.cursor = 'grab';

  const rect = document.createElementNS(ns,'rect');
  rect.setAttribute('x',pL+'%'); rect.setAttribute('y','0');
  rect.setAttribute('width',wPct+'%'); rect.setAttribute('height','100%');
  rect.setAttribute('fill',color); rect.setAttribute('rx','3');
  g.appendChild(rect);

  const hL = document.createElementNS(ns,'rect');
  hL.setAttribute('x',pL+'%'); hL.setAttribute('y','0'); hL.setAttribute('width','8px'); hL.setAttribute('height','100%');
  hL.setAttribute('fill','rgba(0,0,0,.3)'); hL.setAttribute('rx','2'); hL.style.cursor='ew-resize'; hL.style.opacity='0';
  g.appendChild(hL);

  const hR = document.createElementNS(ns,'rect');
  hR.setAttribute('x',`calc(${pR}% - 8px)`); hR.setAttribute('y','0'); hR.setAttribute('width','8px'); hR.setAttribute('height','100%');
  hR.setAttribute('fill','rgba(0,0,0,.3)'); hR.setAttribute('rx','2'); hR.style.cursor='ew-resize'; hR.style.opacity='0';
  g.appendChild(hR);

  g.addEventListener('mouseenter',()=>{ hL.style.opacity='1'; hR.style.opacity='1'; });
  g.addEventListener('mouseleave',()=>{ hL.style.opacity='0'; hR.style.opacity='0'; });

  function makeDrag(onMove) {
    return e => {
      e.stopPropagation(); e.preventDefault();
      const rc = tlEl.getBoundingClientRect();
      const sx = e.clientX;
      const mv = mv2 => onMove(mv2, rc, sx);
      const up = () => { window.removeEventListener('mousemove',mv); window.removeEventListener('mouseup',up); };
      window.addEventListener('mousemove',mv); window.addEventListener('mouseup',up);
    };
  }

  g.addEventListener('mousedown', makeDrag((mv, rc, sx) => {
    if (mv.target === hL || mv.target === hR) return;
    const rows = gRows(scope);
    const dur = rows[ri].segs[si].e - rows[ri].segs[si].s;
    const os = rows[ri].segs[si].s;
    const dx = (mv.clientX-sx)/rc.width*24;
    let ns2 = Math.round(Math.max(0,Math.min(24-dur,os+dx))*4)/4;
    rows[ri].segs[si].s = ns2; rows[ri].segs[si].e = ns2+dur;
    const nl=(ns2/24)*100, nr=((ns2+dur)/24)*100;
    rect.setAttribute('x',nl+'%'); rect.setAttribute('width',(nr-nl)+'%');
    hL.setAttribute('x',nl+'%'); hR.setAttribute('x',`calc(${nr}% - 8px)`);
  }));

  hL.addEventListener('mousedown', makeDrag((mv, rc, sx) => {
    const rows = gRows(scope); const os = rows[ri].segs[si].s;
    const dx = (mv.clientX-sx)/rc.width*24;
    let ns2 = Math.round(Math.max(0,Math.min(rows[ri].segs[si].e-0.25,os+dx))*4)/4;
    rows[ri].segs[si].s = ns2;
    const nl=(ns2/24)*100, nr=(rows[ri].segs[si].e/24)*100;
    rect.setAttribute('x',nl+'%'); rect.setAttribute('width',(nr-nl)+'%'); hL.setAttribute('x',nl+'%');
  }));

  hR.addEventListener('mousedown', makeDrag((mv, rc, sx) => {
    const rows = gRows(scope); const oe = rows[ri].segs[si].e;
    const dx = (mv.clientX-sx)/rc.width*24;
    let ne = Math.round(Math.max(rows[ri].segs[si].s+0.25,Math.min(24,oe+dx))*4)/4;
    rows[ri].segs[si].e = ne;
    const nl=(rows[ri].segs[si].s/24)*100, nr=(ne/24)*100;
    rect.setAttribute('x',nl+'%'); rect.setAttribute('width',(nr-nl)+'%'); hR.setAttribute('x',`calc(${nr}% - 8px)`);
  }));

  g.addEventListener('contextmenu', e => { e.preventDefault(); e.stopPropagation(); openLvlPop(e, scope, ri, si); });
  g.addEventListener('dblclick', e => { e.stopPropagation(); gRows(scope)[ri].segs.splice(si,1); renderGauges(zoomedDept); });
  svgT.appendChild(g);
}

/* ── POPUPS ── */
function openLvlPop(e, scope, ri, si) {
  const p = document.getElementById('lvl-pop'); p.innerHTML = '';
  Object.entries(LEVELS).forEach(([lv, info]) => {
    const it = document.createElement('div'); it.className = 'lp-item';
    it.innerHTML = `<span class="lp-dot" style="background:${info.color}"></span>${info.label}`;
    it.onclick = () => { gRows(scope)[ri].segs[si].lv=lv; renderGauges(zoomedDept); p.classList.remove('open'); };
    p.appendChild(it);
  });
  p.style.left = e.clientX+'px'; p.style.top = e.clientY+'px'; p.classList.add('open'); e.stopPropagation();
}

function openPhenPop(e, scope, ri) {
  const p = document.getElementById('phen-pop'); p.innerHTML = '';
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const it = document.createElement('div'); it.className = 'pp-item';
    it.innerHTML = ico(k,20) + `<span>${k}</span>`;
    it.onclick = () => { gRows(scope)[ri].picto=k; renderGauges(zoomedDept); p.classList.remove('open'); };
    p.appendChild(it);
  });
  const none = document.createElement('div'); none.className='pp-item';
  none.innerHTML = `<span style="font-size:18px;color:var(--t3)">—</span><span>Aucun</span>`;
  none.onclick = () => { gRows(scope)[ri].picto=null; renderGauges(zoomedDept); p.classList.remove('open'); };
  p.appendChild(none);
  const r = e.currentTarget.getBoundingClientRect();
  p.style.left = (r.right+6)+'px'; p.style.top = r.top+'px'; p.classList.add('open'); e.stopPropagation();
}

document.addEventListener('click', () => {
  document.getElementById('lvl-pop').classList.remove('open');
  document.getElementById('phen-pop').classList.remove('open');
});
document.addEventListener('keydown', e => {
  if (e.key==='Escape') { document.getElementById('lvl-pop').classList.remove('open'); document.getElementById('phen-pop').classList.remove('open'); if(zoomedDept) unzoom(); }
});

/* ── INIT ── */
buildSidebar();
buildNextSidebar();
buildProbSidebar();

(async () => {
  const mapDiv = document.getElementById('map');
  const tip    = document.getElementById('tip');
  let topo;
  try { topo = await d3.json('https://cdn.jsdelivr.net/npm/datamaps@0.5.10/src/js/data/fra.topo.json'); }
  catch(e) { mapDiv.innerHTML = '<p style="color:var(--t3);text-align:center;padding:60px">Données indisponibles.</p>'; return; }

  const key = Object.keys(topo.objects)[0];
  features = topojson.feature(topo, topo.objects[key]).features;
  const W = 600, H = 624;
  const proj = d3.geoConicConformal().parallels([44,49]).rotate([-3,0]).center([0,46.5]).scale(1860).translate([300,312]);
  geoPath = d3.geoPath().projection(proj);

  const svg = d3.select(mapDiv).append('svg')
    .attr('viewBox',`0 0 ${W} ${H}`)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('display','block')
    .style('width','100%')
    .style('height','auto');
  svgEl = svg.node(); svgSel = svg;
  svg.append('rect').attr('width',W).attr('height',H).attr('fill','#131823');
  const defs0 = svg.append('defs'); // réservé pour patterns hachures
  // Ombre portée unique réutilisée par tous les badges "double vigilance" (plus léger qu'une
  // ombre par élément, et évite l'effet "rond plat" trop dur sur la carte).
  defs0.append('filter').attr('id','dvShadow').attr('x','-60%').attr('y','-60%').attr('width','220%').attr('height','220%')
    .append('feDropShadow').attr('dx',0).attr('dy',0.6).attr('stdDeviation',1).attr('flood-color','#000').attr('flood-opacity',0.5);
  layerSel = svg.append('g').attr('class','dept-layer');

  layerSel.selectAll('path.dept').data(features).join('path')
    .attr('class','dept')
    .attr('d', d => { const pd = geoPath(d); const nm = d.properties&&(d.properties.name||d.properties.NAME||''); if(nm) pathMap[nm]=pd; return pd; })
    .attr('fill', DFILL).attr('stroke', DSTROKE).attr('stroke-width', 0.6).style('cursor','pointer')
    .on('mousemove', function(event, d) {
      const nm = d.properties&&(d.properties.name||d.properties.NAME||'Département');
      const dd = curData()[nm];
      tip.style.display = 'block';
      const r = mapDiv.getBoundingClientRect();
      tip.style.left = (event.clientX-r.left+14)+'px';
      tip.style.top  = (event.clientY-r.top-36)+'px';
      tip.textContent = nm + (dd ? ' — '+(dd.probable?'~ ':'')+LEVELS[dd.level].label+(dd.picto?' · '+dd.picto:'') : '');
      if (nm !== zoomedDept) d3.select(this).attr('stroke','rgba(255,255,255,.6)').attr('stroke-width',1.2);
    })
    .on('mouseleave', function(event, d) {
      tip.style.display = 'none';
      const nm = d.properties&&(d.properties.name||d.properties.NAME||'');
      if (nm !== zoomedDept) d3.select(this).attr('stroke',DSTROKE).attr('stroke-width',0.6);
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

  svg.on('dblclick', function(event) { if (event.target===svgEl) unzoom(); });

  renderGauges(null);
  // Les cartes "prochain jour" et "probable" sont init au 1er switch de vue
  // mais on les précharge si les données sont dispo, pour un switch instantané
  if (features.length) {
    setTimeout(() => { if (!svgNextEl) initNextMap(); }, 150);
    setTimeout(() => { if (!svgProbEl) initProbMap(); }, 200);
  }
})();

// Fonction utilitaire pour mélanger les couleurs (utilisée pour le mode probable)
function hexBlend(c1, c2, t) {
  const a = hexToRgb(c1), b = hexToRgb(c2);
  return `rgb(${Math.round(a.r*(1-t)+b.r*t)},${Math.round(a.g*(1-t)+b.g*t)},${Math.round(a.b*(1-t)+b.b*t)})`;
}
function hexToRgb(hex) {
  const r = parseInt(hex.slice(1,3),16), g = parseInt(hex.slice(3,5),16), b = parseInt(hex.slice(5,7),16);
  return {r,g,b};
}
</script>
</body>
</html>
