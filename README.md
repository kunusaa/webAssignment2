# webAssignment2

1. Description of the project

The project "Weather App | ALua" is a web application that allows users to receive weather information in selected cities. The application visualizes data on temperature, weather description, humidity, wind speed, perceived temperature, pressure and precipitation volume for the last 3 hours. In addition, the application provides interesting facts about selected cities from Wikipedia and the location on the map.

2. Technologies used

HTML, CSS, JavaScript
Box icons: Used to display icons.
Mapbox API: Used to display a map with a marker of the selected city.
OpenWeatherMap API: To get weather data based on the coordinates of the city.
Wikipedia API: To get information about cities from Wikipedia.

3. Setting up a project

Getting API Keys:
All API keys were obtained after registration on the user site of the technologies used (Mapbox, OpenWeatherMap API, Wikipedia API)
Starting the project:
Write npm start in the project terminal, after that the project will load on http://localhost:3000

4. Project structure

index.html : The HTML file contains the basic markup of the web page.
style.css: The CSS file defines the styles and appearance of the elements on the page.
weather.js: The JavaScript file contains the logic of the application, event handling, API requests and interface updates.

5. Design solutions

Weather Visualization: Icons and images are used to visually display current weather conditions.
Animation: Added animation for smooth appearance of application elements.
Icons - Box icons.
Images - Freepik

6. Using the API

Mapbox API:
It is used to display an interactive map.
Connecting to index.html : <script src="https://apis.mapbox.com/mapbox-gl-js/v2.6.1/mapbox-gl .js"></script>
Map styles: <link href="https://api.mapbox.com/mapbox-gl-js/v2.6.1/mapbox-gl.css " rel="stylesheet" />

OpenWeatherMap API:
It is used to get weather data.
Request to the API in the fetchWeatherData function in weather.js .

Wikipedia API:
It is used to get interesting facts about cities.
API request in fetchWikipediaData function in weather.js .

7. The logic of the work

Entering a city: The user enters the name of the city in the input field.
Search: When you click on the search button icon, a request is made to the Mapbox API to get the coordinates of the city.
Data display: After successfully obtaining coordinates, a request is made to the OpenWeatherMap API to obtain weather data.
Interface update: The received data is displayed on the map and in the weather information block.
Wikipedia: A request is being made to the Wikipedia API to get interesting facts about the city.

8. Error handling

In case of an error when requesting geocoding or receiving weather data, an error message is displayed and the interface is reset to its initial state.

9. Important note

The logic of the application is located in the weather file.js, which provides ease of support, expansion and testing. Any design changes can be made to the style.css file, and the overall structure of the page is in index.html .

## Getting city coordinates from the Mapbox API
A code snippet in `index.html `:
```html

<script src="weather.js"></script>
<script>
    mapboxgl.accessToken = 'pk.eyJ1Ijoia3VudXNhYSIsImEiOiJjbHJpZ3p4ZDkwOTVxMnFxcTk0MHJ1a2RyIn0.7ryQCkxGr0h5kz6udO7a0g';
    const map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v11',
        center: [0, 0],
        zoom: 0
    });

    const cityInput = document.getElementById('cityInput');

    cityInput.addEventListener('keyup', async (event) => {
    if (event.key === 'Enter') {
        const cityName = cityInput.value.trim();

        if (cityName !== '') {
            try {
                const wikipediaData = await fetchWikipediaData(cityName);
                updateWikipediaUI(wikipediaData, maxWords);  
            } catch (error) {
                console.error('', error);
            }
        }
    }
});
</script>
```

## Getting weather data from the OpenWeatherMap API
A code snippet in weather.js:
```javascript
const APIKey = '20a2cc05a0e3e06f1fff99281c82a7b4';
async function fetchWeatherData(latitude, longitude, apiKey) {
    const weatherResponse = await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=${apiKey}`);
    
    if (!weatherResponse.ok) {
        console.error('');
        return null;
    }

    const weatherData = await weatherResponse.json();
    console.log('Weather Data:', weatherData); 
    return weatherData;
}
```
## Getting interesting facts about the city from the Wikipedia API
A code snippet in weather.js:
```javascript
sync function fetchWikipediaData(cityName) {
    const wikipediaEndpoint = `https://en.wikipedia.org/w/api.php?action=query&format=json&prop=extracts&exintro=true&redirects=true&titles=${cityName}&origin=*`;

    try {
        const wikipediaResponse = await fetch(wikipediaEndpoint);

        if (!wikipediaResponse.ok) {
            throw new Error('');
        }

        const wikipediaData = await wikipediaResponse.json();

        const pages = wikipediaData.query.pages;
        const firstPageId = Object.keys(pages)[0];
        const extract = pages[firstPageId].extract;

        const plainText = extract.replace(/<[^>]+>/g, '').trim();

        return plainText;
    } catch (error) {
        console.error('', error);
        throw error; 
}```
