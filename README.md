<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Météo Vigilance France</title>
    <!-- Leaflet pour la carte -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <!-- Chart.js pour le graphique -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1300px;
            margin: 0 auto;
        }

        /* Header */
        .header {
            background: rgba(255,255,255,0.2);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }

        .title h1 {
            color: white;
            font-size: 1.8rem;
        }

        .title p {
            color: #f0f0ff;
        }

        .search-box {
            display: flex;
            gap: 10px;
            background: white;
            padding: 5px 15px;
            border-radius: 50px;
        }

        .search-box input {
            padding: 10px;
            border: none;
            outline: none;
            font-size: 16px;
            width: 200px;
            border-radius: 50px;
        }

        .search-box button {
            background: #ff6b6b;
            color: white;
            border: none;
            padding: 0 20px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.3s;
        }

        .search-box button:hover {
            background: #ff4757;
            transform: scale(1.05);
        }

        /* Grille */
        .grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }

        /* Cartes */
        .card {
            background: rgba(255,255,255,0.95);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }

        /* Carte vigilance */
        .vigilance-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #eee;
        }

        .vigilance-level {
            font-size: 1.3rem;
            font-weight: bold;
            padding: 5px 15px;
            border-radius: 30px;
            background: #f0f0f0;
        }

        #map {
            height: 300px;
            border-radius: 15px;
            margin-bottom: 15px;
            border: 2px solid white;
        }

        .vigilance-message {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 12px;
            border-left: 4px solid #ff6b6b;
            margin-top: 10px;
        }

        /* Météo actuelle */
        .current-weather {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 15px;
            color: white;
        }

        .temp {
            font-size: 3rem;
            font-weight: bold;
        }

        .weather-icon {
            font-size: 3rem;
        }

        .weather-details {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 15px;
        }

        .detail-item {
            background: #f8f9fa;
            padding: 10px;
            border-radius: 10px;
            text-align: center;
        }

        /* Prévisions */
        .forecast {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-top: 15px;
        }

        .forecast-day {
            background: #f8f9fa;
            padding: 10px;
            border-radius: 10px;
            text-align: center;
        }

        .forecast-day i {
            font-size: 1.5rem;
            margin: 8px 0;
            display: block;
        }

        /* Graphique */
        .chart-container {
            margin-top: 20px;
        }

        canvas {
            max-height: 250px;
            width: 100%;
        }

        footer {
            text-align: center;
            margin-top: 20px;
            color: white;
            padding: 15px;
        }

        @media (max-width: 768px) {
            .grid {
                grid-template-columns: 1fr;
            }
            .forecast {
                grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
            }
        }
    </style>
</head>
<body>
<div class="container">
    <div class="header">
        <div class="title">
            <h1><i class="fas fa-cloud-sun-rain"></i> Météo Vigilance France</h1>
            <p>Carte interactive • Alertes météo • Prévisions 5 jours</p>
        </div>
        <div class="search-box">
            <input type="text" id="citySearch" placeholder="Ex: Paris, Lyon, Marseille" value="Paris">
            <button onclick="searchCity()"><i class="fas fa-search"></i> Chercher</button>
        </div>
    </div>

    <div class="grid">
        <!-- Carte Vigilance -->
        <div class="card">
            <div class="vigilance-header">
                <i class="fas fa-exclamation-triangle" style="font-size: 2rem; color: #ff6b6b;"></i>
                <h2>Vigilance Météo</h2>
                <div class="vigilance-level" id="vigilanceLevel">🟢 VERT</div>
            </div>
            <div id="map"></div>
            <div class="vigilance-message" id="vigilanceMessage">
                ✅ Conditions normales, aucune alerte particulière.
            </div>
        </div>

        <!-- Météo Courante -->
        <div class="card">
            <div class="current-weather">
                <div>
                    <h2 id="cityName">Paris</h2>
                    <div class="temp" id="temperature">--°C</div>
                    <div id="description">Chargement...</div>
                </div>
                <div class="weather-icon" id="weatherIcon">
                    <i class="fas fa-sun"></i>
                </div>
            </div>
            <div class="weather-details">
                <div class="detail-item">
                    <i class="fas fa-tint"></i>
                    <div id="humidity">--%</div>
                    <small>Humidité</small>
                </div>
                <div class="detail-item">
                    <i class="fas fa-wind"></i>
                    <div id="wind">-- km/h</div>
                    <small>Vent</small>
                </div>
                <div class="detail-item">
                    <i class="fas fa-tachometer-alt"></i>
                    <div id="pressure">-- hPa</div>
                    <small>Pression</small>
                </div>
                <div class="detail-item">
                    <i class="fas fa-cloud-rain"></i>
                    <div id="rain">--%</div>
                    <small>Risque pluie</small>
                </div>
            </div>
        </div>
    </div>

    <!-- Prévisions 5 jours -->
    <div class="card">
        <h3><i class="fas fa-calendar-week"></i> Prévisions 5 jours</h3>
        <div class="forecast" id="forecast">
            <div>Chargement...</div>
        </div>
        <div class="chart-container">
            <canvas id="tempChart"></canvas>
        </div>
    </div>

    <footer>
        <i class="fas fa-info-circle"></i> Données météo simulées en temps réel • Carte interactive avec vigilance
    </footer>
