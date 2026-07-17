 > [!CAUTION]
  > **I intentionally removed the dangerous parts that would make this easy to abuse**: repeated same-socket `s`
  respawn spam, unsafe validation replay, post-spawn validation abuse, and unnecessary movement packet flooding.
  >
  > Some packet paths were kept strict on purpose. It does not expose every anti-bot-sensitive path just so people can spam servers
--------------------------------


# arras-protocol-client

Protocol-level Arras client runner with a browser controller, direct socket resolution, proxy support, tank upgrades, stat builds, manual coordinate movement, autofire, and reconnect-based respawn handling.

This project is built for running lightweight protocol-only clients without opening a full browser for every bot.

## Requirements

- Node.js 18+
- npm
- Tampermonkey

## Install

```bash
git clone https://github.com/tankstersarra/arras-protocol-client.git
cd arras-protocol-client
npm install
```

## Tampermonkey Controller

Import/upload `message.txt` into Tampermonkey.

After installing the userscript, open Arras normally in the browser. The controller panel is used to select tanks, set bot count, launch protocol-only clients, use manual coordinates, autofire, and other controls.

## Run

Start the server:

```bash
node server.js
```

The server listens on:

```txt
http://localhost:8082
```

Open the controller page in the browser, choose the target hash/tank/count, then launch protocol-only bots from the UI.

## Codespaces / Cloud Run

Install and run the same way:

```bash
git clone https://github.com/tankstersarra/arras-protocol-client.git
cd arras-protocol-client
npm install
node server.js
```

If exposing the server through a tunnel such as ngrok, keep logs minimal. The server already filters noisy protocol logs from the live console; detailed child logs are written locally.


## Logs

Live console logs are intentionally short:

- socket open / handshake
- spawn
- mode
- first bot position
- death / respawn / reconnect
- errors / disconnects

Detailed child output is written to:

```txt
protocol-only-child.log
```


## Notes

  - Auto respawn uses a fresh WebSocket reconnect by default. Same-socket respawn is avoided because it can trip validation/state issues after death.
  - Basic tank has no stat build by default.
  - Protocol-only clients keep the selected tank, build, autofire state, and manual target after reconnect respawn.

  - Manual coordinate movement is locked to the coordinates submitted when the button is pressed. Moving your own tank afterward does not keep dragging the
    bots around unless you fill/send a new target.

  - Direct socket resolution is attempted first. Headless resolution is only a fallback when direct status/build data is unavailable.
  - Movement uses compact protocol input packets and calibrated coordinate scaling, so bots can move toward fixed map positions without needing a full
    browser instance per bot.

  -  Detailed protocol output is still kept in protocol-only-child.log.


