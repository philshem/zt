# zt

A web-based map to locate truffle-compatible host trees in Zürich using the city's tree cadastre (Baumkataster) open data.

## What is this?

`index.html` is an interactive Leaflet map that helps you find trees suitable for truffle foraging in Zürich city. The map fetches tree data from the WFS (Web Feature Service) endpoint and highlights truffle-compatible species based on age and type.

## Quick start

1. Start a local web server:

```sh
python3 -m http.server 8000
```

2. Open in your browser:

```
http://localhost:8000/index.html
```

3. Allow location access when prompted to see your position on the map.

## Features

- Interactive map showing trees in Zürich
- Color-coded markers for different tree species
- Click markers for detailed tree information
- Live location tracking (requires permission)
- Info button with additional details

## Technical details

- Uses Zürich's WFS endpoint to fetch live tree data for the current map view
- Automatically loads trees as you pan/zoom
- Works on desktop and mobile browsers
- Requires HTTPS for geolocation (works on localhost for testing)

## Data source

Tree data from [Stadt Zürich Open Data](https://data.stadt-zuerich.ch/dataset/baumkataster)

## License

This is a tool for personal use. Follow the original dataset's license and data-use policies.