</div>

<script>
    // Base de données des villes françaises
    const villes = {
        'paris': { nom: 'Paris', lat: 48.8566, lon: 2.3522, climat: 'oceanique' },
        'lyon': { nom: 'Lyon', lat: 45.7640, lon: 4.8357, climat: 'continental' },
        'marseille': { nom: 'Marseille', lat: 43.2965, lon: 5.3698, climat: 'mediterraneen' },
        'bordeaux': { nom: 'Bordeaux', lat: 44.8378, lon: -0.5792, climat: 'oceanique' },
        'lille': { nom: 'Lille', lat: 50.6292, lon: 3.0573, climat: 'oceanique' },
        'strasbourg': { nom: 'Strasbourg', lat: 48.5734, lon: 7.7521, climat: 'continental' },
        'nice': { nom: 'Nice', lat: 43.7102, lon: 7.2620, climat: 'mediterraneen' },
        'toulouse': { nom: 'Toulouse', lat: 43.6045, lon: 1.4442, climat: 'oceanique' },
        'nantes': { nom: 'Nantes', lat: 47.2184, lon: -1.5536, climat: 'oceanique' },
        'grenoble': { nom: 'Grenoble', lat: 45.1885, lon: 5.7245, climat: 'montagne' }
    };

    let map;
    let chart;
    let currentCity = 'paris';

    // Génération de météo réaliste
    function generateWeather(cityKey, dayOffset = 0) {
        const ville = villes[cityKey];
        const today = new Date();
        const month = today.getMonth();
        
        // Température de base selon la ville
        let baseTemp = 12;
        if (ville.climat === 'mediterraneen') baseTemp = 16;
        if (ville.climat === 'continental') baseTemp = 11;
        if (ville.climat === 'montagne') baseTemp = 9;
        
        // Variation saisonnière
        const seasonVariation = Math.sin((month - 3) * Math.PI / 6) * 8;
        let temp = baseTemp + seasonVariation + (Math.random() * 6 - 3) + (dayOffset * 0.3);
        
        // Conditions météo
        const weathers = [
            { desc: 'Ensoleillé', icon: 'fa-sun', rain: 0, wind: 5 + Math.random() * 10 },
            { desc: 'Peu nuageux', icon: 'fa-cloud-sun', rain: 10, wind: 8 + Math.random() * 12 },
            { desc: 'Nuageux', icon: 'fa-cloud', rain: 20, wind: 12 + Math.random() * 15 },
            { desc: 'Pluie légère', icon: 'fa-cloud-rain', rain: 60, wind: 15 + Math.random() * 15 },
            { desc: 'Pluie forte', icon: 'fa-cloud-showers-heavy', rain: 85, wind: 25 + Math.random() * 20 },
            { desc: 'Orages', icon: 'fa-bolt', rain: 95, wind: 40 + Math.random() * 25 }
        ];
        
        let weatherIndex = Math.floor(Math.random() * (temp > 25 ? 3 : 5));
        if (temp < 3 && Math.random() > 0.7) {
            weatherIndex = 4;
            weathers[4].desc = 'Neige';
            weathers[4].icon = 'fa-snowflake';
        }
        
        const weather = weathers[weatherIndex];
        
        return {
            temp: Math.round(temp),
            tempMax: Math.round(temp + Math.random() * 4 + 2),
            tempMin: Math.round(temp - Math.random() * 4 - 2),
            desc: weather.desc,
            icon: weather.icon,
            humidity: 40 + Math.floor(Math.random() * 50),
            wind: Math.round(weather.wind),
            pressure: 1005 + Math.floor(Math.random() * 20),
            rain: weather.rain
        };
    }

    // Calcul de la vigilance
    function getVigilance(weather) {
        if (weather.desc === 'Orages' || weather.wind > 70) {
            return { level: '🔴 ROUGE', color: '#d32f2f', message: '⚠️ VIGILANCE ROUGE : Orages violents ou vents extrêmes ! Mettez-vous à l\'abri.' };
        } else if (weather.wind > 50 || weather.rain > 80 || weather.temp > 35) {
            return { level: '🟠 ORANGE', color: '#f57c00', message: '🟠 VIGILANCE ORANGE : Conditions dangereuses. Soyez très prudent.' };
        } else if (weather.wind > 35 || weather.rain > 50 || weather.temp > 32) {
            return { level: '🟡 JAUNE', color: '#f9a825', message: '🟡 VIGILANCE JAUNE : Conditions perturbées. Restez informé.' };
        } else {
            return { level: '🟢 VERT', color: '#2e7d32', message: '✅ Aucune vigilance particulière. Conditions normales.' };
        }
    }

    // Mise à jour de l'affichage
    function updateDisplay() {
        const ville = villes[currentCity];
        if (!ville) return;
        
        // Météo actuelle
        const weather = generateWeather(currentCity, 0);
        
        // Mise à jour des éléments
        document.getElementById('cityName').innerHTML = ville.nom;
        document.getElementById('temperature').innerHTML = weather.temp + '°C';
        document.getElementById('description').innerHTML = weather.desc;
        document.getElementById('weatherIcon').innerHTML = `<i class="fas ${weather.icon}"></i>`;
        document.getElementById('humidity').innerHTML = weather.humidity + '%';
        document.getElementById('wind').innerHTML = weather.wind + ' km/h';
        document.getElementById('pressure').innerHTML = weather.pressure + ' hPa';
        document.getElementById('rain').innerHTML = weather.rain + '%';
        
        // Vigilance
        const vigilance = getVigilance(weather);
        document.getElementById('vigilanceLevel').innerHTML = vigilance.level;
        document.getElementById('vigilanceLevel').style.backgroundColor = vigilance.color + '30';
        document.getElementById('vigilanceLevel').style.color = vigilance.color;
        document.getElementById('vigilanceMessage').innerHTML = `<i class="fas fa-bell"></i> ${vigilance.message}`;
        
        // Prévisions 5 jours
        const forecasts = [];
        const labels = [];
        const maxTemps = [];
        const minTemps = [];
        
        for (let i = 0; i < 5; i++) {
            const f = generateWeather(currentCity, i);
            forecasts.push(f);
            const date = new Date();
            date.setDate(date.getDate() + i);
            labels.push(date.toLocaleDateString('fr-FR', { weekday: 'short' }));
            maxTemps.push(f.tempMax);
            minTemps.push(f.tempMin);
        }
        
        // Affichage des prévisions
        const forecastDiv = document.getElementById('forecast');
        forecastDiv.innerHTML = forecasts.map(f => `
            <div class="forecast-day">
                <strong>Jour ${forecasts.indexOf(f) + 1}</strong>
                <i class="fas ${f.icon}"></i>
                <div>${f.tempMax}° / ${f.tempMin}°</div>
                <small>${f.desc}</small>
            </div>
        `).join('');
        
        // Mise à jour du graphique
        if (chart) chart.destroy();
        const ctx = document.getElementById('tempChart').getContext('2d');
        chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: labels,
                datasets: [
                    {
                        label: 'Température max (°C)',
                        data: maxTemps,
                        borderColor: '#ff6b6b',
                        backgroundColor: 'rgba(255,107,107,0.1)',
                        tension: 0.3,
                        fill: true
                    },
                    {
                        label: 'Température min (°C)',
                        data: minTemps,
                        borderColor: '#4ecdc4',
                        backgroundColor: 'rgba(78,205,196,0.1)',
                        tension: 0.3,
                        fill: true
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: true,
                plugins: {
                    legend: { position: 'top' }
                }
            }
        });
        
        // Mise à jour de la carte
        if (map) {
            map.setView([ville.lat, ville.lon], 8);
            // Supprimer les anciens marqueurs
            map.eachLayer(layer => {
                if (layer instanceof L.Marker) map.removeLayer(layer);
            });
            
            // Marqueur personnalisé
            const markerIcon = L.divIcon({
                html: `<div style="background-color: ${vigilance.color}; width: 30px; height: 30px; border-radius: 50%; border: 3px solid white; box-shadow: 0 2px 5px black; display: flex; align-items: center; justify-content: center;">
                          <i class="fas fa-exclamation-triangle" style="color: white; font-size: 14px;"></i>
                       </div>`,
                iconSize: [30, 30],
                popupAnchor: [0, -15]
            });
            
            L.marker([ville.lat, ville.lon], { icon: markerIcon })
                .addTo(map)
                .bindPopup(`<b>${ville.nom}</b><br>${weather.temp}°C - ${weather.desc}<br><b style="color:${vigilance.color}">${vigilance.level}</b>`)
                .openPopup();
                
            // Cercle de vigilance
            L.circle([ville.lat, ville.lon], {
                radius: 20000,
                color: vigilance.color,
                weight: 2,
                fillColor: vigilance.color,
                fillOpacity: 0.2
            }).addTo(map);
        }
    }

    // Recherche de ville
    function searchCity() {
        const searchTerm = document.getElementById('citySearch').value.toLowerCase().trim();
        if (villes[searchTerm]) {
            currentCity = searchTerm;
            updateDisplay();
        } else {
            // Recherche approximative
            let found = null;
            for (let key in villes) {
                if (key.includes(searchTerm) || searchTerm.includes(key)) {
                    found = key;
                    break;
                }
            }
            if (found) {
                currentCity = found;
                document.getElementById('citySearch').value = villes[found].nom;
                updateDisplay();
            } else {
                alert('Ville non trouvée. Essayez: Paris, Lyon, Marseille, Bordeaux, Lille, Strasbourg, Nice, Toulouse, Nantes, Grenoble');
            }
        }
    }

    // Initialisation de la carte
    function initMap() {
        map = L.map('map').setView([48.8566, 2.3522], 6);
        L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OSM</a>',
            subdomains: 'abcd'
        }).addTo(map);
    }

    // Lancer l'application
    initMap();
    updateDisplay();
</script>
</body>
</html>
