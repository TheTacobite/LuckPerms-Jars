# LuckPerms-Jars

A version-pinned mirror of the exact [LuckPerms](https://luckperms.net/) jars this network runs -
`LuckPerms-Velocity` and `LuckPerms-Bukkit`. Not a source fork, not a rebuild - just the same
finished jars you'd otherwise download from luckperms.net, re-hosted here so the network's deploy
tooling pulls from a repo you control instead of tracking upstream automatically.

**Why**: the deploy pipeline for `NetCoreVelocity`/`NetCorePaper` already pulls jars from GitHub
Releases (see [NetCoreDeploy](https://github.com/TheTacobite/NetCoreDeploy)'s `ubuntu/61-fetch-github-release.sh`). Doing the same for
LuckPerms means every jar the network runs comes from one consistent mechanism - and, more
importantly, this repo only ever has the version you've deliberately chosen. You decide when to
upgrade by pushing a new jar here; nothing pulls a newer LuckPerms release on its own. Same
reasoning as [`TacoLimbo`](https://github.com/TheTacobite/TacoLimbo) (the network's own
version-pinned fork of `LOOHP/Limbo`) - pin to a known-good version, don't silently drift with
upstream.

## Current version

`5.5.59` - matches the version this network's LuckPerms-backed permission system is actually
running.

## ⚠️ Needs a GitHub Release, not just a commit

The deploy tooling (`61-fetch-github-release.sh`) fetches jars via the **GitHub Releases API**
(`/releases/latest` or a specific tag), not by reading files off the `main` branch directly. Right
now these two jars are committed straight into `main` ("Add files via upload") - **no Release has
been cut yet**, so a fetch against this repo will currently fail.

To actually make this usable:
1. GitHub → **Releases → Draft a new release**.
2. Tag it (e.g. `v5.5.59`, matching the LuckPerms version above).
3. Attach `LuckPerms-Velocity-5.5.59.jar` and `LuckPerms-Bukkit-5.5.59.jar` as release assets.
4. Publish.

NetCoreDeploy's `versions.env` pins which tag the network uses (`LUCKPERMS=`).

## Updating to a newer LuckPerms version

1. Download the new `LuckPerms-Velocity-<version>.jar` / `LuckPerms-Bukkit-<version>.jar` from
   [luckperms.net/download](https://luckperms.net/download).
2. Cut a new Release here, tagged with the new version, with those jars attached.
3. Set `LUCKPERMS=` to the new tag in NetCoreDeploy's `versions.env` and push.
4. Run `sudo netcore update` on Box 2 and each game server box. It updates both the Paper and
   Velocity jars, restarting servers one at a time.

Nothing here updates automatically - that's the point.
