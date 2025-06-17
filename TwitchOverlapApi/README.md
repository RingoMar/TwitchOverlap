# TwitchOverlapApi README

### Overview

TwitchOverlapApi provides endpoints to retrieve data about Twitch channel viewer overlaps, calculated every 30 minutes for channels with over 1,000 viewers.

### Prerequisites

- Configure appsettings.json with the DefaultConnection string for PostgreSQL:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Username=postgres;Password=postgres;Database=postgres;"
  }
}
```

### API Endpoints

1. **Get Channel Overlap**

**Endpoint**: `GET /api/overlap/{channelName}`

**Description**: Retrieves a list of channels that share viewers with the specified channel, including the percentage of shared viewers.
**Parameters**:

- channelName (string): The Twitch channel name (case-insensitive).

**Example Request**:

```bash
curl http://localhost:5000/api/overlap/ninja
```

**Example Response**:

```json
[
  {
    "ChannelName": "shroud",
    "SharedViewers": 5000,
    "ProbabilityScore": 0.25
  },
  {
    "ChannelName": "timthetatman",
    "SharedViewers": 3000,
    "ProbabilityScore": 0.15
  }
]
```

2. **Get All Channels**

**Endpoint**: `GET /api/channels`
**Description**: Returns a list of all tracked Twitch channels.

**Example Request**:

```bash
curl http://localhost:5000/api/channels
```

**Example Response**:

```json
["ninja", "shroud", "timthetatman"]
```

3. **Get Community Graph**

**Endpoint**: `GET /api/graph`
**Description**: Retrieves a force-directed graph dataset of viewer overlaps for the top 1,500 channels, suitable for visualization.

**Example Request**:

```bash
curl http://localhost:5000/api/graph
```

```json
Example Response:

{
"nodes": [
{ "id": "ninja", "group": 1 },
{ "id": "shroud", "group": 1 }
],
"links": [
{ "source": "ninja", "target": "shroud", "value": 5000 }
]
}
```

### Notes

Viewer overlap is calculated as `channel_shared/total_shared`, where `channel_shared` is the number of shared viewers with a specific channel, and `total_shared` is the total unique shared viewers across all channels.
