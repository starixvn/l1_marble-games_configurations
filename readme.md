# Marble Product Configuration

This documentation suite provides comprehensive guidance for configuring and deploying the Marble product across different platforms. Whether you're setting up deployment environments, configuring game settings, or integrating with various gaming platforms, you'll find detailed guides for each aspect of the system.

[[_TOC_]]

## 1. Deployment

**For**: Developers  
**Purpose**: Environment-specific deployment settings

## 2. Game

**For**: Product Managers, Developers  
**Purpose**: Define game rules, teams, and match schedules

### 2.1 Schema definition

**Covers:**

- Schema structure and validation rules
- Field definitions and constraints
- Team selection modes
- Match configuration patterns

**When to Use:**

- Understanding configuration structure
- Validating configuration files
- Building admin interfaces

### 2.2 Game Configuration

**Covers:**

- Team setup and management
- Match scheduling strategies
- Video asset management
- Team selection modes

**When to Use:**

- Adding new teams
- Scheduling tournaments
- Managing video assets
- Configuring game modes

**Quick Example:**

```json
{
  "enabled": true,
  "teams": ["clb_liv", "clb_rma", "clb_bar"],
  "matches": [
    {
      "homeTeam": "clb_liv",
      "awayTeam": "clb_rma",
      "theme": "ucl_liv"
    }
  ],
  "teamSelectionMode": 2
}
```

## 3. Integrations

**For**: Developers  
**Purpose**: Configure how event data is transformed and structured for different platforms  
**Covers:**

- SabaSports, SABA.games/VGaming integration
- MarbleTV integration (streaming only)
- JSONata transformation patterns
- Market configuration
- Data transformation pipelines

**When to Use:**

- Integrating with betting platforms
- Setting up streaming services
- Configuring market structures
- Customizing data transformations

**Quick Example:**

```json
{
  "mergeEvent": { ... },
  "mergeEventDetails": [],
  "patches": [ ... ],
  "markets": [ ... ]
}
```

**Operators:**

- [Saba Sports](./game-integrations/saba-sports/readme.md)
- [Marble TV](./game-integrations/marble-tv/readme.md)
- [SABA.games/VGaming](./game-integrations/sgvg/readme.md)
