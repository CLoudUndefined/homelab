# Xray shared configuration

Shared Xray Core config files for all nodes.

## Layout

    conf.d/                  base files, loaded on every node
    examples/                building blocks for node-specific parts

conf.d contains base.json (log, stats, api, policy) and outbounds.freedom.json (direct + block). Node-specific parts are assembled from examples/ on the node.

examples:

    inbound.json.example     vless-reality and hysteria2 inbounds on port 443
    outbound.json.example    wireguard (WARP) outbound
    routing.json.example     default routing: block bittorrent, route RU traffic through warp

## Assembling a node config

1. Copy conf.d to the node's xray/xray/conf.d directory.
2. Copy needed examples into the same directory, rename to *.json and fill placeholders.
3. Run -test before restarting the container:

    xray run -confdir /etc/xray/conf.d -test

## certdumper

Needed when an inbound uses TLS certificates (hysteria2, plain TLS). Uncomment the certdumper service in the node's xray/docker-compose.yml. It dumps certificates from Traefik (acme.json) into ./certs and restarts the xray container on renewal. Mount ./certs at /certs and point tlsSettings paths at /certs/<domain>/cert.pem and /certs/<domain>/key.pem.