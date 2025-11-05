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

## for fun

### count trees per species

```bash
jq -r '
  [.features[].properties.baumnamelat
   | select(type == "string" and . != "")]
  | group_by(.)
  | map({species: .[0], count: length})
  | sort_by(-.count)
  | .[]
  | "\(.species)\t\(.count)"
' 304c0c00-b7f0-11f0-956f-005056b0ce82/data/gsz.baumkataster_baumstandorte.json
```

### find the oldest tree

```bash
jq '
  .features
  | map(select(.properties.pflanzjahr != null))
  | sort_by(.properties.pflanzjahr)
  | .[0]
  | {pflanzjahr: .properties.pflanzjahr, coordinates: .geometry.coordinates, feature: .}
' 304c0c00-b7f0-11f0-956f-005056b0ce82/data/gsz.baumkataster_baumstandorte.json
```

### make a histogram of trees per decade

```bash
jq -r '
  [.features[].properties.pflanzjahr
   | select(. != null)
   | (. / 10 | floor * 10)]
  | group_by(.)
  | map({decade: .[0], count: length})
  | sort_by(.decade)
  | .[]
  | "\(.decade) \(.count)"
' 304c0c00-b7f0-11f0-956f-005056b0ce82/data/gsz.baumkataster_baumstandorte.json \
| awk '{printf "%s | ", $1; for(i=0;i<$2/1000;i++) printf "*"; print ""}'
```
