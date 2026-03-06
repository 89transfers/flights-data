# flight-stats-scraper

A Cloudflare Worker that scrapes flight data from FlightStats.com for Palma de Mallorca Airport (PMI).

## Features

- Fetches both departures and arrivals for PMI airport
- Covers 4 days of flight data (today + next 3 days)
- Handles overnight flights correctly
- Uses KV storage for caching (3-hour TTL, refreshed via cron)
- Concurrent requests for improved performance
- CORS support

## Endpoints

### Get Flights
```
GET /flights?origin=BCN&destination=PMI&date=2026-03-18
```

**Query Parameters:**
| Parameter | Required | Description |
|-----------|----------|-------------|
| `origin` | Yes | Origin airport IATA code (e.g., `BCN`) |
| `destination` | Yes | Destination airport IATA code (e.g., `PMI`) |
| `date` | Yes | Flight date in `YYYY-MM-DD` format |

### Refresh Cache
```
GET /refresh-data
```
Manually triggers a refresh of all flight data. Also runs automatically every 3 hours via cron.

## Response Format

```json
{
  "totalFlights": 5,
  "flights": [
    {
      "flight_id": "VY3914_2026-03-18",
      "origin_iata": "BCN",
      "destination_iata": "PMI",
      "origin_name": "Barcelona",
      "destination_name": "Palma de Mallorca",
      "departure": "08:30",
      "arrival": "09:25",
      "departure_date": "2026-03-18",
      "arrival_date": "2026-03-18",
      "duration": 55,
      "company": "Vueling",
      "company_logo": "https://cdn.jsdelivr.net/gh/spydogenesis/airlines-logo@latest/airlines-logo/200x200_v2/VY.png",
      "flight": "VY3914"
    }
  ]
}
```

## Development

```bash
npm install
npm run dev
```

## Deployment

```bash
npm run deploy
```
