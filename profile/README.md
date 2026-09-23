<p align="center"><img src="https://raw.githubusercontent.com/dddcyborgd/.github/main/profile/assets/cyborg.svg" alt="dddcyborgd — the three-d cyborg daemon" width="640"></p>

<p align="center">
<a href="https://github.com/dddcyborgd/dvengine"><img alt="dvengine" src="https://img.shields.io/badge/dvengine-0.0.2--alpha-9fe9ff?logo=three.js&logoColor=black"></a>
<a href="https://github.com/dddcyborgd/cyborgd"><img alt="cyborgd" src="https://img.shields.io/badge/cyborgd-0.0.1--alpha-3b6cff?logo=node.js&logoColor=white"></a>
<a href="https://github.com/dddcyborgd/cyborg-contracts"><img alt="cyborg-contracts" src="https://img.shields.io/badge/cyborg--contracts-0.0.1--alpha-f4d47c?logo=solidity&logoColor=black"></a>
<a href="LICENSE"><img alt="MIT" src="https://img.shields.io/badge/license-MIT-blue"></a>
<img alt="tests" src="https://img.shields.io/badge/tests-92%20%C2%B7%2063%20%C2%B7%2045-brightgreen">
<img alt="dependencies" src="https://img.shields.io/badge/runtime%20deps-zero-black">
<img alt="deployment" src="https://img.shields.io/badge/mainnet-not%20deployed-lightgrey">
</p>

# The three-d cyborg daemon

