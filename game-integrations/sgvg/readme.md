# SABA.games/VGaming Integrations

[[_TOC_]]

## 1. mergeEvent Section

```json
{
  "mergeEvent": {
    "_id": null,
    "__v": null,
    "operatorGameId": "158",
    "config": {
      "delay": {
        "videoDelayInMs": 6000
      },
      "bet": {
        "delayPlaceBetMs": 0,
        "soonLockBetMs": 3000
      }
    }
  }
}
```

| Property         | Value | Description                     |
| ---------------- | ----- | ------------------------------- |
| `operatorGameId` | 158   | The game id in SG/VG            |
| `config`         |       | The marble client configuration |
| `_id`            | null  |                                 |
| `__v`            | null  |                                 |

## 2. mergeEventDetails Section

```json
{
  "mergeEventDetails": []
}
```

## 3. patches Section

### 3.1 Playlist URL Construction

```json
{
  "op": "jsonata",
  "path": "/eventDetails",
  "expression": "eventDetails ~> | $ | {'playlist': 'https://mgtv.ap.vgaming88.net?token=' & $base64encode('game=2004&operator=vgaming')} |"
}
```

### 3.2 Clean Up Fields

```json
{ "op": "remove", "path": "/theme" },
```

## 4. markets Section

```json
{
  "markets": [
    {
      "name": "SGVG_MARKET_1X2",
      "merge": {
        "hedgeBetExclusion": ["ft_1x2_draw"]
      },
      "selections": [
        {
          "name": "ft_1x2_home",
          "betChoice": "ft_1x2_home"
        },
        {
          "name": "ft_1x2_away",
          "betChoice": "ft_1x2_away"
        },
        {
          "name": "ft_1x2_draw",
          "betChoice": "ft_1x2_draw"
        }
      ]
    }
  ]
}
```

Markets define betting options. Each market has:

- `name`: Internal market identifier
- `merge`:

  - `hedgeBetExclusion`

- `selections`: Array of betting options

  - `betChoice`: Bet choice in SG/VG System
  - `name`: Maps to API selection name
