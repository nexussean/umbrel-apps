# OpenCode 2.0.22 for Umbrel

A personal community app store with `sean-opencode-v2`. The official upstream
multi-architecture image is pinned to its verified registry index digest.

## Install

1. Add `https://github.com/nexussean/umbrel-apps` as a community app store.
   The standard install flow requires this repository to be public.
2. On Umbrel, open App Store → community app stores (in its menu/settings),
   then install **OpenCode V2** from **Sean’s Apps**.
3. Open the app through Umbrel and use its displayed app password to sign in.
4. In OpenCode Desktop 2.0.22 → Settings → Server → Add server, enter
   `http://YOUR-UMBREL-TAILSCALE-IP:4097` and the password displayed by Umbrel.
   The authentication username is `opencode`.

Ports: browser launch through Umbrel on **4098**; direct desktop API on **4097**;
internal container on 4096. Your existing V1 app can remain on host port 4096.
Before installing, ensure 4097 and 4098 are unused on your Umbrel server.
The direct API requires the OpenCode password and is accessible on host interfaces,
including Tailscale; no router port forwarding is needed.

## Data and projects

The entire container home persists in this app’s own `data/opencode` directory.
The app runs as UID/GID 1000. Projects inside `/home/opencode` persist. It has no
host Docker socket, privileged mode, or broad host filesystem mounts.
Provider accounts and project setup begin fresh. Do not point V1 and V2 at the
same live data directory. If you want existing sessions migrated, stop V1 and
make a separate backup/copy before planning that migration.

## Verify after installation

Run on your Mac (curl prompts for the password, keeping it out of shell history):

```sh
curl --fail-with-body -u opencode http://YOUR-UMBREL-TAILSCALE-IP:4097/api/info
```

Expect JSON reporting version 2.0.22. Without credentials the API should reject
requests. Verify web login, adding a provider, creating a session, and persistence
after an Umbrel app restart.

## Validation and limits

Verified: official image tag exists and its index includes linux/amd64 and
linux/arm64; image entrypoint is `opencode`; 2.0.22 accepts the serve flags;
source uses OPENCODE_SERVER_PASSWORD for foreground serve; package IDs, paths,
ports, image digest and persistent mounts were checked. A local macOS 2.0.22
server started with isolated test data: authenticated `/api/info` returned 200
with version 2.0.22, unauthenticated `/api/info` returned 401, and `/` returned 200.

This package has not yet been installed on an Umbrel server. Container startup,
Umbrel proxy behavior and the web login must be confirmed on your device.

Sources:
- https://github.com/getumbrel/umbrel-community-app-store
- https://github.com/getumbrel/umbrel-apps/blob/master/.claude/skills/umbrel-package-app/SKILL.md
- https://github.com/anomalyco/opencode/blob/v2.0.22/packages/cli/Dockerfile
- https://github.com/anomalyco/opencode/blob/v2.0.22/packages/cli/src/server-process.ts
- https://opencode.ai/v2/docs/cli/web
