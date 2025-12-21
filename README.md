# HostAtHome Registry

YAML definitions of available game servers for the HostAtHome CLI.

## Structure

```
registry/
├── games/
│   ├── minecraft.yaml
│   ├── cs2.yaml
│   └── zomboid.yaml
├── schema.json
└── README.md
```

## Game Definition Format

Each game is defined in `games/<name>.yaml`:

```yaml
name: minecraft                              # Unique ID (lowercase)
display_name: Minecraft Java Edition         # Human-readable name
description: Vanilla Minecraft server        # Brief description
image: ghcr.io/hostathome/minecraft-server:latest
ports:
  player: 1024                               # External port
  rcon: 1025
internal_ports:
  player: 25565                              # Container internal port
  rcon: 25575
volumes:
  - ./{game}-server:/data                    # {game} is replaced by CLI
config_schema:                               # Optional: config.yaml schema
  server:
    motd:
      type: string
      default: "HostAtHome Minecraft"
```

## Adding a New Game

1. Create `games/<name>.yaml` following the schema
2. Ensure the Docker image is published to `ghcr.io/hostathome/<name>-server`
3. Submit a PR

## Usage by CLI

The CLI embeds these YAML files at build time and auto-updates from this repo:
- Bundled: immediate offline access
- Cached: `~/.hostathome/cache/registry/`
- Remote: `https://raw.githubusercontent.com/hostathome/registry/main/games/`
