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
.app-hdr{display:flex;align-items:center;justify-content:space-between;padding:10px 16px;background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);flex-wrap:wrap;gap:8px}
.app-title{font-size:14px;font-weight:600;display:flex;align-items:center;gap:8px}
.dot{width:8px;height:8px;border-radius:50%;background:var(--acc);box-shadow:0 0 8px rgba(77,124,254,.6)}
.day-tabs{display:flex;gap:4px}
.dtab{padding:5px 14px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.dtab:hover{background:var(--surf2);color:var(--t1)}
.dtab.on{background:var(--acc);color:#fff;border-color:var(--acc)}
.view-tabs{display:flex;gap:4px;align-items:center}
.vtab{padding:5px 13px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.vtab:hover{background:var(--surf2);color:var(--t1)}
.vtab.on{background:#2a1f3d;color:#c084fc;border-color:#7c3aed}
.vtab-sep{width:1px;height:16px;background:var(--b2);margin:0 4px}
.hdr-right{display:flex;align-items:center;gap:12px;flex-wrap:wrap}

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
.prob-btn{display:none!important}

/* Vue probable */
#map-prob-col{flex:1;min-width:0;position:relative}
#map-prob{width:100%;aspect-ratio:1/1.04;border-radius:12px;overflow:hidden;background:#1a1d27;border:1px solid rgba(124,58,237,.2);position:relative}
#map-prob svg{display:block;width:100%;height:100%}
.prob-sidebar-note{font-size:10px;color:var(--t3);line-height:1.5;padding:8px;background:rgba(124,58,237,.06);border:1px solid rgba(124,58,237,.15);border-radius:8px;margin-bottom:2px}
.prob-level-btn{display:flex;align-items:center;gap:8px;width:100%;padding:6px 8px;margin-bottom:3px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.prob-level-btn:hover{background:var(--surf2);color:var(--t1)}
.prob-level-btn.on{border-color:rgba(124,58,237,.4);background:rgba(124,58,237,.08);color:#c084fc}
#leg-prob{position:absolute;bottom:12px;right:12px;background:rgba(15,17,23,.92);border:1px solid rgba(124,58,237,.25);border-radius:10px;padding:10px 12px;min-width:148px;backdrop-filter:blur(8px);pointer-events:none;display:none}

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
.g-hdr{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:6px}
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

/* Cartes de risque */
.risk-tab{display:flex;gap:4px;align-items:center;margin-top:4px}
.risk-tab .vtab{font-size:11px;padding:4px 12px}
.risk-tab .vtab.on{background:#1a2a1f;color:#4ade80;border-color:#22c55e}
.risk-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:6px;margin-top:8px}
.risk-card{background:var(--surf2);border-radius:var(--rs);padding:8px;border:1px solid var(--b1);display:flex;flex-direction:column;gap:4px}
.risk-card .rc-name{font-size:11px;font-weight:600;color:var(--t1)}
.risk-card .rc-lvl{font-size:10px;color:var(--t2)}
.risk-card .rc-pictos{display:flex;gap:3px;flex-wrap:wrap}
.risk-card .rc-pictos img{width:20px;height:20px;object-fit:contain;border-radius:3px;background:rgba(0,0,0,.3)}
.risk-card .rc-del{background:transparent;border:none;color:var(--t3);cursor:pointer;font-size:12px;align-self:flex-end;margin-top:-4px}
.risk-card .rc-del:hover{color:#ff5f5f}
.risk-upload{display:flex;gap:6px;margin-top:6px;flex-wrap:wrap}
.risk-upload input[type=file]{display:none}
.risk-upload label{padding:6px 14px;border:1px dashed var(--b2);border-radius:20px;cursor:pointer;font-size:11px;color:var(--t2);background:transparent;transition:all .12s}
.risk-upload label:hover{background:var(--surf2);color:var(--t1)}
.risk-upload .act-btn{padding:6px;font-size:11px;width:auto;flex:1;min-width:80px}
#risk-gallery{display:flex;flex-direction:column;gap:8px;margin-top:6px}
.risk-gallery-item{display:flex;align-items:center;gap:10px;padding:6px 10px;background:var(--surf);border:1px solid var(--b1);border-radius:var(--rs)}
.risk-gallery-item .rgi-name{font-size:11px;color:var(--t2);flex:1}
.risk-gallery-item .rgi-del{background:transparent;border:none;color:var(--t3);cursor:pointer;font-size:14px}
.risk-gallery-item .rgi-del:hover{color:#ff5f5f}
</style>
</head>
<body>
<div class="page">

  <div class="app-hdr">
    <div class="app-title"><span class="dot"></span>Carte Vigilance Météo France</div>
    <div class="hdr-right">
      <div class="view-tabs">
        <button class="vtab on" id="vtab-vigil" onclick="switchView('vigilance')">Vigilance</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-prob" onclick="switchView('probable')" style="color:#7c6a9a;border-color:rgba(124,58,237,.3)">↗ Évolution probable</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-risk" onclick="switchView('risk')" style="color:#4ade80;border-color:rgba(34,197,94,.3)">📊 Cartes risque</button>
      </div>
      <div class="day-tabs" id="day-tabs-wrap">
        <button class="dtab on" id="dtab-today" onclick="switchDay('today')">Aujourd'hui</button>
        <button class="dtab" id="dtab-tomorrow" onclick="switchDay('tomorrow')">Demain</button>
      </div>
    </div>
  </div>

  <!-- VUE VIGILANCE -->
  <div class="top" id="view-vigilance">
    <div id="map-col">
      <div id="map" style="width:100%;border-radius:12px;overflow:hidden;background:#1a1d27;border:1px solid rgba(255,255,255,.06)"></div>
      <div class="tip" id="tip"></div>
      <div id="leg"></div>
    </div>
    <div class="sb">
      <div class="card"><p class="stitle">Niveau d'alerte</p><div id="lvl-container"></div></div>
      <div class="card"><p class="stitle">Phénomène</p><div class="pgrid" id="pgrid"></div></div>
      <div class="card">
        <p class="stitle" style="margin-bottom:6px">Submersion marine</p>
        <div class="tog-row" onclick="toggleSub()"><span style="font-size:11px;color:var(--t2)">Zones côtières</span><div class="tog" id="sub-tog"><div class="tog-k"></div></div></div>
        <div class="sub-lvls" id="sub-lvls" style="display:none">
          <button class="slvl on" data-sl="vert" onclick="setSubLvl('vert')">Vert</button>
          <button class="slvl" data-sl="jaune" onclick="setSubLvl('jaune')">Jaune</button>
          <button class="slvl" data-sl="orange" onclick="setSubLvl('orange')">Orange</button>
          <button class="slvl" data-sl="rouge" onclick="setSubLvl('rouge')">Rouge</button>
        </div>
      </div>
      <div class="card">
        <p class="stitle" style="margin-bottom:6px">Double vigilance</p>
        <button class="dv-tog" onclick="toggleDV()"><span style="font-size:11px;color:var(--t2)">Activer 2ᵉ niveau</span><div class="tog" id="dv-tog"><div class="tog-k"></div></div></button>
        <div id="dv-opts" style="display:none;margin-top:8px">
          <p style="font-size:10px;color:var(--t3);margin-bottom:5px">2ᵉ niveau</p><div id="dv-lvls"></div>
          <p style="font-size:10px;color:var(--t3);margin-top:6px;margin-bottom:4px">2ᵉ phénomène <span style="opacity:.5">(opt.)</span></p>
          <div class="pgrid" id="dv-pgrid"></div>
        </div>
      </div>
      <p class="hint">Clic = appliquer · Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetAll()">↺ Réinitialiser</button>
      <button class="act-btn primary" onclick="dlMapPNG()">↓ Télécharger PNG</button>
    </div>
  </div>

  <!-- VUE ÉVOLUTION PROBABLE -->
  <div class="top" id="view-probable" style="display:none">
    <div id="map-prob-col">
      <div id="map-prob"></div>
      <div class="tip" id="tip-prob"></div>
      <div id="leg-prob"></div>
    </div>
    <div class="sb">
      <div class="card" style="border-color:rgba(124,58,237,.2)">
        <p class="stitle" style="color:#7c6a9a">Niveau cible probable</p>
        <p class="prob-sidebar-note">Colorez les départements susceptibles de passer en vigilance supérieure.</p>
        <div id="prob-lvl-container" style="margin-top:8px"></div>
      </div>
      <p class="hint" style="color:var(--t3)">Clic = appliquer · Re-clic = effacer<br>Dbl-clic = zoom</p>
      <button class="rst-btn" onclick="resetProb()" style="border-color:rgba(124,58,237,.2);color:#7c6a9a">↺ Réinitialiser</button>
      <button class="act-btn" onclick="dlMapProbPNG()" style="border-color:rgba(124,58,237,.3);color:#c084fc">↓ Télécharger PNG</button>
    </div>
  </div>

  <!-- VUE CARTES DE RISQUE -->
  <div class="top" id="view-risk" style="display:none">
    <div style="flex:1;display:flex;flex-direction:column;gap:12px">
      <div class="card" style="border-color:rgba(34,197,94,.3)">
        <p class="stitle" style="color:#4ade80">📤 Importer une carte</p>
        <div class="risk-upload">
          <input type="file" id="riskFileInput" accept="image/*" onchange="importRiskCard(event)">
          <label for="riskFileInput">📁 Choisir une image</label>
          <button class="act-btn" onclick="document.getElementById('riskFileInput').click()" style="border-color:rgba(34,197,94,.3);color:#4ade80">Importer</button>
        </div>
        <div style="margin-top:8px;font-size:10px;color:var(--t3)">Format PNG, JPG ou WEBP. La carte sera ajoutée à votre galerie.</div>
      </div>
      <div class="card" style="border-color:rgba(34,197,94,.3);flex:1">
        <p class="stitle" style="color:#4ade80">🖼️ Ma galerie de cartes</p>
        <div id="risk-gallery"></div>
      </div>
    </div>
    <div class="sb">
      <div class="card" style="border-color:rgba(34,197,94,.3)">
        <p class="stitle" style="color:#4ade80">📊 Cartes par région</p>
        <div style="font-size:10px;color:var(--t3);margin-bottom:6px">Cliquez sur une région pour appliquer un risque</div>
        <div id="risk-region-grid" style="display:grid;grid-template-columns:1fr 1fr;gap:4px"></div>
      </div>
    </div>
  </div>

  <div id="gauges"></div>
</div>

<div class="pop" id="lvl-pop"></div>
<div class="pop" id="phen-pop"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js"></script>
<script>
/* ── PICTO URLS ── */
const PICTO_URLS = {
  vent:'https://vigilance.meteofrance.fr/images/picto/vent.png', orage:'https://vigilance.meteofrance.fr/images/picto/orage.png',
  pluies:'https://vigilance.meteofrance.fr/images/picto/pluie-inondation.png', inondation:'https://vigilance.meteofrance.fr/images/picto/inondation.png',
  neige:'https://vigilance.meteofrance.fr/images/picto/neige.png', verglas:'https://vigilance.meteofrance.fr/images/picto/verglas.png',
  froid:'https://vigilance.meteofrance.fr/images/picto/grand-froid.png', chaleur:'https://vigilance.meteofrance.fr/images/picto/canicule.png',
  vagues:'https://vigilance.meteofrance.fr/images/picto/vague-submersion.png', avalanches:'https://vigilance.meteofrance.fr/images/picto/avalanche.png',
  neigeverglas:'https://vigilance.meteofrance.fr/images/picto/neige-verglas.png'
};
const PICTO_FALLBACK = {
  vent:'https://i.imgur.com/w9U4uxl.png', orage:'https://i.imgur.com/TqMfDUO.png',
  pluies:'https://i.imgur.com/doeHp1Y.png', inondation:'https://i.imgur.com/b6bctj5.png',
  neige:'https://i.imgur.com/g0IsPe2.png', verglas:'https://i.imgur.com/EJKnm3Q.png',
  froid:'https://i.imgur.com/nCPwfFD.png', chaleur:'https://i.imgur.com/EGeV2X0.png',
  vagues:'https://i.imgur.com/u5zMvyy.png', avalanches:'https://i.imgur.com/KZQXNzU.png',
  neigeverglas:'https://i.imgur.com/59zrRen.png'
};
const resolvedUrls = {};
Object.keys(PICTO_FALLBACK).forEach(k => resolvedUrls[k] = PICTO_FALLBACK[k]);
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
  { k:'vert', l:'Vert', semi:false },
  { k:'pre-alerte', l:'Pré-alerte', semi:false },
  { k:'jaune', l:'Jaune', semi:false },
  { k:'jaune-orange', l:'Semi J/O', semi:true },
  { k:'orange', l:'Orange', semi:false },
  { k:'orange-rouge', l:'Semi O/R', semi:true },
  { k:'rouge', l:'Rouge', semi:false },
  { k:'violet', l:'Violet', semi:false }
];
const COASTAL = new Set(['Manche','Calvados','Seine-Maritime','Somme','Pas-de-Calais','Nord','Finistère',"Côtes-d'Armor",'Ille-et-Vilaine','Morbihan','Loire-Atlantique','Vendée','Charente-Maritime','Gironde','Landes','Hérault','Gard','Bouches-du-Rhône','Var','Alpes-Maritimes','Pyrénées-Orientales','Aude','Corse-du-Sud','Haute-Corse']);

const DFILL = '#1e2535';
const DSTROKE = 'rgba(255,255,255,.14)';

/* ── ÉTATS ── */
let activeLevel = 'vert';
let activePicto = null;
let probLevel = null;
let deptData = { today: {}, tomorrow: {} };
let subVisible = false, subLvl = 'vert';
let dvActive = false, dvLevel = 'orange', dvPicto = null;
let zoomedDept = null;
let activeDay = 'today';
let svgEl=null, svgSel=null, geoPath=null, layerSel=null;
let features=[], pathMap={};
let probData = {};
let probActiveLevel = 'orange';
let probActivePicto = null;
let svgProbEl=null, svgProbSel=null, layerProbSel=null;
let zoomedDeptProb = null;
let activeView = 'vigilance';
let riskCards = []; // { id, name, dataUrl, regions: { regionName: level } }

/* ── SWITCH VUE ── */
function switchView(view) {
  activeView = view;
  document.getElementById('view-vigilance').style.display = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('view-probable').style.display = view === 'probable' ? 'flex' : 'none';
  document.getElementById('view-risk').style.display = view === 'risk' ? 'flex' : 'none';
  document.getElementById('day-tabs-wrap').style.display = view === 'vigilance' ? 'flex' : 'none';
  document.getElementById('vtab-vigil').classList.toggle('on', view === 'vigilance');
  document.getElementById('vtab-prob').classList.toggle('on', view === 'probable');
  document.getElementById('vtab-risk').classList.toggle('on', view === 'risk');
  if (view === 'probable' && !svgProbEl) initProbMap();
  if (view === 'probable') redrawProbMap();
  if (view === 'risk') renderRiskView();
}
function switchDay(d) { activeDay = d; document.getElementById('dtab-today').classList.toggle('on', d === 'today'); document.getElementById('dtab-tomorrow').classList.toggle('on', d === 'tomorrow'); redrawMap(); renderGauges(zoomedDept); }

const gaugeData = {
  national: { today: [{ picto: 'orage', segs: [{ s:6, e:15, lv:'jaune' }, { s:18, e:24, lv:'orange' }] }], tomorrow: [{ picto: null, segs: [] }] }
};
function gRows(scope) { const k = scope || 'national'; if (!gaugeData[k]) gaugeData[k] = { today:[{picto:null,segs:[]}], tomorrow:[{picto:null,segs:[]}] }; return gaugeData[k][activeDay]; }

function curData() { return deptData[activeDay]; }

/* ── BUILD SIDEBARS ── */
function buildSidebar() {
  const c = document.getElementById('lvl-container');
  LVL_DEFS.forEach(({ k, l, semi }) => {
    const row = document.createElement('div'); row.className = 'lvl-row-wrap';
    const btn = document.createElement('button'); btn.className = 'lvl-btn' + (k === 'vert' ? ' on' : '') + (semi ? ' semi' : '');
    btn.dataset.lk = k;
    const yc = (k === 'pre-alerte' || k === 'jaune' || k === 'jaune-orange') ? 'border:1px solid rgba(0,0,0,.4)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color};${yc}"></span>${l}`;
    btn.onclick = () => selectLevel(k);
    row.appendChild(btn);
    c.appendChild(row);
  });
  buildPictoGrid('pgrid', k => { activePicto = activePicto === k ? null : k; document.querySelectorAll('#pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k)); }, 'pbtn');
  const dvc = document.getElementById('dv-lvls');
  LVL_DEFS.filter(d => d.k !== 'vert').forEach(({ k, l }) => {
    const btn = document.createElement('button'); btn.className = 'lvl-btn' + (k === dvLevel ? ' on' : ''); btn.dataset.dvk = k;
    btn.style.fontSize = '11px'; btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${l}`;
    btn.onclick = () => { dvLevel = k; document.querySelectorAll('[data-dvk]').forEach(b => b.classList.toggle('on', b.dataset.dvk === k)); };
    dvc.appendChild(btn);
  });
  buildPictoGrid('dv-pgrid', k => { dvPicto = dvPicto === k ? null : k; document.querySelectorAll('#dv-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k)); }, 'pbtn dv-pbtn');
}
function buildPictoGrid(id, onclick, cls) {
  const g = document.getElementById(id);
  if (!g) return;
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button'); btn.className = cls; btn.dataset.ik = k;
    btn.innerHTML = ico(k, 26) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => onclick(k);
    g.appendChild(btn);
  });
}
function selectLevel(l) { activeLevel = l; document.querySelectorAll('[data-lk]').forEach(b => b.classList.toggle('on', b.dataset.lk === l)); }
function toggleSub() { subVisible = !subVisible; document.getElementById('sub-tog').classList.toggle('on', subVisible); document.getElementById('sub-lvls').style.display = subVisible ? 'flex' : 'none'; drawSub(); }
function setSubLvl(l) { subLvl = l; document.querySelectorAll('[data-sl]').forEach(b => b.classList.toggle('on', b.dataset.sl === l)); if (subVisible) drawSub(); }
function toggleDV() { dvActive = !dvActive; document.getElementById('dv-tog').classList.toggle('on', dvActive); document.getElementById('dv-opts').style.display = dvActive ? 'block' : 'none'; }
function resetAll() { deptData[activeDay] = {}; redrawMap(); }

/* ── SIDEBAR PROBABLE ── */
function buildProbSidebar() {
  const c = document.getElementById('prob-lvl-container');
  const probLevels = ['jaune','jaune-orange','orange','orange-rouge','rouge','violet'];
  probLevels.forEach(k => {
    const btn = document.createElement('button');
    btn.className = 'prob-level-btn' + (k === probActiveLevel ? ' on' : '');
    btn.dataset.plk = k;
    const yc = (k === 'jaune' || k === 'jaune-orange') ? 'border:1px solid rgba(0,0,0,.4)' : '';
    btn.innerHTML = `<span class="lvl-dot" style="background:${LEVELS[k].color};${yc}"></span>${LEVELS[k].label}`;
    btn.onclick = () => { probActiveLevel = k; document.querySelectorAll('[data-plk]').forEach(b => b.classList.toggle('on', b.dataset.plk === k)); };
    c.appendChild(btn);
  });
  const pictoCard = document.createElement('div'); pictoCard.className = 'card'; pictoCard.style.borderColor = 'rgba(124,58,237,.2)'; pictoCard.style.marginTop = '8px';
  const pTitle = document.createElement('p'); pTitle.className = 'stitle'; pTitle.style.color = '#7c6a9a'; pTitle.textContent = 'Phénomène probable'; pictoCard.appendChild(pTitle);
  const pGrid = document.createElement('div'); pGrid.className = 'pgrid'; pGrid.id = 'prob-pgrid'; pictoCard.appendChild(pGrid);
  c.parentNode.insertBefore(pictoCard, c.nextSibling);
  const grid = document.getElementById('prob-pgrid');
  Object.keys(PICTO_FALLBACK).forEach(k => {
    const btn = document.createElement('button'); btn.className = 'pbtn'; btn.dataset.ik = k;
    btn.innerHTML = ico(k, 26) + `<span>${k.slice(0,7)}</span>`;
    btn.onclick = () => { probActivePicto = probActivePicto === k ? null : k; document.querySelectorAll('#prob-pgrid .pbtn').forEach(b => b.classList.toggle('on', b.dataset.ik === k)); };
    grid.appendChild(btn);
  });
}

/* ── CARTE PROBABLE ── */
function initProbMap() {
  if (!features.length) return;
  const mapDiv = document.getElementById('map-prob');
  d3.select(mapDiv).selectAll('svg').remove();
  const W=600, H=624;
  const svg = d3.select(mapDiv).append('svg').attr('viewBox',`0 0 ${W} ${H}`).attr('preserveAspectRatio','xMidYMid meet').style('display','block').style('width','100%').style('height','100%');
  svgProbEl = svg.node(); svgProbSel = svg;
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
      if (info) { txt += ` — Probable : ${LEVELS[info.level].label}`; if (info.picto) txt += ` · ${info.picto}`; } else txt += ' — Pas de changement';
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
  updateLegProb();
}

function redrawProbPath(sel, nm) {
  const info = probData[nm];
  const isZoomed = nm === zoomedDeptProb;
  if (info) {
    sel.attr('fill', LEVELS[info.level].color).attr('stroke', isZoomed ? '#fff' : 'rgba(0,0,0,.2)').attr('stroke-width', isZoomed ? 2.5 : 0.6);
  } else {
    sel.attr('fill', '#1e2535').attr('stroke', isZoomed ? '#fff' : 'rgba(255,255,255,.14)').attr('stroke-width', isZoomed ? 2.5 : 0.6);
  }
}
function redrawProbMap() {
  if (!layerProbSel) return;
  layerProbSel.selectAll('path.dept-prob').each(function(d) { const nm = d.properties&&(d.properties.name||d.properties.NAME||''); redrawProbPath(d3.select(this), nm); });
  layerProbSel.selectAll('.dept-picto-prob').remove();
  Object.entries(probData).forEach(([name, info]) => {
    if (!info.picto || !PICTO_FALLBACK[info.picto]) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === name);
    if (!feat) return;
    const b = geoPath.bounds(feat);
    const cx = (b[0][0] + b[1][0]) / 2, cy = (b[0][1] + b[1][1]) / 2;
    const sz = Math.max(12, Math.min((Math.min(b[1][0]-b[0][0], b[1][1]-b[0][1])) * 0.52, 26));
    const url = resolvedUrls[info.picto] || PICTO_FALLBACK[info.picto];
    layerProbSel.append('image').attr('class','dept-picto-prob').attr('href',url).attr('x',cx-sz/2).attr('y',cy-sz/2).attr('width',sz).attr('height',sz).attr('pointer-events','none').style('image-rendering','auto');
  });
  updateLegProb();
}
function zoomToProb(feat, name) {
  const svgNode = svgProbEl;
  const viewBox = svgNode.getAttribute('viewBox').split(' ').map(Number);
  const W = viewBox[2] || 600, H = viewBox[3] || 624;
  zoomedDeptProb = name;
  layerProbSel.selectAll('path.dept-prob').attr('stroke','rgba(255,255,255,.14)').attr('stroke-width',0.6);
  layerProbSel.selectAll('path.dept-prob').filter(d=>(d.properties.name||d.properties.NAME)===name).raise().attr('stroke','#fff').attr('stroke-width',2.5);
  const b = geoPath.bounds(feat);
  const sc = Math.min(0.82*W/(b[1][0]-b[0][0]), 0.82*H/(b[1][1]-b[0][1]), 6);
  const cx=(b[0][0]+b[1][0])/2, cy=(b[0][1]+b[1][1])/2;
  layerProbSel.transition().duration(480).ease(d3.easeCubicInOut).attr('transform',`translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
}
function unzoomProb() { zoomedDeptProb = null; layerProbSel.selectAll('path.dept-prob').attr('stroke','rgba(255,255,255,.14)').attr('stroke-width',0.6); layerProbSel.transition().duration(450).ease(d3.easeCubicInOut).attr('transform',''); redrawProbMap(); }
function resetProb() { probData = {}; redrawProbMap(); }
function updateLegProb() {
  const el = document.getElementById('leg-prob');
  const used = Object.values(probData);
  if (!used.length) { el.style.display='none'; return; }
  el.style.display = 'block';
  let h = `<div style="font-size:9px;font-weight:600;text-transform:uppercase;letter-spacing:.07em;color:#7c6a9a;margin-bottom:6px">Évolution probable</div>`;
  const lvls = [...new Set(used.map(d => d.level))];
  lvls.forEach(lv => { h += `<div class="prob-legend-item"><span class="prob-legend-dot" style="background:${LEVELS[lv].color}"></span><span style="font-size:11px;color:var(--t2)">${LEVELS[lv].label}</span></div>`; });
  const pics = [...new Set(used.filter(d=>d.picto).map(d=>d.picto))];
  if (pics.length) {
    h += `<div style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)"><div style="font-size:8px;text-transform:uppercase;letter-spacing:.06em;color:var(--t3);margin-bottom:4px">Phénomènes</div><div style="display:flex;flex-wrap:wrap;gap:6px">`;
    pics.forEach(p => { h += `<div style="display:flex;flex-direction:column;align-items:center;gap:1px;width:28px">${ico(p,20)}<span style="font-size:7px;color:var(--t3)">${p.slice(0,5)}</span></div>`; });
    h += `</div></div>`;
  }
  const neutral = features.length - Object.keys(probData).length;
  if (neutral > 0) { h += `<div class="prob-legend-item" style="margin-top:6px;padding-top:6px;border-top:1px solid var(--b1)"><span class="prob-legend-dot" style="background:#1e2535;border:1px solid rgba(255,255,255,.15)"></span><span style="font-size:10px;color:var(--t3)">Pas de changement</span></div>`; }
  el.innerHTML = h;
}

/* ── REDRAW MAP ── */
function redrawMap() {
  if (!layerSel) return;
  const data = curData();
  layerSel.selectAll('path.dept')
    .attr('fill', d => {
      const n = d.properties && (d.properties.name || d.properties.NAME || '');
      const dd = data[n];
      if (!dd) return DFILL;
      if (dd.probable) return hexBlend(LEVELS[dd.level].color, '#1e2535', 0.18);
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
  layerSel.selectAll('.dept-picto-g').remove();
  Object.entries(data).forEach(([name, dd]) => {
    if (!dd.picto || !PICTO_FALLBACK[dd.picto]) return;
    const feat = features.find(f => (f.properties.name || f.properties.NAME) === name);
    if (!feat) return;
    const b = geoPath.bounds(feat);
    const cx = (b[0][0] + b[1][0]) / 2, cy = (b[0][1] + b[1][1]) / 2;
    const sz = Math.max(12, Math.min((Math.min(b[1][0]-b[0][0], b[1][1]-b[0][1])) * 0.52, 26));
    const url = resolvedUrls[dd.picto] || PICTO_FALLBACK[dd.picto];
    layerSel.append('image').attr('class','dept-picto-g').attr('href',url).attr('x',cx-sz/2).attr('y',cy-sz/2).attr('width',sz).attr('height',sz).attr('pointer-events','none').style('image-rendering','auto');
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
    const b = geoPath.bounds(feat);
    const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
    const w2 = b[1][0]-b[0][0], h2 = b[1][1]-b[0][1];
    const bx = cx+w2*0.25, by = cy-h2*0.25;
    const g = layerSel.append('g').attr('class','dv-badge').attr('pointer-events','none');
    g.append('circle').attr('cx',bx).attr('cy',by).attr('r',8).attr('fill',LEVELS[dd.dv.level].color).attr('stroke','white').attr('stroke-width',1.5);
    if (dd.dv.picto && PICTO_FALLBACK[dd.dv.picto]) {
      const url2 = resolvedUrls[dd.dv.picto] || PICTO_FALLBACK[dd.dv.picto];
      g.append('image').attr('href',url2).attr('x',bx-6).attr('y',by-6).attr('width',12).attr('height',12).style('image-rendering','auto');
    } else {
      g.append('text').attr('x',bx).attr('y',by).attr('text-anchor','middle').attr('dominant-baseline','central').attr('font-size',9).attr('font-weight',700).attr('fill','white').text('2');
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
      h += `<div class="leg-r"><span style="width:13px;height:13px;border-radius:3px;flex-shrink:0;display:inline-block;background:repeating-linear-gradient(45deg,${c}2a 0,${c}2a 2px,${c}99 2px,${c}99 5px,${c}2a 5px,${c}2a 8px);border:1px solid ${c}88"></span><span class="leg-lbl" style="color:#ffb400">~ ${LEVELS[lv].label}</span></div>`;
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
function hexBlend(c1, c2, t) {
  const r1=parseInt(c1.slice(1,2),16)*17||parseInt(c1.slice(1,3),16), g1=parseInt(c1.slice(2,3),16)*17||parseInt(c1.slice(3,5),16), b1=parseInt(c1.slice(3,4),16)*17||parseInt(c1.slice(5,7),16);
  const r2=parseInt(c2.slice(1,2),16)*17||parseInt(c2.slice(1,3),16), g2=parseInt(c2.slice(2,3),16)*17||parseInt(c2.slice(3,5),16), b2=parseInt(c2.slice(3,4),16)*17||parseInt(c2.slice(5,7),16);
  const r=Math.round(r1*t+r2*(1-t)), g=Math.round(g1*t+g2*(1-t)), b=Math.round(b1*t+b2*(1-t));
  return `#${r.toString(16).padStart(2,'0')}${g.toString(16).padStart(2,'0')}${b.toString(16).padStart(2,'0')}`;
}
function zoomTo(feat, name) {
  if (!svgSel || !geoPath) return;
  const [,, W, H] = svgEl.getAttribute('viewBox').split(' ').map(Number);
  zoomedDept = name;
  layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6);
  layerSel.selectAll('path.dept').filter(d => (d.properties.name || d.properties.NAME) === name).raise().attr('stroke','#fff').attr('stroke-width', 2.5);
  const b = geoPath.bounds(feat);
  const cx = (b[0][0]+b[1][0])/2, cy = (b[0][1]+b[1][1])/2;
  const sc = Math.min(0.82*W/(b[1][0]-b[0][0]), 0.82*H/(b[1][1]-b[0][1]), 6);
  layerSel.transition().duration(480).ease(d3.easeCubicInOut).attr('transform', `translate(${W/2-sc*cx},${H/2-sc*cy}) scale(${sc})`);
  renderGauges(name);
}
function unzoom() { if (!svgSel) return; zoomedDept = null; layerSel.selectAll('path.dept').attr('stroke', DSTROKE).attr('stroke-width', 0.6); layerSel.transition().duration(450).ease(d3.easeCubicInOut).attr('transform',''); renderGauges(null); }

/* ── TÉLÉCHARGEMENT PNG ── */
async function svgToPNG(svgElement, filename) {
  const svgData = new XMLSerializer().serializeToString(svgElement);
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');
  const viewBox = svgElement.getAttribute('viewBox').split(' ').map(Number);
  const w = viewBox[2] || 600, h = viewBox[3] || 624;
  canvas.width = w * 2; canvas.height = h * 2;
  ctx.scale(2, 2);
  const img = new Image();
  img.src = 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svgData);
  await new Promise(r => img.onload = r);
  ctx.drawImage(img, 0, 0, w, h);
  const a = document.createElement('a');
  a.href = canvas.toDataURL('image/png');
  a.download = filename;
  a.click();
}
function dlMapPNG() { if (svgEl) svgToPNG(svgEl, 'vigilance.png'); }
function dlMapProbPNG() { if (svgProbEl) svgToPNG(svgProbEl, 'evolution-probable.png'); }

/* ── JAUGES ── */
function renderGauges(scope) {
  const sec = document.getElementById('gauges');
  sec.innerHTML = '';
  const rows = gRows(scope);
  const wrap = document.createElement('div');
  wrap.className = 'g-wrap' + (scope ? ' dept' : '');
  const hdr = document.createElement('div'); hdr.className = 'g-hdr';
  const ttl = document.createElement('div'); ttl.className = 'g-title';
  const dayLabel = activeDay === 'today' ? "Aujourd'hui" : 'Demain';
  ttl.innerHTML = scope ? `${scope} <span class="g-badge">Département</span>` : `France <span class="g-badge">National · ${dayLabel}</span>`;
  hdr.appendChild(ttl);
  if (scope) { const bk = document.createElement('button'); bk.className = 'g-back'; bk.textContent = '\u2190 Vue nationale'; bk.onclick = () => unzoom(); hdr.appendChild(bk); }
  wrap.appendChild(hdr);
  const ticks = document.createElement('div'); ticks.className = 'g-ticks';
  [0,3,6,9,12,15,18,21,24].forEach(h => { const t = document.createElement('span'); t.className = 'g-tick'; t.textContent = h+'h'; ticks.appendChild(t); });
  wrap.appendChild(ticks);
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
  const pw = document.createElement('div'); pw.className = 'g-picto';
  pw.innerHTML = row.picto ? ico(row.picto, 20) : '<span style="font-size:13px;color:var(--t3)">+</span>';
  pw.onclick = e => openPhenPop(e, scope, ri);
  el.appendChild(pw);
  const lbl = document.createElement('div'); lbl.className = 'g-lbl';
  lbl.textContent = row.picto || '—'; el.appendChild(lbl);
  const tl = document.createElement('div'); tl.className = 'g-tl';
  const bg = document.createElement('div'); bg.className = 'g-bg'; tl.appendChild(bg);
  const ns = 'http://www.w3.org/2000/svg';
  const svgT = document.createElementNS(ns,'svg');
  svgT.style.cssText = 'position:absolute;top:0;left:0;width:100%;height:100%;overflow:visible;pointer-events:none';
  svgT.setAttribute('preserveAspectRatio','none');
  tl.appendChild(svgT);
  row.segs.forEach((seg, si) => drawSeg(svgT, tl, scope, ri, si, seg));
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
document.addEventListener('click', () => { document.getElementById('lvl-pop').classList.remove('open'); document.getElementById('phen-pop').classList.remove('open'); });
document.addEventListener('keydown', e => { if (e.key==='Escape') { document.getElementById('lvl-pop').classList.remove('open'); document.getElementById('phen-pop').classList.remove('open'); if(zoomedDept) unzoom(); } });

/* ── RISK VIEW ── */
const OLD_REGIONS = [
  'Alsace', 'Aquitaine', 'Auvergne', 'Basse-Normandie', 'Bourgogne', 'Bretagne',
  'Centre', 'Champagne-Ardenne', 'Corse', 'Franche-Comté', 'Haute-Normandie',
  'Île-de-France', 'Languedoc-Roussillon', 'Limousin', 'Lorraine', 'Midi-Pyrénées',
  'Nord-Pas-de-Calais', 'Pays de la Loire', 'Picardie', 'Poitou-Charentes',
  'Provence-Alpes-Côte d\'Azur', 'Rhône-Alpes'
];
const REGION_DEPTS = {
  'Alsace': ['Bas-Rhin','Haut-Rhin'],
  'Aquitaine': ['Dordogne','Gironde','Landes','Lot-et-Garonne','Pyrénées-Atlantiques'],
  'Auvergne': ['Allier','Cantal','Haute-Loire','Puy-de-Dôme'],
  'Basse-Normandie': ['Calvados','Manche','Orne'],
  'Bourgogne': ['Côte-d\'Or','Nièvre','Saône-et-Loire','Yonne'],
  'Bretagne': ['Côtes-d\'Armor','Finistère','Ille-et-Vilaine','Morbihan'],
  'Centre': ['Cher','Eure-et-Loir','Indre','Indre-et-Loire','Loir-et-Cher','Loiret'],
  'Champagne-Ardenne': ['Ardennes','Aube','Haute-Marne','Marne'],
  'Corse': ['Corse-du-Sud','Haute-Corse'],
  'Franche-Comté': ['Doubs','Haute-Saône','Jura','Territoire de Belfort'],
  'Haute-Normandie': ['Eure','Seine-Maritime'],
  'Île-de-France': ['Essonne','Hauts-de-Seine','Paris','Seine-Saint-Denis','Seine-et-Marne','Val-de-Marne','Val-d\'Oise','Yvelines'],
  'Languedoc-Roussillon': ['Aude','Gard','Hérault','Lozère','Pyrénées-Orientales'],
  'Limousin': ['Corrèze','Creuse','Haute-Vienne'],
  'Lorraine': ['Meurthe-et-Moselle','Meuse','Moselle','Vosges'],
  'Midi-Pyrénées': ['Ariège','Aveyron','Haute-Garonne','Gers','Lot','Hautes-Pyrénées','Tarn','Tarn-et-Garonne'],
  'Nord-Pas-de-Calais': ['Nord','Pas-de-Calais'],
  'Pays de la Loire': ['Loire-Atlantique','Maine-et-Loire','Mayenne','Sarthe','Vendée'],
  'Picardie': ['Aisne','Oise','Somme'],
  'Poitou-Charentes': ['Charente','Charente-Maritime','Deux-Sèvres','Vienne'],
  'Provence-Alpes-Côte d\'Azur': ['Alpes-de-Haute-Provence','Hautes-Alpes','Alpes-Maritimes','Bouches-du-Rhône','Var','Vaucluse'],
  'Rhône-Alpes': ['Ain','Ardèche','Drôme','Isère','Loire','Rhône','Savoie','Haute-Savoie']
};
const RISK_LEVELS = ['Faible','Modéré','Élevé','Très élevé','Extrême'];
const RISK_COLORS = ['#31AA35','#FFF600','#FFB82B','#D13A10','#CC0000'];

function renderRiskView() {
  // Régions
  const grid = document.getElementById('risk-region-grid');
  grid.innerHTML = '';
  OLD_REGIONS.forEach(r => {
    const btn = document.createElement('button');
    btn.style.cssText = `padding:6px 8px;border:1px solid var(--b2);border-radius:5px;background:transparent;color:var(--t2);font-size:10px;font-family:inherit;cursor:pointer;text-align:left;transition:all .12s`;
    btn.textContent = r;
    btn.onmouseover = () => { btn.style.background = 'var(--surf2)'; btn.style.color = 'var(--t1)'; };
    btn.onmouseout = () => { btn.style.background = 'transparent'; btn.style.color = 'var(--t2)'; };
    btn.onclick = () => applyRiskToRegion(r);
    grid.appendChild(btn);
  });
  renderRiskGallery();
}

function applyRiskToRegion(regionName) {
  const depts = REGION_DEPTS[regionName] || [];
  if (!depts.length) return;
  const data = curData();
  // Niveau aléatoire entre 0 et 4 pour la démo
  const lvlIdx = Math.floor(Math.random() * 3) + 1; // 1=Modéré, 2=Élevé, 3=Très élevé
  const level = ['jaune','orange','rouge','violet'][lvlIdx-1] || 'orange';
  const pictos = Object.keys(PICTO_FALLBACK);
  const picto = pictos[Math.floor(Math.random() * pictos.length)];
  depts.forEach(d => {
    data[d] = { level: level, picto: picto, probable: false };
  });
  redrawMap();
  renderGauges(zoomedDept);
  // Notification visuelle
  const grid = document.getElementById('risk-region-grid');
  grid.querySelectorAll('button').forEach(b => {
    if (b.textContent === regionName) {
      b.style.borderColor = '#4ade80';
      b.style.color = '#4ade80';
      setTimeout(() => { b.style.borderColor = 'var(--b2)'; b.style.color = 'var(--t2)'; }, 1500);
    }
  });
}

let riskCardId = 0;
function importRiskCard(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    const dataUrl = e.target.result;
    const name = file.name.split('.')[0] || `Carte ${riskCardId+1}`;
    riskCards.push({ id: ++riskCardId, name, dataUrl });
    renderRiskGallery();
  };
  reader.readAsDataURL(file);
  event.target.value = '';
}
function renderRiskGallery() {
  const gallery = document.getElementById('risk-gallery');
  if (!riskCards.length) {
    gallery.innerHTML = `<div style="font-size:11px;color:var(--t3);padding:20px;text-align:center">Aucune carte importée.<br>Utilisez le bouton ci-dessus pour ajouter une carte.</div>`;
    return;
  }
  let html = `<div class="risk-grid">`;
  riskCards.forEach(c => {
    html += `<div class="risk-card">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <span class="rc-name">${c.name}</span>
        <button class="rc-del" onclick="deleteRiskCard(${c.id})">✕</button>
      </div>
      <img src="${c.dataUrl}" style="width:100%;height:auto;max-height:120px;object-fit:contain;border-radius:4px;background:var(--bg)">
      <div style="font-size:9px;color:var(--t3);margin-top:2px">Cliquez sur l'image pour l'agrandir</div>
    </div>`;
  });
  html += `</div>`;
  gallery.innerHTML = html;
}
function deleteRiskCard(id) {
  riskCards = riskCards.filter(c => c.id !== id);
  renderRiskGallery();
}

/* ── INIT ── */
buildSidebar();
buildProbSidebar();

(async () => {
  const mapDiv = document.getElementById('map');
  const tip = document.getElementById('tip');
  let topo;
  try { topo = await d3.json('https://cdn.jsdelivr.net/npm/datamaps@0.5.10/src/js/data/fra.topo.json'); }
  catch(e) { mapDiv.innerHTML = '<p style="color:var(--t3);text-align:center;padding:60px">Données indisponibles.</p>'; return; }
  const key = Object.keys(topo.objects)[0];
  features = topojson.feature(topo, topo.objects[key]).features;
  const W=600, H=624;
  const proj = d3.geoConicConformal().parallels([44,49]).rotate([-3,0]).center([0,46.5]).scale(1860).translate([300,312]);
  geoPath = d3.geoPath().projection(proj);
  const svg = d3.select(mapDiv).append('svg')
    .attr('viewBox',`0 0 ${W} ${H}`)
    .attr('preserveAspectRatio','xMidYMid meet')
    .style('display','block').style('width','100%').style('height','auto');
  svgEl = svg.node(); svgSel = svg;
  svg.append('rect').attr('width',W).attr('height',H).attr('fill','#131823');
  svg.append('defs');
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
  // Pré-init probable
  setTimeout(() => { if (!svgProbEl) initProbMap(); }, 200);
})();
</script>
</body>
</html>
