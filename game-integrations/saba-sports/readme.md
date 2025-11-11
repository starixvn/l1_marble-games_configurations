# SABA Sports Integrations

[[_TOC_]]

## 1. mergeEvent Section

```json
{
  "mergeEvent": {
    "productCode": "SM_CLUB_WC_25",
    "productTypeKey": "MARBLE",
    "_id": null,
    "__v": null,
    "streamServer": "https://d10r8t0fxmjzn1.cloudfront.net",
    "videoExtension": "mp4"
  }
}
```

| Property         | Value                                                            | Description                                             |
| ---------------- | ---------------------------------------------------------------- | ------------------------------------------------------- |
| `productCode`    | SM_CLUB_WC_25, SM_SPINNER_GALAXY, SM_SPINNER_EPL, SM_SPINNER_UCL | Code representing the product associated with the event |
| `productTypeKey` | MARBLE                                                           |                                                         |
| `streamServer`   | https://d10r8t0fxmjzn1.cloudfront.net                            | The CDN link                                            |
| `videoExtension` | .mp4                                                             |                                                         |
| `_id`            | null                                                             |                                                         |
| `__v`            | null                                                             |                                                         |

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

### 3.2 Remove Temporary Fields

Cleans up temporary fields used for URL construction.

```json
{ "op": "remove", "path": "/streamServer" },
{ "op": "remove", "path": "/theme" },
{ "op": "remove", "path": "/videoExtension" }
```

### 3.3 Remove Selections

Cleans up temporary fields used for URL construction.

```json
{ "op": "remove", "path": "/eventDetails/0/selections" },
{ "op": "remove", "path": "/eventDetails/1/selections" }
```

### 3.4 Remove Game ID

```json
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

### 3.6 Filter Low-Odds Markets

Removes markets that have selections with odds below 1.1.

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($detail) { $merge([$detail, {'markets': $filter($detail.markets, function($market) { $count($market.selections[price < 1.1]) = 0 })}])})"
},
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "$map(eventDetails, function($v) { $merge([$v, {'markets': $count($v.markets.selections) > 0 ? $v.markets : []}]) })"
}
```

## 4. markets Section

Markets define betting options. Each market has:

- `name`: Internal market identifier
- `merge`: Display metadata

  - `marketID`: Unique numeric ID
  - `marketNameKey`: Translation key
  - `marketName`: Display name

- `selections`: Array of betting options

  - `selectionID`: Unique numeric ID
  - `column`: Display column position
  - `selectionOrder`: Order within column
  - `selectionNameLongKey`: Translation key
  - `name`: Maps to API selection name
