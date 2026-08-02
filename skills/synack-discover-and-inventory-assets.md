---
name: Discover and inventory Synack assets
description: Seed asset discovery, then enumerate assets, ports, and suspected vulnerabilities across the attack surface.
api: openapi/synack-asset-discovery-openapi.yml
operations: [getSeedGroups, postSeedGroup, getSeeds, postSeed, getAssets, postAsset, getSuspectedVulnerabilities]
---

# Discover and inventory Synack assets

Drive Synack's attack-surface discovery and asset inventory.

## Auth
The asset-discovery, asset, and vulnerability services use **OAuth2** (implicit
flow, authorize at `login.synack.com`). Read needs a read scope
(`assetdiscovery_or`, `asset_or`/`asset_lr`); writes need a write scope
(`assetdiscovery_ow`, `asset_client_ow`/`asset_client_lw`). See scopes/synack-scopes.yml.
Hosts: `https://client.synack.com/api/asset-discovery`, `/api/asset`, `/api/vulnerability`.

## Steps
1. **Review seeds** — `getSeedGroups` (and `getSeeds`) to see what feeds discovery.
2. **Add discovery input** — `postSeedGroup` / `postSeed` to expand the surface;
   assets discovered from those seeds attach to the seed group's listing.
3. **Enumerate assets** — `getAssets` (paginated) across the org/listings; add
   assets manually with `postAsset` (pick the object matching the asset type:
   cloud, host, mobile, network, or web).
4. **Assess exposure** — `getSuspectedVulnerabilities` to review machine-suspected
   findings on the discovered assets.

## Rules
- OAuth2 scope determines visibility: org-level vs per-listing vs global. Request
  the narrowest scope that covers the task.
- Pagination is `page[number]`/`page[size]` (max 50). Errors are RFC 9457.
- Deleting a seed group removes assets discovered from its seeds — destructive; confirm first.
