# Marble TV Integrations

[[_TOC_]]

## 1. mergeEvent Section

```json
{
  "mergeEvent": {
    "_id": null,
    "__v": null,
    "streamServer": "https://d10r8t0fxmjzn1.cloudfront.net",
    "videoExtension": "m3u8",
    "voiceConfig": "https://d10r8t0fxmjzn1.cloudfront.net/youtube/config_voice.json"
  }
}
```

| Property         | Value                                                           | Description                       |
| ---------------- | --------------------------------------------------------------- | --------------------------------- |
| `streamServer`   | https://d10r8t0fxmjzn1.cloudfront.net                           | The CDN link                      |
| `voiceConfig`    | https://d10r8t0fxmjzn1.cloudfront.net/youtube/config_voice.json | Commentator's voice configuration |
| `videoExtension` | .m3u8                                                           |                                   |
| `_id`            | null                                                            |                                   |
| `__v`            | null                                                            |                                   |

## 2. mergeEventDetails Section

```json
{
  "mergeEventDetails": []
}
```

## 3. patches Section

### 3.1 Playlist and Goal Info URL Construction

Constructs full playlist URLs by combining `streamServer`, `theme`, and `playlist`/`goalInfo` fields.

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v) { $merge([$v, {'playlist': $exists($v.playlist) and $trim($v.playlist) != \"\" ? streamServer & '/' & theme & '/' & $v.playlist & '.' & videoExtension : \"\" }])})"
},
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v) { $merge([$v, {'goalInfo': $exists($v.goalInfo) and $trim($v.goalInfo) != \"\" ? streamServer & '/' & theme & '/' & $v.goalInfo & '.json' : \"\" }])})"
}
```

### 3.2 In-Game Event Info URL

Creates an additional `inGameEventInfo` URL with `event_` prefix.

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v) { $merge([$v, {'inGameEventInfo': $exists($v.goalInfo) and $trim($v.goalInfo) != \"\" ? streamServer & '/' & theme & '/event_' & $v.goalInfo & '.json' : \"\" }])})"
}
```

### 3.3 Add Phase Index

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v, $i) { $merge([$v, {'phase': $i }])})"
}
```

### 3.4 Clean Up Fields

```json
{ "op": "remove", "path": "/streamServer" },
{ "op": "remove", "path": "/theme" },
{ "op": "remove", "path": "/videoExtension" },
{ "op": "remove", "path": "/eventDetails/0/markets" },
{ "op": "remove", "path": "/eventDetails/1/markets" },
{ "op": "remove", "path": "/gameId" }
```

### 3.5 Transform Results

Flattens the result object to just show the final score.

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v) { $merge([$v, {'result': $v.result.finalScore ?? \"\"}]) })"
}
```

## 4. markets Section

```json
{
  "markets": []
}
```
