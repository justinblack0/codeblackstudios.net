# Glide - Developer Documentation

**Version:** 1.3.0
**Last Updated:** January 2026

## Overview

Glide is a route accessibility preview tool designed for wheelchair users and others who need to assess route conditions before traveling. It displays Street View imagery along planned routes, with elevation data to help identify steep sections.

## Architecture

Single-file HTML application (`index.html`) containing:
- HTML structure
- CSS styles (including responsive breakpoints and light/dark modes)
- Vanilla JavaScript (no build tools or frameworks)

## Google Maps APIs Used

Enable these APIs in Google Cloud Console:

| API | Purpose |
|-----|---------|
| Maps JavaScript API | Main map display |
| Directions API | Route calculation |
| Street View Static API | Street view images along route |
| Street View API | Interactive 360° look around |
| Elevation API | Elevation data for route |
| Places API | Location autocomplete |

## Key Features

### Route Planning
- Origin/destination input with Places Autocomplete
- Travel modes: Wheelchair, Walking, Bicycling, Driving
- Draggable routes for customization
- Route alternatives display

### Street View Preview
- Automatic image generation along route path
- Image crossfade transitions
- Playback controls (play/pause, next/prev, speed)
- Keyboard navigation support

### Elevation Profile
- Canvas-based elevation graph
- Color-coded difficulty (green/yellow/red)
- Scrubbing support (click/drag to navigate)
- Synced marker showing current position

### 360° Look Around
- Fullscreen Street View panorama modal
- Interactive exploration at any point

### Recent Routes
- Cached in localStorage
- Saves customized (dragged) routes with path data
- Stores elevation data and images for quick reload

## State Management

Global `state` object holds application state:

```javascript
state = {
  map: null,                    // Main Google Map instance
  miniMap: null,                // Mini map (currently disabled)
  directionsService: null,      // Google Directions Service
  directionsRenderer: null,     // Google Directions Renderer
  elevationService: null,       // Google Elevation Service
  routes: [],                   // Available route options
  selectedRouteIndex: 0,        // Currently selected route
  selectedMode: 'WHEELCHAIR',   // Travel mode
  images: [],                   // Street view image data
  currentIndex: -1,             // Current image index
  elevationData: [],            // Elevation points
  isPlaying: false,             // Playback state
  playbackSpeed: 1,             // Playback speed multiplier
  currentStep: 1,               // UI wizard step
  savedRoutePolyline: null,     // Polyline for cached routes
  savedRouteMarkers: null,      // A/B markers for cached routes
  mainMapMarker: null,          // Current position marker on main map
  cachedRecentRoute: null,      // Loaded recent route data
  // ... additional state properties
}
```

## Key Functions

### Route Finding
- `findRoutes()` - Calculates routes using Directions API
- `selectRoute(index)` - Selects a route alternative
- `loadRecentRoute(routeId)` - Loads a cached route

### Preview Generation
- `generatePreview()` - Generates street view images along route
- `sampleRoutePoints(path, interval)` - Samples points along route path
- `fetchStreetViewImages(points)` - Fetches images for sampled points

### Display
- `displayImage(index)` - Shows street view image with crossfade
- `drawElevationProfile(data)` - Renders elevation canvas
- `updateElevationMarker(progress)` - Updates marker on elevation graph

### Playback
- `play()` / `pause()` - Control playback
- `setPlaybackSpeed(speed)` - Adjust playback speed

### Caching
- `saveRecentRoute(...)` - Saves route to localStorage
- `getRecentRoutes()` - Retrieves cached routes
- `renderRecentRoutes()` - Displays recent routes list

## CSS Structure

### Breakpoints
- Desktop: default styles
- Tablet: `@media (max-width: 900px)`
- Mobile: `@media (max-width: 600px)`
- Small mobile: `@media (max-width: 400px)`

### Theme Support
- Dark mode: default
- Light mode: `body.light-mode` class

### Key CSS Classes
- `.sidebar` - Left sidebar with controls
- `.main-content` - Map/street view area
- `.streetview-panel` - Street view display container
- `.elevation-profile` - Elevation graph container
- `.playback-controls` - Navigation buttons
- `.mini-map-container` - Mini map (currently `display: none`)
- `.streetview-overlay` - Stats overlay on images

## Temporarily Disabled Features

### Mini Map
Location: `.mini-map-container`
```css
display: none; /* Temporarily disabled */
```

Related overlay adjustments (restore when re-enabling):
```css
/* Desktop */
.streetview-overlay { right: 230px; }
/* Tablet */
.streetview-overlay { right: 140px; }
/* Mobile */
.streetview-overlay { right: 110px; }
```

## PWA Support

- Configured via `manifest.json`
- Safe area insets handled for iOS
- Standalone display mode supported

## localStorage Schema

### Recent Routes
Key: `glide_recent_routes`

```javascript
{
  id: string,           // Unique identifier
  origin: string,       // Origin address
  destination: string,  // Destination address
  mode: string,         // Travel mode
  distance: string,     // Formatted distance
  duration: string,     // Formatted duration
  timestamp: number,    // Save timestamp
  images: array,        // Cached image data
  routePath: array,     // [{lat, lng}, ...] for custom routes
  elevationData: array  // Cached elevation points
}
```

## Event Handling

### Keyboard Shortcuts (when street view active)
- `Space` - Play/pause
- `ArrowLeft` - Previous image
- `ArrowRight` - Next image
- `Escape` - Exit fullscreen / close panels

### Touch Support
- Elevation profile scrubbing
- Swipe navigation (planned)

## Performance Considerations

- Image preloading for smooth playback
- Debounced elevation updates on route drag (500ms)
- Crossfade transitions for smooth image switching
- Canvas rendering for elevation profile

## File Structure

```
glide/
├── index.html          # Main application
├── manifest.json       # PWA manifest
├── glide-icon.svg      # App icon
├── glide-logo.svg      # Logo
└── DEVELOPER.md        # This file
```
