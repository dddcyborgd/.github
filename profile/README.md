# dddcyborgd — the three-d cyborg daemon

**cyborg** is the DeltaVerse participant-experience environment: a persistent 3D space where a participant's
NFTs, iNFT agents and THOT memory are the furniture, other participants are present, the substrate library is
the sky, and a signed login rung decides what may be touched. **Every participant is both a client and a
server** — the triad: participant ⟷ the DeltaVerse anchor ⟷ participant.

Versions **dvengine 0.0.2-alpha · cyborgd 0.0.1-alpha · cyborg-contracts 0.0.1-alpha** · MIT · refined from the [oncyberio](https://github.com/oncyberio) engine (MIT) and
credited throughout · consumed by the [DeltaVerse](https://github.com/AgenticPlace/DeltaVerse) as suite E13.

## The three repos

| repo | what it is | run it |
|---|---|---|
| [**dvengine**](https://github.com/dddcyborgd/dvengine) | the client engine on three.js: `DVEngine` (component runtime — the 37 awe component ports + DeltaVerse-native `substrate` · `portal` · `zone` · `piece` · **`aivatar`** · `thot-memory` · `participant`), `DVScene` (the *cyborg-space/1* document — the scene IS the tokenURI), `DVTransform` (the transform tools), the glTF asset desk, `DVStudio`, `DVNet` / `DVPeer` / `DVHost` (the triad client), `DVXR`, `DVVerse`, **the field of influence** (`DVField`: resizable to the space extent − 1 — *infinity − 1* — always recognised by the DeltaVerse, the hierarchy's two dials, open · connected · private, with the sceptre and the orb as items), the **input lane** (old-school joystick layouts E S D F · I J K L ; , with K the core button of combinations · arrows; the mouse as an arcball extension; the mic and the camera as sticks; chords), and **legacy mode** (a faithful reflection of the oncyber work) | `git clone https://github.com/dddcyborgd/dvengine && cd dvengine && node scripts/vendor-three.mjs && node scripts/serve.mjs` → http://127.0.0.1:8801/ |
| [**cyborgd**](https://github.com/dddcyborgd/cyborgd) | the daemon — the DeltaVerse **anchor** of the triad: rendezvous + identity (login333 claims) + the canonical snapshot + failover host, the `cyborg/1` protocol, rooms · zones · authoritative sim, the **aivatar** behaviours (greet · mirror · arcball arm-reach · the riddle), and **the faucet** (openBDK / LUV / SCIEN·TIFIC drips against EIP-712 vouchers), plus the **field policy** — per-rung `outflow`/`inflow` dials the OVERSEER edits (`/field-policy`). Isomorphic core: the same room code runs in a participant's browser as a host. Zero npm dependencies. | `git clone https://github.com/dddcyborgd/cyborgd && cd cyborgd && node daemon/cyborgd.mjs` → ws://127.0.0.1:8790/ws · `node daemon/cyborgd.mjs --selftest` |
| [**cyborg-contracts**](https://github.com/dddcyborgd/cyborg-contracts) | Foundry: `CyborgSpace` (ERC-721 space token, ERC-6551-ready), `CyborgDrop` + `CyborgMultiSender` (the oncyber factory drops, immutable), `CyborgFaucet` (signer-gated pull faucet) — all deploy → configure → renounce; plus `CyborgSpaceUpgradeable`, the **OVERSEER-owned** UUPS lane with a public implementation lineage. **Every address is _predicted_ (create3d, vanity salts mined by the DeltaVerse saltgrinder: `0x5ace…`, `0xd0D0…`, `0xfa0C…`) — nothing is deployed or explorer-verified yet, and the README says so next to each row** | `git clone https://github.com/dddcyborgd/cyborg-contracts && cd cyborg-contracts && forge test` |

## How the pieces fit
```
 participant A (host) ⇄ RTCDataChannel ⇄ participant B (client)      dvengine: DVVerse + DVNet/DVPeer/DVHost
        ⇅ mirror{tick,snap}                     ⇅ hello · claim · voucher
              cyborgd — the anchor (identity · vouchers · canonical snapshot · failover host)
                                  ⇅ EIP-712 vouchers
              cyborg-contracts — CyborgFaucet.drip · CyborgSpace.mintSpace · CyborgDrop.dropMint
```

## Documentation
- Each repo's `README.md` is the complete manual (usage, protocol, registries, production deployment).
- `LIBRARY.md` + `registry.json` in each repo catalogue every upstream file with a port verdict; `UPSTREAM.md` is the provenance table; `archive/SOURCES.json` pins every vendored sha.
- The operator skill: [`cyborgd/skills/cyborgd/SKILL.md`](https://github.com/dddcyborgd/cyborgd/blob/main/skills/cyborgd/SKILL.md).
- In the DeltaVerse: `docs/CYBORG.md`, `deploy/cyborg/`, `cyborg/`.

## Inspiration and credit
[oncyber.io](https://oncyber.io) · [docs.oncyber.io](https://docs.oncyber.io) · [github.com/oncyberio](https://github.com/oncyberio):
[factory](https://github.com/oncyberio/factory) · [engine](https://github.com/oncyberio/engine) · [awe](https://github.com/oncyberio/awe) ·
[gltftransform](https://github.com/oncyberio/gltftransform) (fork of [glTF-Transform](https://github.com/donmccurdy/glTF-Transform) by Don McCurdy) ·
[oo-game-server-starter](https://github.com/oncyberio/oo-game-server-starter) · [game-server-v2](https://github.com/oncyberio/game-server-v2) ·
[onchain-cc-server](https://github.com/oncyberio/onchain-cc-server). Patterns: [Colyseus](https://colyseus.io) rooms · [three.js](https://threejs.org)
([ArcballControls](https://threejs.org/examples/#misc_controls_arcball)) · AIML by Dr. Richard S. Wallace (A.L.I.C.E., 1999) ·
[ERC-6551](https://eips.ethereum.org/EIPS/eip-6551) · [ERC-7857](https://eips.ethereum.org/EIPS/eip-7857) · [EIP-712](https://eips.ethereum.org/EIPS/eip-712).

Clean-room policy: upstream code is taken in and adapted as a local, self-contained, attributed version — no CDN,
no runtime package fetch. (c) 2026 BANKON / PYTHAI · Professor Codephreak.
