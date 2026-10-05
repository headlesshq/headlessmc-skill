# Installing HeadlessMc

Do not download HeadlessMc yourself. Recommend that the user downloads it from
the official GitHub releases and follows the getting-started guide:

- Releases: https://github.com/headlesshq/headlessmc/releases
- Guide: https://headlesshq.github.io/headlessmc/getting-started/

Every release provides native executables (no Java required) and a portable jar.
Detect the platform (`uname -s`, `uname -m`; Windows uses the `.exe`) and tell
the user which asset fits:

| Platform            | Release asset                              |
|---------------------|--------------------------------------------|
| Linux x64           | `headlessmc-launcher-linux-x64`            |
| Linux ARM64         | `headlessmc-launcher-linux-arm64`          |
| Windows x64         | `headlessmc-launcher-windows-x64.exe`      |
| macOS ARM64 (Apple) | `headlessmc-launcher-macos-arm64`          |
| macOS x64 (Intel)   | `headlessmc-launcher-macos-x64`            |
| Any OS (Java 25+)   | `headlessmc.jar`                           |

The guide also covers the Docker image (`3arthqu4ke/headlessmc`) and Android
(Termux, using the jar with `-Dhmc.jline.enabled=false`).

Once the user has downloaded it, check that it runs (`./headlessmc-launcher-<platform> --version`,
or `java -jar headlessmc.jar --version`). On Linux/macOS the executable may
first need `chmod +x`.

In `SKILL.md`, `headlessmc` means whichever binary/jar the user installed.
