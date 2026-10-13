# homelab

Docker Compose definitions for a handful of VPS nodes that keep my internet life running: mail, matrix calls, forgejo CI, and a bunch of little games pretending to be harmless websites.

Each top-level directory is one server. Inside it, one subdirectory per service. Every service lists the variables it needs in `.env.example`.

## Nodes

- **restless-desert**: matrix, livekit, traefik, web-camouflage
- **noble-angel**: mail, forgejo, woodpecker, navidrome, vaultwarden, traefik, web-camouflage
- **fierce-lion**: headscale, rss, search, traefik, web-camouflage
- **lucky-sun**: traefik, web-camouflage
- **toxic-union**: not provisioned yet

Every node also runs xray; its shared configuration lives in `common/xray/`.

The `web-camouflage` service on each node is what unauthorised visitors see. Traefik routes point at small browser games (tetris, wordle, rubiks cube...), so the node answers with a game instead of a 404.

## Layout

- `<node>/<service>/docker-compose.yml`: what runs on that node
- `<node>/<service>/.env.example`: variables to fill at deploy
- `common/xray/`: shared xray configuration, assembled per node at deploy; see its `README.md`
- `ansible/`: server provisioning. Bootstrap a fresh Debian box, install docker, create a docker manager user. Inventory lives in `inventory.ini` (template: `inventory.ini.example`)

## Conventions

- Placeholders use `example.com`, `PLACEHOLDER`, `0.0.0.0`.
- Domain names enter compose only through env variables.