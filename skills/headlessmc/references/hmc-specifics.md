# Controlling a running client (hmc-specifics)

With the [hmc-specifics](https://github.com/headlesshq/hmc-specifics) mod in the
instance's `mods` folder (Fabric/Forge/NeoForge, most major versions from 1.7.10
to 26.x), lines written to HeadlessMc's stdin while the game runs are executed
inside the game:

| Command | Purpose |
|---|---|
| `gui` | Dump the current screen: buttons, text fields, slots with ids (`gui --tooltip <id>`) |
| `click <id>` | Click a GUI element / inventory slot |
| `text <id> "<text>"` | Fill a text field |
| `render` | Dump every string rendered on screen |
| `close` / `menu` / `menu -inventory` | Close screen / open pause menu / inventory |
| `key <key>` | Press a key (`--duration`, `-release`), e.g. `key f2` = screenshot |
| `. <msg>` / `msg <msg>` | Send chat message |
| `/<command>` | Send chat command, e.g. `/gamemode creative` |
| `connect <address>` / `disconnect` | Join / leave a server |
| `memory` | Memory stats |
| `quit` | Quit the game |

Typical loop: `gui` → read ids → `click`/`text` → `gui` again to verify.

To drive the client from a script or agent, see the automation recipe
(section 5 of `SKILL.md`): launch in the background with a FIFO as stdin and
write these commands into the FIFO.
