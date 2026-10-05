# MC-Runtime-Test — testing mods in CI / GitHub Actions

MC-Runtime-Test is a GitHub Action built on HeadlessMc. It catches runtime bugs
(crashing mixins, missing classes, broken registries) that only show up when the
game really starts. In each run it:

- downloads HeadlessMc, the requested Minecraft version and the mod loader
  (and caches `.minecraft`),
- launches the client under Xvfb (or headless with `-lwjgl`) with an offline
  account, which is the CI use case offline accounts exist for,
- adds a small mod that joins a singleplayer world, waits for chunks to load and
  quits after a few seconds. The job fails if the game crashes,
- runs Minecraft's **GameTest Framework** (`/test runall`) on newer versions, so
  the mod's own GameTests run in a real client.

Supported: Forge and Fabric from 1.7.10 up, NeoForge from 1.20.2 up, through 26.x.
Check the [README table](https://github.com/headlesshq/mc-runtime-test#supported-minecraft-versions-and-modloaders)
for the exact versions.

## Minimal workflow

Put the mod jar into `run/mods` before the action runs:

```yaml
name: Run Minecraft Client
on: [push, pull_request, workflow_dispatch]

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: 25
          distribution: temurin
      - name: Build mod
        run: ./gradlew build
      - name: Stage mod for test client
        run: |
          mkdir -p run/mods
          cp build/libs/<your-mod>.jar run/mods
      - name: Run MC test client
        uses: headlesshq/mc-runtime-test@4.5.2   # check releases for the newest tag
        with:
          mc: 26.1.1
          modloader: fabric            # fabric | forge | neoforge
          regex: .*fabric.*            # matches the mod loader's version jar
          mc-runtime-test: fabric      # fabric | lexforge | neoforge | none
          java: 25
```

Before writing the workflow, look up the newest tag at
https://github.com/headlesshq/mc-runtime-test/releases. Match `java` to the
Minecraft version: 8 for 1.16.5 and older, 16 for 1.17.x, 17 for 1.18–1.20.4, 21 for 1.20.5–1.21.x,
25 for 26.x. Read the mod's build file to get the Minecraft version and loader;
don't guess them.

## Important inputs

| Input | Purpose |
|---|---|
| `mc`, `modloader`, `regex`, `java`, `mc-runtime-test` | Required (see above) |
| `fabric-api` | Fabric API version to download (e.g. `0.97.0`), default `none` |
| `fabric-gametest-api` | Fabric GameTest API version, default `none` |
| `xvfb` | Run under Xvfb (default `true`). If `false`, add `-lwjgl` to `headlessmc-command` |
| `headlessmc-command` | Extra HeadlessMc arguments, default `--jvm "-Djava.awt.headless=true"` |
| `dummy-assets` | Skip downloading real assets (default `true`, faster) |
| `cache-mc` | `github` (default), `blacksmith`, `false` |
| `hmc-version` | HeadlessMc version the action downloads (it pins its own default) |

## GameTests

See [gametest.md](gametest.md) for running the GameTest Framework and the
related JVM properties.

## Testing several versions

Use a `strategy.matrix` over `mc`/`modloader`/`java`, building the matching mod jar
in each job. The
[hmc-specifics matrix workflow](https://github.com/3arthqu4ke/hmc-specifics/blob/main/.github/workflows/run-matrix.yml)
is a full example.
