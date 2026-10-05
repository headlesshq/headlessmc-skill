# Installing HeadlessMc

Every release provides native executables (no Java required) and a portable jar.

| Platform            | Release asset                              |
|---------------------|--------------------------------------------|
| Linux x64           | `headlessmc-launcher-linux-x64`            |
| Linux ARM64         | `headlessmc-launcher-linux-arm64`          |
| Windows x64         | `headlessmc-launcher-windows-x64.exe`      |
| macOS ARM64 (Apple) | `headlessmc-launcher-macos-arm64`          |
| macOS x64 (Intel)   | `headlessmc-launcher-macos-x64`            |
| Any OS (Java 25+)   | `headlessmc.jar`                           |

## Find the newest release

Detect the platform first (`uname -s`, `uname -m`; on Windows use the `.exe`).
Resolve the newest release tag (HeadlessMc 3 releases may be marked as
pre-release, so do not rely on `/releases/latest`):

```sh
VERSION=$(curl -fsSL "https://api.github.com/repos/headlesshq/headlessmc/releases?per_page=1" \
  | grep -m1 '"tag_name"' | cut -d '"' -f4)
echo "$VERSION"
```

If `gh` is available, `gh release list -R headlesshq/headlessmc -L 5` works too.

## Linux / macOS
```sh
ASSET=headlessmc-launcher-linux-x64   # pick from the table above
curl -fL "https://github.com/headlesshq/headlessmc/releases/download/$VERSION/$ASSET" -o headlessmc
chmod +x headlessmc
./headlessmc --version
```
Optionally put it on the PATH, e.g. `mkdir -p ~/.local/bin && mv headlessmc ~/.local/bin/`.

## Windows (PowerShell)

The `sh` snippet above does not work on Windows. Resolve the tag in PowerShell
instead:
```powershell
$VERSION = (Invoke-RestMethod "https://api.github.com/repos/headlesshq/headlessmc/releases?per_page=1")[0].tag_name
$VERSION
curl.exe -fL --output headlessmc.exe --url "https://github.com/headlesshq/headlessmc/releases/download/$VERSION/headlessmc-launcher-windows-x64.exe"
.\headlessmc.exe --version
```

## Jar (any OS, needs Java 25+)
```sh
curl -fL "https://github.com/headlesshq/headlessmc/releases/download/$VERSION/headlessmc.jar" -o headlessmc.jar
java -jar headlessmc.jar --version
```

## Docker
```sh
docker pull 3arthqu4ke/headlessmc:latest
docker run -it 3arthqu4ke/headlessmc:latest   # `headlessmc` is on the PATH inside
```

## Android (Termux from F-Droid)

Install `openjdk`, use the jar, and disable JLine with
`-Dhmc.jline.enabled=false`.

In `SKILL.md`, `headlessmc` means whichever binary/jar you installed.
