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
#map-col{flex:1;min-width:0;position:relative;height:624px;background:#131823;border-radius:12px;border:1px solid var(--b1);overflow:hidden}
.sb{width:220px;flex-shrink:0;display:flex;flex-direction:column;gap:8px}
.card{background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:12px}
.stitle{font-size:10px;font-weight:600;text-transform:uppercase;letter-spacing:.08em;color:var(--t3);margin-bottom:8px}

/* Level rows */
.lvl-row-wrap{display:flex;align-items:center;gap:3px;margin-bottom:2px}
.lvl-btn{display:flex;align-items:center;gap:7px;flex:1;padding:5px 7px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);text-align:left;transition:all .12s}
.lvl-btn:hover{background:var(--surf2);color:var(--t1)}
.lvl-btn.on{border-color:var(--b2);background:var(--surf2);color:var(--t1)}
.lvl-dot{width:12px;height:12px;border-radius:3px;flex-shrink:0}

/* Vue selector */
.view-tabs{display:flex;gap:4px;align-items:center}
.vtab{padding:5px 13px;border:1px solid var(--b2);border-radius:20px;cursor:pointer;font-size:12px;font-family:inherit;background:transparent;color:var(--t2);transition:all .18s;font-weight:500}
.vtab:hover{background:var(--surf2);color:var(--t1)}
.vtab.on{background:#2a1f3d;color:#c084fc;border-color:#7c3aed}
.vtab-sep{width:1px;height:16px;background:var(--b2);margin:0 4px}

/* Picto grid */
.pgrid{display:grid;grid-template-columns:repeat(4,1fr);gap:3px}
.pbtn{display:flex;flex-direction:column;align-items:center;gap:2px;padding:4px 2px;border:1px solid transparent;border-radius:var(--rs);cursor:pointer;background:transparent;transition:all .12s;font-size:9px;color:var(--t3);font-family:inherit}
.pbtn:hover{background:var(--surf2);color:var(--t2)}
.pbtn.on{border-color:var(--acc);background:rgba(77,124,254,.1);color:var(--acc)}
.pbtn img{width:22px;height:22px;display:block}

.act-btn{display:flex;align-items:center;justify-content:center;gap:6px;width:100%;padding:8px;border:1px solid var(--b2);border-radius:var(--rs);cursor:pointer;background:transparent;font-size:12px;font-family:inherit;color:var(--t2);transition:all .12s;font-weight:500}
.act-btn:hover{background:var(--surf2);color:var(--t1)}
.act-btn.primary{background:var(--acc);border-color:var(--acc);color:#fff}

.tip{position:absolute;background:var(--surf);border:1px solid var(--b2);border-radius:8px;padding:5px 11px;font-size:11px;color:var(--t1);pointer-events:none;display:none;z-index:10;white-space:nowrap;box-shadow:0 4px 16px rgba(0,0,0,.5)}

/* Rubrique Espace Cartes de Risque */
.risk-section{background:var(--surf);border:1px solid var(--b1);border-radius:var(--r);padding:16px;margin-top:10px;display:flex;flex-direction:column;gap:12px}
.gallery-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(130px, 1fr));gap:10px;margin-top:8px}
.gallery-item{background:var(--surf2);border:1px solid var(--b1);border-radius:var(--rs);padding:6px;text-align:center;position:relative}
.gallery-item img{width:100%;aspect-ratio:1;object-fit:cover;border-radius:4px;background:#131823;margin-bottom:4px}
.gallery-item span{font-size:11px;color:var(--t2);display:block;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.upload-box {border:2px dashed var(--b2);border-radius:var(--r);padding:20px;text-align:center;cursor:pointer;color:var(--t2);font-size:12px}
.upload-box:hover {border-color:var(--acc);color:var(--t1)}
</style>
</head>
<body>
<div class="page">

  <div class="app-hdr">
    <div class="app-title"><span class="dot"></span>Système Vigilance & Risques Météo</div>
    <div style="display:flex;align-items:center;gap:12px">
      <div class="view-tabs">
        <button class="vtab on" id="vtab-vigil" onclick="setView('vigilance')">Vigilance</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-prob" onclick="setView('probable')">↗ Évolution probable</button>
        <span class="vtab-sep"></span>
        <button class="vtab" id="vtab-reg" onclick="setView('regions')">🗺️ Anciennes Régions</button>
      </div>
      
      <div class="day-tabs" id="day-tabs-wrap">
        <button class="dtab on" id="dtab-today" onclick="setDay('today')">Aujourd'hui</button>
        <button class="dtab" id="dtab-tomorrow" onclick="setDay('tomorrow')">Demain</button>
      </div>
    </div>
  </div>

  <div class="top">
    <div id="map-col">
      <div id="map" style="width:100%;height:100%"></div>
      <div class="tip" id="tip"></div>
    </div>

    <div class="sb" id="sidebar-controls">
      </div>
  </div>

  <div class="risk-section">
    <div style="display:flex;justify-content:between;align-items:center">
      <div>
        <h3 style="font-size:14px;font-weight:600">📁 Espace « Cartes de Risque »</h3>
        <p style="font-size:11px;color:var(--t2)">Uploadez vos propres cartes ou enregistrez vos configurations saisies.</p>
      </div>
    </div>
    
    <div class="upload-box" onclick="document.getElementById('map-uploader').click()">
      Insérer / Glisser une carte météo (Fichier Image)...
      <input type="file" id="map-uploader" accept="image/*" style="display:none" onchange="importCustomMap(event)">
    </div>

    <div>
      <p class="stitle">Galerie des cartes enregistrées</p>
      <div class="gallery-grid" id="risk-gallery"></div>
    </div>
  </div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js"></script>
<script>
const PICTOS = {
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

// Association des départements aux anciennes régions françaises
const DEPT_TO_OLD_REG = {
  "Bas-Rhin":"Alsace","Haut-Rhin":"Alsace",
  "Meuse":"Lorraine","Meurthe-et-Moselle":"Lorraine","Moselle":"Lorraine","Vosges":"Lorraine",
  "Finistère":"Bretagne","Côtes-d'Armor":"Bretagne","Ille-et-Vilaine":"Bretagne","Morbihan":"Bretagne",
  "Gironde":"Aquitaine","Dordogne":"Aquitaine","Landes":"Aquitaine","Lot-et-Garonne":"Aquitaine","Pyrénées-Atlantiques":"Aquitaine",
  "Nord":"Nord-Pas-de-Calais","Pas-de-Calais":"Nord-Pas-de-Calais",
  "Seine-Maritime":"Haute-Normandie","Eure":"Haute-Normandie",
  "Calvados":"Basse-Normandie","Manche":"Basse-Normandie","Orne":"Basse-Normandie",
  "Loire-Atlantique":"Pays de la Loire","Vendée":"Pays de la Loire","Maine-et-Loire":"Pays de la Loire","Mayenne":"Pays de la Loire","Sarthe":"Pays de la Loire",
  "Charente-Maritime":"Poitou-Charentes","Charente":"Poitou-Charentes","Deux-Sèvres":"Poitou-Charentes","Vienne":"Poitou-Charentes",
  "Hérault":"Languedoc-Roussillon","Gard":"Languedoc-Roussillon","Pyrénées-Orientales":"Languedoc-Roussillon","Aude":"Languedoc-Roussillon","Lozère":"Languedoc-Roussillon",
  "Bouches-du-Rhône":"PACA","Var":"PACA","Alpes-Maritimes":"PACA","Vaucluse":"PACA","Alpes-de-Haute-Provence":"PACA","Hautes-Alpes":"PACA"
};

const COASTAL_REGIONS = new Set(["Alsace","Lorraine","Bretagne","Aquitaine","Nord-Pas-de-Calais","Haute-Normandie","Basse-Normandie","Pays de la Loire","Poitou-Charentes","Languedoc-Roussillon","PACA"]);

let activeView = 'vigilance';
let activeDay = 'today';

// États des données
let activeLevel = 'vert';
let activePicto = null;
let mainData = { today: {}, tomorrow: {} };

// Mode probable
let probViewMode = 'passage'; // 'passage' ou 'sans_passage'
let probData = {};

// Mode anciennes régions
let regActiveLevel = 'vert';
let regActivePictos = []; // Pictos côtiers multiples sélectionnés
let regData = {}; // { RegionName: { level: 'vert', pictos: [] } }

let svg, gMap, projection, pathGenerator, featuresData;

function setView(view) {
  activeView = view;
  d3.selectAll('.vtab').classList?.remove('on');
  d3.selectAll('.vtab').each(function() {
    this.classList.toggle('on', this.id === `vtab-${view === 'regions' ? 'reg' : view === 'probable' ? 'prob' : 'vigil'}`);
  });
  d3.select('#day-tabs-wrap').style('display', view === 'vigilance' ? 'flex' : 'none');
  
  buildSidebar();
  redrawMap();
}

function setDay(day) {
  activeDay = day;
  d3.selectAll('.day-tabs .dtab').each(function() { this.classList.toggle('on', this.id === `dtab-${day}`); });
  redrawMap();
}

function buildSidebar() {
  const sb = d3.select('#sidebar-controls');
  sb.html('');

  if (activeView === 'vigilance') {
    let cardLvl = sb.append('div').attr('class', 'card');
    cardLvl.append('p').attr('class', 'stitle').text("Niveau d'alerte");
    Object.keys(LEVELS).forEach(k => {
      let b = cardLvl.append('button').attr('class', 'lvl-btn' + (activeLevel === k ? ' on' : ''));
      b.html(`<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${LEVELS[k].label}`);
      b.on('click', () => {
        activeLevel = k;
        cardLvl.selectAll('.lvl-btn').classed('on', false);
        b.classed('on', true);
      });
    });

    let cardPicto = sb.append('div').attr('class', 'card');
    cardPicto.append('p').attr('class', 'stitle').text("Phénomène");
    let grid = cardPicto.append('div').attr('class', 'pgrid');
    Object.keys(PICTOS).forEach(k => {
      let pbtn = grid.append('button').attr('class', 'pbtn' + (activePicto === k ? ' on' : '')).attr('data-k', k);
      pbtn.html(`<img src="${PICTOS[k]}"><span>${k.slice(0,5)}</span>`);
      pbtn.on('click', () => {
        activePicto = activePicto === k ? null : k;
        grid.selectAll('.pbtn').classed('on', false);
        if (activePicto) pbtn.classed('on', true);
      });
    });
  } 
  else if (activeView === 'probable') {
    let cardMode = sb.append('div').attr('class', 'card');
    cardMode.append('p').attr('class', 'stitle').text("Mode d'Évolution (Double carte)");
    
    let bPassage = cardMode.append('button').attr('class', 'lvl-btn' + (probViewMode === 'passage' ? ' on' : ''));
    bPassage.html(`<span class="lvl-dot" style="background:#FFB82B"></span>Passage Vigilance Supérieure`);
    bPassage.on('click', () => { probViewMode = 'passage'; cardMode.selectAll('.lvl-btn').classed('on', false); bPassage.classed('on', true); redrawMap(); });

    let bSans = cardMode.append('button').attr('class', 'lvl-btn' + (probViewMode === 'sans_passage' ? ' on' : ''));
    bSans.html(`<span class="lvl-dot" style="background:#31AA35"></span>Pas de passage (Reste stable)`);
    bSans.on('click', () => { probViewMode = 'sans_passage'; cardMode.selectAll('.lvl-btn').classed('on', false); bSans.classed('on', true); redrawMap(); });
  } 
  else if (activeView === 'regions') {
    let cardLvl = sb.append('div').attr('class', 'card');
    cardLvl.append('p').attr('class', 'stitle').text("Intensité du Risque");
    Object.keys(LEVELS).forEach(k => {
      let b = cardLvl.append('button').attr('class', 'lvl-btn' + (regActiveLevel === k ? ' on' : ''));
      b.html(`<span class="lvl-dot" style="background:${LEVELS[k].color}"></span>${LEVELS[k].label}`);
      b.on('click', () => {
        regActiveLevel = k;
        cardLvl.selectAll('.lvl-btn').classed('on', false);
        b.classed('on', true);
      });
    });

    let cardPicto = sb.append('div').attr('class', 'card');
    cardPicto.append('p').attr('class', 'stitle').text("Pictos Côtiers Cumulables");
    let grid = cardPicto.append('div').attr('class', 'pgrid');
    Object.keys(PICTOS).forEach(k => {
      let pbtn = grid.append('button').attr('class', 'pbtn' + (regActivePictos.includes(k) ? ' on' : ''));
      pbtn.html(`<img src="${PICTOS[k]}"><span>${k.slice(0,5)}</span>`);
      pbtn.on('click', () => {
        let idx = regActivePictos.indexOf(k);
        if (idx > -1) regActivePictos.splice(idx, 1);
        else regActivePictos.push(k);
        pbtn.classed('on', regActivePictos.includes(k));
      });
    });
  }

  sb.append('button').attr('class', 'act-btn primary').style('margin-top', '6px').text('📸 Capturer & Télécharger PNG').on('click', exportToPNG);
}

function redrawMap() {
  if (!gMap) return;

  gMap.selectAll('path')
    .attr('fill', d => {
      const nm = d.properties.name || d.properties.NAME || '';
      const reg = DEPT_TO_OLD_REG[nm] || 'Autre';

      if (activeView === 'vigilance') {
        let dd = mainData[activeDay][nm];
        return dd ? LEVELS[dd.level].color : '#1e2535';
      } 
      else if (activeView === 'probable') {
        let pb = probData[nm];
        if (pb === probViewMode) {
          return probViewMode === 'passage' ? '#FFB82B' : '#31AA35';
        }
        return '#1e2535';
      } 
      else if (activeView === 'regions') {
        let rd = regData[reg];
        if (!rd) return '#1e2535';
        // Plus le risque est élevé, plus la couleur est sombre/foncée
        let base = d3.color(LEVELS[rd.level].color);
        return rd.level === 'vert' ? base : base.darker(1.5);
      }
      return '#1e2535';
    });

  // Mise à jour des pictos sur la carte
  gMap.selectAll('.map-picto').remove();

  featuresData.forEach(d => {
    const nm = d.properties.name || d.properties.NAME || '';
    const reg = DEPT_TO_OLD_REG[nm] || 'Autre';
    const cent = pathGenerator.centroid(d);
    if (!cent || isNaN(cent[0])) return;

    if (activeView === 'vigilance') {
      let dd = mainData[activeDay][nm];
      if (dd && dd.picto) {
        gMap.append('image').attr('class', 'map-picto')
          .attr('href', PICTOS[dd.picto])
          .attr('x', cent[0] - 10).attr('y', cent[1] - 10)
          .attr('width', 20).attr('height', 20);
      }
    } 
    else if (activeView === 'regions' && COASTAL_REGIONS.has(reg)) {
      let rd = regData[reg];
      if (rd && rd.pictos && rd.pictos.length > 0) {
        // Pour éviter de dupliquer l'icône sur tous les départements d'une même ancienne région côtière,
        // on ne l'affiche que sur le département côtier référencé dans notre dictionnaire.
        if (DEPT_TO_OLD_REG[nm] === reg && ["Bas-Rhin","Finistère","Gironde","Nord","Seine-Maritime","Calvados","Loire-Atlantique","Charente-Maritime","Hérault","Bouches-du-Rhône"].includes(nm)) {
          rd.pictos.forEach((p, i) => {
            gMap.append('image').attr('class', 'map-picto')
              .attr('href', PICTOS[p])
              .attr('x', cent[0] - 10 + (i * 15) - ((rd.pictos.length * 15)/4))
              .attr('y', cent[1] - 10)
              .attr('width', 20).attr('height', 20);
          });
        }
      }
    }
  });
}

function exportToPNG() {
  const svgEl = document.querySelector('#map svg');
  const svgString = new XMLSerializer().serializeToString(svgEl);
  const svgBlob = new Blob([svgString], {type: "image/svg+xml;charset=utf-8"});
  const blobURL = URL.createObjectURL(svgBlob);
  
  const image = new Image();
  image.onload = () => {
    const canvas = document.createElement('canvas');
    canvas.width = 600; canvas.height = 624;
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = '#131823';
    ctx.fillRect(0, 0, 600, 624);
    ctx.drawImage(image, 0, 0, 600, 624);
    
    const png = canvas.toDataURL('image/png');
    
    // Téléchargement automatique
    const a = document.createElement('a');
    a.download = `carte-${activeView}-${Date.now()}.png`;
    a.href = png;
    a.click();

    // Ajout automatique à la galerie des miniatures de cartes générées
    addMiniature(png, `Saisie ${activeView}`);
  };
  image.src = blobURL;
}

function importCustomMap(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(evt) {
    addMiniature(evt.target.result, "Import utilisateur");
  };
  reader.readAsDataURL(file);
}

function addMiniature(src, label) {
  const grid = d3.select('#risk-gallery');
  let item = grid.append('div').attr('class', 'gallery-item');
  item.html(`
    <img src="${src}">
    <span>${label}</span>
    <button style="background:transparent;border:none;color:var(--t3);font-size:10px;cursor:pointer;margin-top:2px" onclick="this.parentElement.remove()">Supprimer</button>
  `);
}

// Initialisation de la carte d'origine (Stable, Fixe, et parfaitement cadrée)
(async () => {
  const topo = await d3.json('https://cdn.jsdelivr.net/npm/datamaps@0.5.10/src/js/data/fra.topo.json');
  const key = Object.keys(topo.objects)[0];
  featuresData = topojson.feature(topo, topo.objects[key]).features;

  const width = 600, height = 624;
  projection = d3.geoConicConformal().parallels([44,49]).rotate([-3,0]).center([0,46.5]).scale(1860).translate([width/2, height/2]);
  pathGenerator = d3.geoPath().projection(projection);

  svg = d3.select('#map').append('svg').attr('viewBox', `0 0 ${width} ${height}`).style('width','100%').style('height','100%');
  gMap = svg.append('g');

  gMap.selectAll('path')
    .data(featuresData)
    .join('path')
    .attr('d', pathGenerator)
    .attr('fill', '#1e2535')
    .attr('stroke', 'rgba(255,255,255,.14)')
    .style('cursor', 'pointer')
    .on('mouseenter', function(event, d) {
      const nm = d.properties.name || d.properties.NAME || '';
      const reg = DEPT_TO_OLD_REG[nm] || 'Autre';
      d3.select('#tip').style('display', 'block')
        .style('left', (event.offsetX + 10) + 'px')
        .style('top', (event.offsetY + 10) + 'px')
        .html(activeView === 'regions' ? `Ancienne Région: <b>${reg}</b><br>Dép: ${nm}` : `Département: <b>${nm}</b>`);
    })
    .on('mouseleave', () => d3.select('#tip').style('display', 'none'))
    .on('click', function(event, d) {
      const nm = d.properties.name || d.properties.NAME || '';
      const reg = DEPT_TO_OLD_REG[nm] || 'Autre';

      if (activeView === 'vigilance') {
        let dd = mainData[activeDay][nm];
        if (dd && dd.level === activeLevel && dd.picto === activePicto) delete mainData[activeDay][nm];
        else mainData[activeDay][nm] = { level: activeLevel, picto: activePicto };
      } 
      else if (activeView === 'probable') {
        if (probData[nm] === probViewMode) delete probData[nm];
        else probData[nm] = probViewMode;
      } 
      else if (activeView === 'regions') {
        if (!regData[reg]) regData[reg] = { level: 'vert', pictos: [] };
        regData[reg].level = regActiveLevel;
        regData[reg].pictos = [...regActivePictos];
      }
      redrawMap();
    });

  buildSidebar();
  redrawMap();
})();
</script>
</body>
</html>