**cyborg** is the DeltaVerse participant-experience environment: a persistent 3D space where a participant's NFTs,
iNFT agents and THOT memory are the furniture, other participants are present, the substrate library is the sky,
and a signed login rung decides what may be touched. It is refined from the [oncyberio](https://github.com/oncyberio)
engine (MIT), credited throughout, and kept alongside a faithful **legacy mode** of that work.

Three ideas hold it together:

| | |
|---|---|
| **The triad** | Every participant is **both a client and a server**. A participant *hosts* their own space to peers over WebRTC by running the same room core in the browser; the DeltaVerse *anchor* (cyborgd) keeps identity, vouchers, the canonical snapshot and fails over as host; anyone else is a *client*. No single point of authority for presence. |
| **The field of influence** | The arcball around a participant becomes a **resizable field**: items of influence (the sceptre, the orb) ride on its surface, the aivatar's arm reaches along it, combinations act on it. It can grow toward the whole space but stays **one unit short of it — infinity − 1** — so it remains a thing the DeltaVerse can recognise. The DeltaVerse **always recognises** it; the OVERLORD hierarchy holds two dials per rung (**outflow**: how much the DeltaVerse is affected, **inflow**: how much the participant is), and a field is open, connected to chosen participants, or private to its signer. |
| **Privilege is earned, custody is not shared** | login333 is the gate; rungs come from signature, holdings and appointment. Value contracts are immutable (deploy → configure → renounce to zero); the one upgradeable contract is owned by an appointed, revocable OVERSEER with a public lineage. The faucet signs vouchers; the participant redeems from their own wallet. |

## The three repositories

| repo | what it is | quick start |
|---|---|---|
| [**dvengine**](https://github.com/dddcyborgd/dvengine) | The client engine on three.js: `DVEngine` (component runtime — the 37 awe ports + DeltaVerse-native `substrate` · `portal` · `zone` · `piece` · **`aivatar`** · `thot-memory` · `participant` · `sceptre` · `orb`), `DVScene` (the *cyborg-space/1* document — the scene IS the tokenURI), `DVTransform` (transform tools), the glTF asset desk, `DVStudio`, `DVNet` / `DVPeer` / `DVHost` (the triad client), `DVXR`, `DVVerse`, **`DVField`** (the field of influence), the **input lane** (old-school joystick layouts, the mouse as an arcball extension, the mic and the camera as sticks, chords) and **legacy mode**. | `git clone https://github.com/dddcyborgd/dvengine && cd dvengine && node scripts/vendor-three.mjs && node scripts/serve.mjs` → http://127.0.0.1:8801/ (`/studio/studio.html`, `/legacy/legacy.html`) · `node --test test/*.test.mjs` |
| [**cyborgd**](https://github.com/dddcyborgd/cyborgd) | The daemon: the DeltaVerse **anchor** of the triad — rendezvous, login333 claims, the canonical snapshot, failover host — the `cyborg/1` protocol with WebRTC signaling, rooms · zones · authoritative sim, the **aivatar** behaviours (greet · mirror · arcball arm-reach · the riddle · answering raised items), **the faucet** (openBDK / LUV / SCIEN·TIFIC drips against EIP-712 vouchers) and the **field policy** dials. Isomorphic core: the same room code runs in a participant's browser as host. Zero npm dependencies. | `git clone https://github.com/dddcyborgd/cyborgd && cd cyborgd && node daemon/cyborgd.mjs` → ws://127.0.0.1:8790/ws · `node daemon/cyborgd.mjs --selftest` · production: `ops/README.md` |
| [**cyborg-contracts**](https://github.com/dddcyborgd/cyborg-contracts) | Foundry: `CyborgSpace` (ERC-721 space token, ERC-6551-ready), `CyborgDrop` + `CyborgMultiSender` (the oncyber factory drops, immutable), `CyborgFaucet` (signer-gated pull faucet) — deploy → configure → renounce; and `CyborgSpaceUpgradeable`, the OVERSEER-owned UUPS lane. **Every address is predicted; nothing is deployed or explorer-verified yet.** | `git clone https://github.com/dddcyborgd/cyborg-contracts && cd cyborg-contracts && forge test` |

## The input map (defaults, all customisable)

```
LEFT STICK — movement                   RIGHT STICK — the field of influence    ARROWS — joystick emulation
        [E]                                     [I]                                     [↑]
  [S] ◄  ●  ► [F]                         [J] ◄ (K) ► [L]                         [←] ◄  ●  ► [→]
        [D]                                     [,]                                     [↓]
fire [Space]  side [A] [C]              core [K] — combinations only · side [;]  fire [RCtrl]  side [RShift] [Num0]
```
The mouse extends onto the ball (the [three.js ArcballControls](https://threejs.org/examples/#misc_controls_arcball)
model: position steers, pointer-lock velocity turns the sphere, wheel raises/lowers), the mic and the camera feed the
same sticks (head yaw/pitch → the right stick, lean → forward, a nod → the core button, an onset → fire), and chords
such as `core+up`, `core,core`, `fire:hold` become actions. `DVControls.describe()` prints the full map.

## How the pieces fit

```
 participant A (host) ⇄ RTCDataChannel ⇄ participant B (client)      dvengine: DVVerse · DVField · DVNet/DVPeer/DVHost
        ⇅ mirror{tick,snap}                     ⇅ hello · claim · field · item · voucher
              cyborgd — the anchor (identity · vouchers · canonical snapshot · field policy · failover host)
                                  ⇅ EIP-712 vouchers
              cyborg-contracts — CyborgFaucet.drip · CyborgSpace.mintSpace · CyborgDrop.dropMint
                                  ⇅ suite E13 (create3d) · deploy/cyborg · docs/CYBORG.md · docs/DVAAS.md
              the DeltaVerse — the consumer (cyborg/ · pages/dvaas.html · pages/allchain.html · login333)
```

## Status — what is real today

| claim | state |
|---|---|
| Engine, daemon and contracts run locally with their test suites green (92 · 63 · 45) | **verified** — run the commands above |
| The daemon recognises a field at its bound and answers a raised sceptre over a live WebSocket | **verified** locally |
| The E13 suite rehearses on anvil: 15 txids, `owner()==0` on the three immutable contracts, replay refused, the OVERSEER proxy initialised | **verified** locally (DeltaVerse `npm run deploy:cyborg`) |
| Contract addresses `0x5ace…595e` (Space) · `0xd0D0…93cc` (Drop) · `0xfa0C…f199` (Faucet) | **predicted** by the create3d formula from vanity salts — not deployed, not verified |
| Public deployment, explorer verification, external audit | **not done** — they wait for the OVERLORD's signature and a review |

We publish only what can be checked; every address in these repositories carries its state next to it.

## Documentation

- Each repository's `README.md` is its complete manual — usage, the protocol, the registries, production deployment.
- `LIBRARY.md` + `registry.json` catalogue every vendored upstream file with a port verdict; `UPSTREAM.md` is the provenance table; `archive/SOURCES.json` pins every vendored sha.
- The operator skill: [`cyborgd/skills/cyborgd/SKILL.md`](https://github.com/dddcyborgd/cyborgd/blob/main/skills/cyborgd/SKILL.md).
- In the DeltaVerse: `docs/CYBORG.md`, `docs/DVAAS.md` (DeltaVerse as a Service), `deploy/cyborg/` (the ceremony and the audit posture), `cyborg/` (the consumer surface).

## Related homes

The organisations this work descends from and feeds — each a piece of the same estate:

| home | what it holds | how cyborg relates |
|---|---|---|
| [**deltav-deltaverse**](https://github.com/deltav-deltaverse) | the decentralised cryptoverse metaDAO — NeuralNode, the BubbleRoom lineage, the co-canonical DeltaVerse upstream (126 repos) | `CyborgSpace` links a `BubbleRoomV4` room; the rooms are the zones the aivatars live in |
| [**cypherpunk4096**](https://github.com/cypherpunk4096) | 2^3 == 8 — the successor posture to cypherpunk2048 (CP2048-OVL-1: the OVERLORD / shadow-OVERSEER standard) | the custody doctrine here: immutable value contracts, an appointed and revocable OVERSEER, every upgrade public |
| [**DAONOW**](https://github.com/DAONOW) | a collection of inception contracts for DAO creation (151 repos) | the DAIO devolution target: the governance the cyborg suite hands off to |
| [**poormanvpn**](https://github.com/poormanvpn) | poor man's VPN client + server code | the sovereign network posture the triad assumes — a participant's host can sit behind their own tunnel; the anchor stays reachable |
| [**AgenticPlace / DeltaVerse**](https://github.com/AgenticPlace/DeltaVerse) | the DeltaVerse itself — the consumer of these three repositories (suite E13, `cyborg/`, `pages/dvaas.html`) | where the gathered lanes are served and where the OVERLORD signs |

## Inspiration and credit

[oncyber.io](https://oncyber.io) · [docs.oncyber.io](https://docs.oncyber.io) · [github.com/oncyberio](https://github.com/oncyberio):
[factory](https://github.com/oncyberio/factory) · [engine](https://github.com/oncyberio/engine) · [awe](https://github.com/oncyberio/awe) ·
[gltftransform](https://github.com/oncyberio/gltftransform) (fork of [glTF-Transform](https://github.com/donmccurdy/glTF-Transform) by Don McCurdy) ·
[oo-game-server-starter](https://github.com/oncyberio/oo-game-server-starter) · [game-server-v2](https://github.com/oncyberio/game-server-v2) ·
[onchain-cc-server](https://github.com/oncyberio/onchain-cc-server). Patterns: [Colyseus](https://colyseus.io) rooms · [three.js](https://threejs.org)
([ArcballControls](https://threejs.org/examples/#misc_controls_arcball)) · AIML by Dr. Richard S. Wallace (A.L.I.C.E., 1999) ·
[ERC-6551](https://eips.ethereum.org/EIPS/eip-6551) · [ERC-7857](https://eips.ethereum.org/EIPS/eip-7857) · [EIP-712](https://eips.ethereum.org/EIPS/eip-712).

Clean-room policy: upstream code is taken in and adapted as a local, self-contained, attributed version — no CDN, no runtime
package fetch. Licensed MIT. (c) 2026 BANKON / PYTHAI · Professor Codephreak.
