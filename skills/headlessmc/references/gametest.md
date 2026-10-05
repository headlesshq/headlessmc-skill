# GameTests with MC-Runtime-Test

On newer Minecraft versions MC-Runtime-Test runs Minecraft's **GameTest
Framework** (`/test runall`) in the real client, so the mod's own GameTests run
as part of the CI job. Workflow setup is in [mc-runtime-test.md](mc-runtime-test.md).

## Configuration

Pass JVM properties through `headlessmc-command`, e.g.
`--jvm "-Djava.awt.headless=true -DMcRuntimeGameTestMinExpectedGameTests=1"`:

- `-DMcRuntimeGameTestMinExpectedGameTests=<n>` fails the run if fewer tests ran,
  so an empty run can't pass.
- `-DMcRuntimeGameTest=false` only checks that the world loads, without running tests.
- `-DMcRuntimeGameTestFailOnOptional=false` lets optional tests fail.
- 26.3: `-DMcRuntimeGameTestInstance=<namespace:test>` selects a single test instance.

For Fabric, set the `fabric-gametest-api` action input to the Fabric GameTest API
version. Forge/NeoForge GameTest discovery may need extra setup. See the
[README](https://github.com/headlesshq/mc-runtime-test).
