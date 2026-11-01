# Play with Android users

Double-click **Play Android Online.cmd**, then use **Co-Op > Online** in the
game. Android players should use the supplied "New Server Fixed" APK. Use the
community server's account/login requirements in the game's online menus.
Local BitDemon accounts are not automatically accounts on this server.

The Android profile connects to **195.7.7.200**, the address in that APK's
`assets/touchHLE_default_options.txt`. Authentication and lobby use the game's
original TCP port **3074**. STUN is pointed at the same server. This profile
skips local server startup and disables our custom Wi-Fi relay. Relay room
names do not select Android Online lobbies.

To host a match through the community server, use that same Android Online
shortcut and create a match in **Co-Op > Online**. Forward **UDP 9784** in your
router to this PC and allow the game through Windows Firewall. Friends using
the reference APK can look for the match on that community server. No local
`dw-server.exe` is needed in this mode. Match discovery still requires the
community server; forwarding a gameplay port alone does not supply a lobby.

**Play Local Relay.cmd** starts the existing local BitDemon / Wi-Fi relay mode.
The regular **WawZombies.exe** continues to use your saved settings. Neither
shortcut edits settings or saves.

## Run your own server for Android and PC

1. Set `hostname` in `config.json` to your server's public IPv4 or hostname.
2. Forward/allow **TCP 3074** (combined Android auth/lobby), **TCP 3076**
   (content) and **UDP 3478 and 3074** (STUN) to the server PC. TCP 3075 remains
   available for older PC clients. TCP 3077 is only needed for PC Wi-Fi relay.
3. Double-click **Host Android Server.cmd**, or run `dw-server.exe` separately
   to keep matchmaking open when your game closes. Start a match in
   **Co-Op > Online**. The server provides matchmaking; a player runs the match.
4. On a PC hosting the actual match, forward **UDP 9784**. If the server and
   game are on this same PC, change `--waw-public-ip=auto` to your public IPv4
   in `touchHLE_options.txt`. This prevents local STUN from advertising
   loopback as the game's reachable address. For LAN-only play use the PC's
   LAN IPv4 instead. For a different external port, set `--waw-public-port`.
5. Android players must change the three host overrides to your server's IPv4.
   See **Android custom server options.txt** for the line to merge into their
   APK's `touchHLE_options.txt` (on Android, not the PC settings file).
   If their fork keeps the old address, replace the three `195.7.7.200` values
   in its `touchHLE_default_options.txt` as well. Relaunch the APK.
6. Other PC players launch with `--network-profile=android
   --dw-server=YOUR_SERVER_ADDRESS`. Everyone uses **Online**, the same server,
   and accounts accepted by that server.

The reference APK defaults to the public community server. Games on a private
server do not appear there automatically. Router forwarding requires a
reachable public IPv4; carrier-grade NAT can prevent incoming connections.
Your router/firewall configuration has not been changed by this release.

To use a different Android-compatible community server:

```powershell
.\WawZombies.exe --network-profile=android --dw-server=SERVER_IP_OR_HOSTNAME
```

If its administrator specifies a different authentication port, append
`--dw-auth-port=PORT`. Options after a profile override that profile. To save
the Android profile as your default, add `--network-profile=android` after
the existing server options in `touchHLE_options.txt`, within the
`com.activision.callofduty` entry. Remove conflicting old `--resolve` options
if you added any: explicit DNS overrides retain priority.

Both supplied copies contain the identical COD Zombies 1.5.0 game executable
(SHA-256 `4c940898943a1ac63318083b17bc04219251ad4b3cd2b846eaf41c1702f98f0d`).
The profile aligns server routing; it does not add the PC relay protocol to
Android. Public server availability, account acceptance and an actual mixed
PC/Android match require a live test. Online gameplay uses direct UDP; our PC
build binds gameplay port 9784, and router/firewall reachability can affect
hosting or joining even when the lobby connects.

## Optional cheats

In `touchHLE_options.txt`, change any of these from `off` to `on`, then restart:

| Setting | Effect from the cheats APK |
| --- | --- |
| `--cheat-health=off` | 999999 regular and Recruit player health |
| `--cheat-points=off` | 999999 initial solo points |
| `--cheat-wunderwaffe=off` | Nacht/Shi start and respawn weapon; 80/999999 ammo respectively |
| `--cheat-weak-dogs=off` | Shi dog health 1 and dog-amount constants 0 |

All four default to off. They apply to verified original 1.5.0 data in memory.
The original IPA and saves are not rewritten. Unsupported or already-modded
resources fail with a clear error when cheats are requested. These are the
APK's data edits, not an invincibility or infinite-ammo implementation.
The APK's unrelated corrupted Spanish translation is not included.
Co-op behavior follows the hosting game's rules.
