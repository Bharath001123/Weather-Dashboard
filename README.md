<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather Dashboard</title>
  <link rel="stylesheet" href="styles.css">
  <script defer src="script.js"></script>
</head>
<body>
  <div class="container">
    <header>
      <h1>Weather Dashboard</h1>
      <input type="text" id="cityInput" placeholder="Enter city name">
      <button id="searchButton">Search</button>
      <button id="currentLocationButton">Current Location</button>
    </header>

    <main>
      <section id="currentWeather">
        <h2>Current Weather</h2>
        <div id="weatherDetails">
          <!-- Dynamic content will go here -->
        </div>
        <button id="addToFavoritesButton" style="display:none;">Add to Favorites</button> <!-- Hidden initially -->
      </section>

      <section id="forecast">
        <h2>5-Day Forecast</h2>
        <div id="forecastContainer">
          <!-- Dynamic content will go here -->
        </div>
      </section>
    </main>

    <footer>
      <button id="toggleTempUnit">Toggle °C/°F</button>
      <div id="favoritesContainer">
        <h3>Favorites</h3>
        <ul id="favoritesList"></ul>
      </div>
    </footer>
  </div>
</body>
</html>




/* Existing CSS */
body {
    font-family: 'Roboto', sans-serif;
    margin: 10px;
    padding: 50px;
    background: url('https://wallpaperaccess.com/full/11748762.jpg') no-repeat center center/cover;
    color: #sky-blue-with-smooth-flowing-texture_892776-255;
}


.cloud-layer-1, .cloud-layer-2 {
    position: absolute;
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    background-position: center;
    background-size: cover;
}

.cloud-layer-1 {
    background-image: url(https://img.freepik.com/premium-photo/sky-blue-with-smooth-flowing-texture_892776-255.jpg);
    animation: cloudMove 40s linear infinite;
    opacity: 0.8;
}

.cloud-layer-2 {
    background-image: url("https://img.freepik.com/premium-photo/sky-blue-with-smooth-flowing-texture_892776-255.jpg");
    animation: cloudMove 60s linear infinite;
    opacity: 0.5;
}

@keyframes cloudMove {
    0% {
        transform: translateX(-100%);
    }
    100% {
        transform: translateX(100%);
    }
}

.container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    background-color: rgba(255, 255, 255, 0.4); /* Light background */
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.9);
    position: relative;
    z-index: 10;
    animation: fadeIn 1s ease-in-out;
}

header {
    text-align: center;
    margin-bottom: 20px;
}

header input {
    padding: 8px;
    margin-right: 10px;
    border: none;
    border-radius: 4px;
    background-color: rgba(255, 255, 255, 0.2);
    color: #fff;
}

header button {
    padding: 8px 12px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
}

header button:hover {
    background-color: #0056b3;
}

main section {
    margin-bottom: 20px;
}

#weatherDetails, #forecastContainer {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    animation: slideIn 1s ease-in-out;
}

.forecast-card {
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 10px;
    width: 150px;
    text-align: center;
    background-color: rgba(255, 255, 255, 0.2);
    animation: fadeInUp 1s ease-in-out;
}

footer {
    margin-top: 20px;
}

#favoritesContainer ul {
    list-style: none;
    padding: 0;
}

#favoritesContainer li {
    padding: 5px;
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 4px;
    margin-bottom: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
}

#favoritesContainer li:hover {
    background-color: rgba(255, 255, 255, 0.3);
}

/* Weather Backgrounds */
.sunny {
    background: url('https://wallpaperaccess.com/full/3265126.jpg') no-repeat center center/cover;
}

.cloudy {
    background: url('https://img.freepik.com/premium-photo/sky-blue-with-smooth-flowing-texture_892776-255.jpg') no-repeat center center/cover;
}

.rainy {
    background: url('https://th.bing.com/th/id/OIP.FyvexifVyaUxHIdW-eljuQHaFj?rs=1&pid=ImgDetMain') no-repeat center center/cover;
}

.snowy {
    background: url('https://th.bing.com/th/id/OIP.4T-8vQVMsXZiX1I1c50EtAHaE4?rs=1&pid=ImgDetMain') no-repeat center center/cover;
}

/* Animations */
@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@keyframes slideIn {
    from {
        transform: translateY(20px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}

@keyframes fadeInUp {
    from {
        transform: translateY(20px);
        opacity: 0;
    }
    to {
        transform: translateY(0);
        opacity: 1;
    }
}


// your code goes here
// your code goes here
// script.js
const API_KEY = `bd5e378503939ddaee76f12ad7a97608`;
let isCelsius = true;

document.getElementById('searchButton').addEventListener('click', () => {
  const city = document.getElementById('cityInput').value;
  fetchWeather(city);
});

document.getElementById('currentLocationButton').addEventListener('click', () => {
  navigator.geolocation.getCurrentPosition((position) => {
    const { latitude, longitude } = position.coords;
    fetchWeather(null, latitude, longitude);
  });
});

document.getElementById('toggleTempUnit').addEventListener('click', () => {
  isCelsius = !isCelsius;
  const city = document.getElementById('cityInput').value;
  if (city) fetchWeather(city);
});

function fetchWeather(city, lat = null, lon = null) {
  const url = city
    ? `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}`
    : `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}`;

  fetch(url)
    .then((response) => response.json())
    .then((data) => {
      displayCurrentWeather(data);
      fetchForecast(data.coord.lat, data.coord.lon);
    })
    .catch(() => alert('Error fetching weather data.'));
}

function displayCurrentWeather(data) {
  const temp = isCelsius
    ? (data.main.temp - 273.15).toFixed(1)
    : ((data.main.temp - 273.15) * 9/5 + 32).toFixed(1);

  const weatherHTML = `
    <p>City: ${data.name}</p>
    <p>Temperature: ${temp}°${isCelsius ? 'C' : 'F'}</p>
    <p>Condition: ${data.weather[0].description}</p>
    <p>Humidity: ${data.main.humidity}%</p>
    <p>Wind Speed: ${data.wind.speed} m/s</p>
  `;
  document.getElementById('weatherDetails').innerHTML = weatherHTML;
}

function fetchForecast(lat, lon) {
  const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${API_KEY}`;

  fetch(url)
    .then((response) => response.json())
    .then((data) => displayForecast(data.list))
    .catch(() => alert('Error fetching forecast data.'));
}

function displayForecast(forecast) {
  const forecastHTML = forecast.slice(0, 5).map((item) => {
    const temp = isCelsius
      ? (item.main.temp - 273.15).toFixed(1)
      : ((item.main.temp - 273.15) * 9/5 + 32).toFixed(1);
    
    return `
      <div class="forecast-card">
        <p>${new Date(item.dt_txt).toLocaleDateString()}</p>
        <p>Temp: ${temp}°${isCelsius ? 'C' : 'F'}</p>
        <p>${item.weather[0].description}</p>
      </div>
    `;
  }).join('');
  
  document.getElementById('forecastContainer').innerHTML = forecastHTML;
}
