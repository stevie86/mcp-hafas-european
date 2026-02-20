# mcp-hafas-european

MCP server for European public transport queries via [hafas-client](https://github.com/public-transport/hafas-client).
Supports 50+ rail and transit operators across Europe (and some in the US) — no API key required.

**Status:** Ready

---

## Prerequisites

- [Bun](https://bun.sh) runtime (`curl -fsSL https://bun.sh/install | bash`)
- No API keys needed — uses publicly accessible HAFAS endpoints

---

## Install

```bash
git clone git@github.com:McCullonas/mcp-hafas-european.git
cd mcp-hafas-european
bun install
```

---

## Supported Operators

50+ operators accessible via the `operator` parameter:

| Code | Operator | Country |
|------|----------|---------|
| `db` | Deutsche Bahn | 🇩🇪 Germany |
| `oebb` | ÖBB | 🇦🇹 Austria |
| `sbb` | SBB (via hafas) | 🇨🇭 Switzerland |
| `sncb` | SNCB/NMBS | 🇧🇪 Belgium |
| `pkp` | PKP Intercity | 🇵🇱 Poland |
| `rejseplanen` | Rejseplanen | 🇩🇰 Denmark |
| `irish-rail` | Irish Rail | 🇮🇪 Ireland |
| `cfl` | CFL | 🇱🇺 Luxembourg |
| `bvg` | BVG Berlin | 🇩🇪 Germany |
| `vbb` | VBB Berlin-Brandenburg | 🇩🇪 Germany |
| `rmv` | RMV (Frankfurt region) | 🇩🇪 Germany |
| `bart` | Bay Area Rapid Transit | 🇺🇸 USA |

Run `hafas_list_operators` to see the complete list.

---

## Available Tools

### `hafas_list_operators`
List all available operator profiles with their codes, names, and countries.

No parameters.

### `hafas_search_stations`
Search for stations by name across a specific operator's network.

**Parameters:**
- `operator` — Operator code (e.g. `"db"`, `"oebb"`)
- `query` — Station name to search (e.g. `"Frankfurt"`, `"Wien Hauptbahnhof"`)
- `results` *(optional)* — Max results (default 5)

### `hafas_get_departures`
Get upcoming departures from a station.

**Parameters:**
- `operator` — Operator code
- `stationId` — Station ID (from `hafas_search_stations`)
- `when` *(optional)* — ISO 8601 datetime (default: now)
- `duration` *(optional)* — Timespan in minutes (default 60)

Returns: line names, destinations, scheduled/actual times, delays, platform numbers.

### `hafas_get_arrivals`
Get upcoming arrivals at a station.

**Parameters:**
- `operator` — Operator code
- `stationId` — Station ID
- `when` *(optional)* — ISO 8601 datetime
- `duration` *(optional)* — Timespan in minutes

### `hafas_get_journeys`
Plan a journey between two stations, including connections and transfers.

**Parameters:**
- `operator` — Operator code
- `fromId` — Origin station ID
- `toId` — Destination station ID
- `when` *(optional)* — Departure datetime (ISO 8601)
- `results` *(optional)* — Number of journey options (default 3)

Returns: routes, legs, transfer info, duration, delays, platform numbers.

---

## MCP Client Configuration

Add to your `settings.json`:

```json
{
  "mcpServers": {
    "hafas-european": {
      "command": "bun",
      "args": ["run", "/path/to/mcp-hafas-european/src/index.ts"]
    }
  }
}
```

No environment variables required.

---

## Development

```bash
# Run directly
bun run src/index.ts

# Build
bun build src/index.ts --outdir dist --target node
```

---

## Data Source

This server uses [hafas-client](https://github.com/public-transport/hafas-client) by
[derhuerst](https://github.com/derhuerst), which connects to publicly accessible HAFAS
endpoints operated by each transit authority. Usage is subject to each operator's terms.

---

## Licence

MIT
