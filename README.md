# Taiwan Public Toilets Map

A jQuery Mobile web application that displays public toilet locations across Taiwan on Google Maps, with integrated gas station search and navigation features.

## Overview

This application leverages Taiwan's Open Data initiative to provide real-time access to public toilet locations throughout the country. Built with Google Maps integration, it offers two primary modules: a comprehensive toilet locator and a gas station finder with turn-by-turn navigation capabilities.

## Features

### Module 1: Public Toilet Locator
- **Comprehensive Coverage**: Displays thousands of public toilet locations from Taiwan's Open Data database
- **Smart Clustering**: Implements MarkerClusterer for optimal performance with large datasets
- **Type Differentiation**: Visual icons distinguish between men's, women's, accessible, and family-friendly facilities
- **Detailed Information**: Click any marker to view:
  - Facility grade (特優級, 優等級, 普通級)
  - Toilet type (men's, women's, accessible, parent-child, mixed)
  - Full address
  - Managing organization

### Module 2: Gas Station Search & Navigation
- **Proximity Search**: Find gas stations within a configurable radius (0-100km)
- **Google Places Integration**: Real-time search using Google Places API
- **Interactive Navigation**:
  - Single-click: View gas station name and details
  - Double-click: Display turn-by-turn driving directions from your location
- **Direction Instructions**: Step-by-step navigation rendered directly in the interface

### Additional Features
- **QR Code Scanner**: Camera-based QR code decoder (currently commented out but available in codebase)
- **Responsive Design**: jQuery Mobile ensures mobile-friendly interface

## Technology Stack

- **jQuery** 1.11.3
- **jQuery Mobile** 1.4.5 - UI framework with responsive navigation
- **Google Maps JavaScript API v3** - Mapping and geolocation
- **Google Places API** - Location search functionality
- **MarkerClusterer** - Efficient marker management

## Prerequisites

- Modern web browser with HTML5 geolocation support
- Google Maps API key with the following APIs enabled:
  - Maps JavaScript API
  - Places API
- HTTP server for local development (HTTPS recommended for production)

## Installation & Setup

```bash
# Clone the repository
git clone <repository-url>
cd pitstop-map

# Configure Google Maps API key
# Edit index.html at line 13 and replace the API key with your own:
# <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&libraries=places"></script>

# Serve with any static HTTP server
python -m http.server 8000

# Or using Node.js
npx serve

# Or using PHP
php -S localhost:8000
```

Access the application at `http://localhost:8000/index.html`

## Configuration

### Google Maps API Key

**Location**: `index.html` line 13

```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&libraries=places"></script>
```

**How to obtain an API key**:
1. Visit the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Maps JavaScript API and Places API
4. Create credentials (API key)
5. Restrict your API key by HTTP referrer or IP address for security

**Security Note**: The current API key in the repository is publicly exposed. For production use, always restrict your API key and consider using environment-based configuration.

## Usage

1. **Initial Setup**: Open the application in your browser
2. **Grant Permissions**: Allow location access when prompted (required for map centering)
3. **View Toilets**:
   - Click the "找廁所！" (Find Toilets) button on the home screen
   - Markers will appear showing all nearby public facilities
   - Click any marker for detailed information
4. **Search Gas Stations**:
   - Navigate to the "搜尋" (Search) tab
   - Adjust the radius slider (0-100, represents kilometers)
   - Enter a location in the search box or use your current location
   - Click "搜尋" button to find gas stations
   - Double-click a gas station marker to get driving directions

## Project Structure

```
pitstop-map/
├── index.html              # Main application page with three views
├── js/
│   ├── main.js            # Core application logic (map, markers, navigation)
│   ├── effects_saycheese.js  # Webcam integration for QR scanner
│   ├── say-cheese.js      # Camera capture library
│   └── qr/                # QR code decoder modules
│       ├── qrcode.js      # Main decoder
│       ├── detector.js    # Pattern detection
│       ├── decoder.js     # Data decoding
│       └── [15+ modules]  # Supporting decoder components
├── img/
│   └── toilet.png         # Application icon assets
├── taiwan_toilet.json     # Public toilet database (~353KB, thousands of records)
├── CLAUDE.md              # Development guide for Claude Code
└── README.md              # This file
```

## Data Source

**Taiwan Open Data: Public Toilet Locations**

The application uses `taiwan_toilet.json`, which contains comprehensive public toilet data across Taiwan with the following fields:

- **Location**: Country, City, Village, Address
- **Identification**: Number, Name
- **Geographic**: Latitude, Longitude (as strings)
- **Classification**:
  - Grade: 特優級 (Excellent), 優等級 (Good), 普通級 (Standard)
  - Type: 男廁所 (Men's), 女廁所 (Women's), 無障礙廁所 (Accessible), 親子廁所 (Family), 混合廁所 (Mixed)
  - Type2: Facility category (government, school, park, station, etc.)
- **Administration**: Managing organization

## Development Notes

### Key Implementation Details

- **Language**: Application UI is primarily in Traditional Chinese (Taiwan market)
- **Geolocation**: Requires HTTPS or localhost for browser geolocation API access
- **Radius Calculation**: The search radius slider shows 0-100 but multiplies by 1000 internally (actual range: 0-100,000 meters)
- **QR Scanner**: Full QR code scanning functionality exists in the codebase but is currently commented out in the UI (see `index.html` lines 47-53)
- **Map Instances**: Two separate map instances are maintained - one for the toilet view and one for the search view
- **Marker Clustering**: Applied only to toilet markers for performance; user position and gas station markers are not clustered

### Code Entry Points

- **Initialization**: `js/main.js` lines 23-38 (data loading and geolocation)
- **Map Setup**: `js/main.js` lines 40-208 (`showPosition` function)
- **Toilet Display**: `js/main.js` lines 210-270 (`showtoilet` function)
- **Gas Station Search**: `js/main.js` lines 148-207 (within `showPosition`)

### Testing Considerations

- Geolocation timeout is set to 10 seconds
- Error handling for four geolocation states (permission denied, unavailable, timeout, unknown)
- DirectionsRenderer overwrites previous routes (no route history)

## Browser Compatibility

- Modern browsers with HTML5 geolocation support
- Tested with Chrome, Firefox, Safari, Edge
- Requires user permission for location access
- Best experience on mobile devices with GPS

## Known Limitations

- API key is publicly visible in source (should be restricted in production)
- No offline functionality
- Toilet data is static (loaded from JSON file, not live API)
- QR scanner feature is disabled in current build

## License

No license specified.
