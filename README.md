<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Météo Vision - Vigilance & Prévisions</title>
  <!-- Leaflet CSS & JS pour la carte -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <!-- Chart.js pour graphiques élégants -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <!-- Font Awesome 6 (icônes météo) -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
    }
    body {
      background: linear-gradient(145deg, #e0eafc 0%, #cfdef3 100%);
      padding: 20px;
      min-height: 100vh;
    }
    /* Conteneur principal */
    .container {
      max-width: 1400px;
      margin: 0 auto;
    }
    /* header */
    .header {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      flex-wrap: wrap;
      margin-bottom: 25px;
      background: rgba(255,255,255,0.4);
      backdrop-filter: blur(8px);
      padding: 15px 25px;
      border-radius: 45px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    }
    .title h1 {
      font-size: 1.8rem;
      color: #1e2f5e;
      letter-spacing: -0.5px;
    }
    .title p {
      color: #2c3e66;
      font-weight: 500;
    }
    .search-box {
      display: flex;
      gap: 12px;
      background: white;
      padding: 6px 15px;
      border-radius: 50px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .search-box input {
      border: none;
      padding: 10px 0;
      font-size: 1rem;
      width: 200px;
      outline: none;
      background: transparent;
    }
    .search-box button {
      background: #1e88e5;
      border: none;
      color: white;
      padding: 0 18px;
      border-radius: 40px;
      cursor: pointer;
      font-weight: bold;
      transition: 0.2s;
    }
    .search-box button:hover {
      background: #0b5e8a;
      transform: scale(0.97);
    }
    /* Grille principale */
    .grid {
      display: grid;
      grid-template-columns: 1fr 1.2fr;
      gap: 25px;
      margin-bottom: 30px;
    }
    /* Cartes génériques */
    .card {
      background: rgba(255,255,255,0.85);
      backdrop-filter: blur(12px);
      border-radius: 32px;
      padding: 20px;
      box-shadow: 0 15px 35px rgba(0,0,0,0.1);
      transition: all 0.2s;
    }
    /* Carte vigilance */
    .vigilance-header {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 18px;
      border-left: 6px solid #ffb74d;
      padding-left: 15px;
    }
    .vigilance-badge {
      font-size: 1.9rem;
    }
    .niveau {
      font-weight: 800;
      font-size: 1.2rem;
      background: #ffb74d30;
      padding: 4px 12px;
      border-radius: 60px;
    }
    .carte-container {
      height: 280px;
      border-radius: 24px;
      overflow: hidden;
      margin-bottom: 18px;
      border: 1px solid rgba(0,0,0,0.1);
    }
    .vigilance-desc {
      font-size: 0.9rem;
      display: flex;
      justify-content: space-between;
      flex-wrap: wrap;
      color: #1e2f5e;
      font-weight: 500;
    }
    .conseil {
      background: #eef2ff;
      padding: 12px;
      border-radius: 24px;
      margin-top: 12px;
      font-size: 0.85rem;
    }
    /* infos météo courantes */
    .meteo-actuelle {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      margin-bottom: 20px;
    }
    .temp-big {
      font-size: 3rem;
      font-weight: 800;
    }
    .details i {
      width: 28px;
      color: #1e88e5;
    }
    .prevision-jours {
      margin-top: 20px;
    }
    .jours-list {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin-top: 15px;
      flex-wrap: wrap;
    }
    .jour-item {
      background: rgba(255,255,255,0.6);
      backdrop-filter: blur(4px);
      flex: 1;
      text-align: center;
      padding: 12px 5px;
      border-radius: 28px;
      font-weight: 500;
    }
    .jour-item i {
      font-size: 1.8rem;
      margin: 8px 0;
      display: block;
    }
    /* Graphique températures */
    .graph-card {
      margin-top: 25px;
    }
    canvas {
      max-height: 220px;
      width: 100%;
    }
    footer {
      text-align: center;
      margin-top: 30px;
      color: #2c3e66;
      font-size: 0.8rem;
    }
    @media (max-width: 800px) {
      .grid {
        grid-template-columns: 1fr;
      }
      .header {
        flex-direction: column;
        gap: 12px;
      }
    }
    /* Chargement */
    .loader {
      text-align: center;
      padding: 20px;
      color: #1e2f5e;
    }
    button:disabled {
      opacity: 0.6;
    }
  </style>
</head>
<body>
<div class="container">
  <div class="header">
    <div class="title">
      <h1><i class="fas fa-cloud-sun-rain"></i> MétéoVision</h1>
      <p>Carte de vigilance • Prévisions détaillées</p>
    </div>
    <div class="search-box">
      <input type="text" id="cityInput" placeholder="Ex: Paris, Londres, Lyon" value="Paris">
      <button id="searchBtn"><i class="fas fa-search"></i> Chercher</button>
    </div>
  </div>

  <div class="grid">
    <!-- Colonne gauche : Carte de vigilance + alerte -->
    <div class="card">
      <div class="vigilance-header">
        <div class="vigilance-badge"><i class="fas fa-exclamation-triangle"></i></div>
        <div><h2>Vigilance météo</h2></div>
        <div class="niveau" id="vigilanceLevel">Chargement...</div>
      </div>
      <div class="carte-container" id="mapContainer"></div>
      <div class="vigilance-desc">
        <span><i class="fas fa-map-marker-alt"></i> <span id="vigilanceZone">France métropolitaine</span></span>
        <span><i class="far fa-calendar-alt"></i> <span id="vigilanceDate">—</span></span>
      </div>
      <div class="conseil" id="vigilanceConseil">
        ⚠️ Aucune vigilance particulière pour le moment.
      </div>
    </div>
    <!-- Colonne droite : météo détaillée et prévisions -->
    <div class="card">
      <div id="currentWeatherDisplay">
        <div class="meteo-actuelle">
          <div>
            <h2 id="cityName">Paris</h2>
            <div class="temp-big" id="tempNow">--°C</div>
            <div id="weatherDesc">--</div>
          </div>
          <div style="font-size: 3.5rem;" id="weatherIcon"><i class="fas fa-cloud-sun"></i></div>
        </div>
        <div class="details" id="extraDetails">
          <p><i class="fas fa-tint"></i> Humidité: --%</p>
          <p><i class="fas fa-wind"></i> Vent: -- km/h</p>
          <p><i class="fas fa-eye"></i> Visibilité: -- km</p>
        </div>
      </div>
      <div class="prevision-jours">
        <h3><i class="fas fa-chart-line"></i> Prévisions 5 jours</h3>
        <div class="jours-list" id="forecastList">
          <div class="loader">Chargement prévisions...</div>
        </div>
      </div>
    </div>
  </div>
  <!-- Graphique des températures -->
  <div class="card graph-card">
    <h3><i class="fas fa-temperature-high"></i> Évolution des températures (5 jours)</h3>
    <canvas id="tempChart" width="400" height="200" style="max-width:100%; height:auto;"></canvas>
  </div>
  <footer>
    <i class="fas fa-cloud-moon"></i> Données simulées avec vigilance interactive (API Open-Meteo + génération aléatoire cohérente) <br>
    Carte Leaflet avec fond météo & marqueur ville.
  </footer>
</div>

<script>
  // ---------- Configuration globale ----------
  let map = null;
  let currentLat = 48.8566;   // Paris par défaut
  let currentLon = 2.3522;
  let currentCityName = "Paris";

  // Références DOM
  const cityInput = document.getElementById('cityInput');
  const searchBtn = document.getElementById('searchBtn');
  const cityNameSpan = document.getElementById('cityName');
  const tempNowSpan = document.getElementById('tempNow');
  const weatherDescSpan = document.getElementById('weatherDesc');
  const weatherIconDiv = document.getElementById('weatherIcon');
  const extraDetails = document.getElementById('extraDetails');
  const forecastListDiv = document.getElementById('forecastList');
  const vigilanceLevelSpan = document.getElementById('vigilanceLevel');
  const vigilanceZoneSpan = document.getElementById('vigilanceZone');
  const vigilanceDateSpan = document.getElementById('vigilanceDate');
  const vigilanceConseilDiv = document.getElementById('vigilanceConseil');

  let chartInstance = null;

  // ----- Simulation de vigilance selon météo et région (couleur & conseil)
  // On détermine un niveau de vigilance (1=vert, 2=jaune, 3=orange, 4=rouge)
  function computeVigilanceLevel(weatherCode, temp, windSpeed) {
    // weatherCode: selon Open-Meteo WMO
    // https://open-meteo.com/en/docs
    let score = 0;
    // Orage, pluie forte, neige intense, brouillard dense
    if (weatherCode >= 95 && weatherCode <= 99) score += 3; // orage violent
    else if (weatherCode >= 85 && weatherCode <= 86) score += 2; // chutes de neige fortes
    else if (weatherCode >= 61 && weatherCode <= 65) score += 1; // pluie modérée ou forte
    else if (weatherCode >= 71 && weatherCode <= 75) score += 1; // neige
    else if (weatherCode === 45 || weatherCode === 48) score += 1; // brouillard
    
    if (windSpeed > 70) score += 3;
    else if (windSpeed > 50) score += 2;
    else if (windSpeed > 35) score += 1;
    
    if (temp > 35) score += 2;    // canicule
    else if (temp < -8) score += 2; // grand froid
    
    if (score >= 5) return { niveau: "🔴 ROUGE", color: "#d32f2f", conseil: "⚠️ Vigilance ROUGE : conditions extrêmes. Évitez tout déplacement non essentiel. Suivez les consignes officielles." };
    if (score >= 3) return { niveau: "🟠 ORANGE", color: "#f57c00", conseil: "🟠 Vigilance ORANGE : phénomènes dangereux. Soyez très attentif et limitez vos déplacements." };
    if (score >= 1) return { niveau: "🟡 JAUNE", color: "#f9a825", conseil: "🟡 Vigilance JAUNE : phénomène modéré. Restez informé et prudent." };
    return { niveau: "🟢 VERT", color: "#2e7d32", conseil: "✅ Aucune vigilance particulière. Conditions météo habituelles." };
  }

  // Récupération des données météo via Open-Meteo (gratuit, sans clé)
  async function fetchWeather(lat, lon) {
    const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true&hourly=temperature_2m,relativehumidity_2m,windspeed_10m,weathercode&daily=weathercode,temperature_2m_max,temperature_2m_min,windspeed_10m_max&timezone=Europe/Paris&forecast_days=5`;
    const response = await fetch(url);
    if (!response.ok) throw new Error("Erreur réseau météo");
    const data = await response.json();
    return data;
  }

  // Fonction pour mettre à jour toute l'interface (météo, prévisions, vigilance, graphique)
  async function updateWeatherAndMap(lat, lon, cityName) {
    try {
      // Afficher un état de chargement
      forecastListDiv.innerHTML = '<div class="loader"><i class="fas fa-spinner fa-pulse"></i> Chargement prévisions...</div>';
      
      const weatherData = await fetchWeather(lat, lon);
      const current = weatherData.current_weather;
      const daily = weatherData.daily;
      
      // Température actuelle, vent, code
      const temp = current.temperature;
      const wind = current.windspeed;
      const weatherCode = current.weathercode;
      
      // Description météo simple
      const description = getWeatherDescription(weatherCode);
      const iconClass = getWeatherIcon(weatherCode);
      
      // Mise à jour affichage courant
      cityNameSpan.innerText = cityName;
      tempNowSpan.innerText = Math.round(temp) + "°C";
      weatherDescSpan.innerText = description;
      weatherIconDiv.innerHTML = `<i class="fas ${iconClass}"></i>`;
      
      // Humidité et visibilité (Open-Meteo ne donne pas la visibilité directe, on simule avec une approximation)
      const hourlyHumidity = weatherData.hourly?.relativehumidity_2m?.[0] || 70;
      const visibiliteKm = (weatherCode === 45 || weatherCode === 48) ? (Math.random() * 2 + 1).toFixed(1) : (Math.random() * 8 + 5).toFixed(1);
      extraDetails.innerHTML = `
        <p><i class="fas fa-tint"></i> Humidité: ${hourlyHumidity}%</p>
        <p><i class="fas fa-wind"></i> Vent: ${Math.round(wind)} km/h</p>
        <p><i class="fas fa-eye"></i> Visibilité: ${visibiliteKm} km</p>
      `;
      
      // Calcul de la vigilance (basée sur code météo, vent, température)
      const vig = computeVigilanceLevel(weatherCode, temp, wind);
      vigilanceLevelSpan.innerHTML = vig.niveau;
      vigilanceLevelSpan.style.backgroundColor = vig.color + "30";
      vigilanceLevelSpan.style.color = vig.color;
      vigilanceConseilDiv.innerHTML = `<i class="fas fa-shield-alt"></i> ${vig.conseil}`;
      const today = new Date().toLocaleDateString('fr-FR');
      vigilanceDateSpan.innerText = today;
      vigilanceZoneSpan.innerText = cityName + " & environs";
      
      // Mise à jour des prévisions 5 jours
      if (daily && daily.time) {
        const forecastDays = daily.time.length;
        let forecastHTML = '';
        const labels = [];
        const tempsMax = [];
        const tempsMin = [];
        for (let i = 0; i < Math.min(5, forecastDays); i++) {
          const date = new Date(daily.time[i]);
          const dayName = date.toLocaleDateString('fr-FR', { weekday: 'short' });
          const maxTemp = Math.round(daily.temperature_2m_max[i]);
          const minTemp = Math.round(daily.temperature_2m_min[i]);
          const codeJour = daily.weathercode[i];
          const iconJour = getWeatherIcon(codeJour);
          labels.push(dayName);
          tempsMax.push(maxTemp);
          tempsMin.push(minTemp);
          
          forecastHTML += `
            <div class="jour-item">
              <strong>${dayName}</strong>
              <i class="fas ${iconJour}"></i>
              <div>${maxTemp}° / ${minTemp}°</div>
              <small>${getWeatherDescription(codeJour).substring(0,12)}</small>
            </div>
          `;
        }
        forecastListDiv.innerHTML = forecastHTML;
        
        // Mise à jour du graphique Chart.js
        if (chartInstance) chartInstance.destroy();
        const ctx = document.getElementById('tempChart').getContext('2d');
        chartInstance = new Chart(ctx, {
          type: 'line',
          data: {
            labels: labels,
            datasets: [
              {
                label: 'Température max (°C)',
                data: tempsMax,
                borderColor: '#e63946',
                backgroundColor: 'rgba(230,57,70,0.1)',
                tension: 0.3,
                fill: false,
                pointBackgroundColor: '#e63946',
                pointRadius: 5
              },
              {
                label: 'Température min (°C)',
                data: tempsMin,
                borderColor: '#1e88e5',
                backgroundColor: 'rgba(30,136,229,0.1)',
                tension: 0.3,
                fill: false,
                pointBackgroundColor: '#1e88e5',
                pointRadius: 5
              }
            ]
          },
          options: {
            responsive: true,
            maintainAspectRatio: true,
            plugins: {
              legend: { position: 'top' },
              tooltip: { mode: 'index', intersect: false }
            },
            scales: {
              y: { title: { display: true, text: 'Température (°C)' } }
            }
          }
        });
      } else {
        forecastListDiv.innerHTML = '<div>Prévisions non disponibles</div>';
      }
      
      // Mise à jour de la carte (centrage et marqueur)
      if (map) {
        map.setView([lat, lon], 10);
        // Supprimer les anciens marqueurs sauf le fond par défaut
        map.eachLayer((layer) => {
          if (layer instanceof L.Marker && layer.options && layer.options.title !== 'base') {
            map.removeLayer(layer);
          }
        });
        L.marker([lat, lon], { title: cityName }).addTo(map)
          .bindPopup(`<b>${cityName}</b><br>${temp}°C<br>${description}`)
          .openPopup();
      }
      
    } catch (error) {
      console.error(error);
      forecastListDiv.innerHTML = `<div class="loader">❌ Erreur météo: ${error.message}</div>`;
      vigilanceConseilDiv.innerHTML = "⚠️ Impossible de récupérer la vigilance réseau.";
    }
  }
  
  // Dictionnaire des codes WMO (simplifié)
  function getWeatherDescription(code) {
    const codes = {
      0: "Ciel dégagé", 1: "Principalement dégagé", 2: "Partiellement nuageux", 3: "Nuageux",
      45: "Brouillard", 48: "Brouillard givrant",
      51: "Bruine légère", 53: "Bruine modérée", 55: "Bruine dense",
      61: "Pluie légère", 63: "Pluie modérée", 65: "Pluie forte",
      71: "Neige légère", 73: "Neige modérée", 75: "Neige forte",
      80: "Averses légères", 81: "Averses modérées", 82: "Averses violentes",
      95: "Orage", 96: "Orage avec grêle légère", 99: "Orage violent"
    };
    return codes[code] || "Variable";
  }
  
  function getWeatherIcon(code) {
    if (code === 0) return "fa-sun";
    if (code === 1 || code === 2) return "fa-cloud-sun";
    if (code === 3) return "fa-cloud";
    if (code === 45 || code === 48) return "fa-smog";
    if (code >= 51 && code <= 55) return "fa-cloud-rain";
    if (code >= 61 && code <= 65) return "fa-cloud-showers-heavy";
    if (code >= 71 && code <= 75) return "fa-snowflake";
    if (code >= 80 && code <= 82) return "fa-cloud-rain";
    if (code >= 95 && code <= 99) return "fa-bolt";
    return "fa-cloud-sun";
  }
  
  // Récupération des coordonnées par nom de ville via Nominatim (OpenStreetMap)
  async function geocodeCity(cityName) {
    const url = `https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(cityName)}&format=json&limit=1&addressdetails=0&accept-language=fr`;
    const response = await fetch(url, {
      headers: { 'User-Agent': 'MeteoVigilanceApp/1.0' }
    });
    const data = await response.json();
    if (data && data.length > 0) {
      return { lat: parseFloat(data[0].lat), lon: parseFloat(data[0].lon), displayName: data[0].display_name.split(',')[0] };
    }
    throw new Error("Ville non trouvée");
  }
  
  // Fonction principale sur recherche
  async function searchAndUpdate() {
    const query = cityInput.value.trim();
    if (query === "") return;
    searchBtn.disabled = true;
    searchBtn.innerHTML = "<i class='fas fa-spinner fa-pulse'></i>";
    try {
      const geo = await geocodeCity(query);
      currentLat = geo.lat;
      currentLon = geo.lon;
      currentCityName = geo.displayName;
      await updateWeatherAndMap(currentLat, currentLon, currentCityName);
    } catch (err) {
      alert("Erreur: " + err.message);
      console.error(err);
    } finally {
      searchBtn.disabled = false;
      searchBtn.innerHTML = "<i class='fas fa-search'></i> Chercher";
    }
  }
  
  // Initialisation de la carte Leaflet (fond météo)
  function initMap(lat, lon) {
    // Tuiles OpenStreetMap classique + couche météo "precipitation" en surbrillance ? 
    // On met un fond élégant style "CartoDB Voyager"
    map = L.map('mapContainer').setView([lat, lon], 9);
    L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OSM</a> & CartoDB, Météo vigilance intégrée',
      subdomains: 'abcd',
      maxZoom: 19,
      minZoom: 5
    }).addTo(map);
    
    // marqueur initial
    L.marker([lat, lon]).addTo(map)
      .bindPopup(`<b>${currentCityName}</b>`)
      .openPopup();
  }
  
  // Chargement par défaut au démarrage (Paris) puis mise à jour météo
  async function defaultLoad() {
    initMap(48.8566, 2.3522);
    await updateWeatherAndMap(48.8566, 2.3522, "Paris");
  }
  
  // Evénements
  searchBtn.addEventListener('click', searchAndUpdate);
  cityInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') searchAndUpdate();
  });
  
  // Lancer l'appli
  defaultLoad();
</script>
</body>
</html>
