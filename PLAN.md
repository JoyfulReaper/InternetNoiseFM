# PLAN.md

# Internet Noise FM plan

## Milestone 0 — Design

- Capture station concept and sound goals.
- Define one-station architecture.
- Define node and requested-port behavior.
- Define public metadata/privacy defaults.
- Keep unresolved implementation choices explicitly unresolved.

## Milestone 1 — Carrier + synthetic events

Build the smallest audio prototype with no live network dependency.

- Generate the evolving idle carrier.
- Define a small coherent initial sound palette.
- Feed fake connection events into the audio engine.
- Verify events interrupt/layer over the carrier cleanly.
- Add bounded polyphony.
- Simulate quiet periods, normal traffic, and large bursts.

Success means it can run for a while without becoming boring, obnoxious, or unstable.

## Milestone 2 — One real tcpnoise node

Connect one existing tcpnoise sensor.

- Consume live tcpnoise connection events.
- Map events into the audio engine.
- Include node and destination-port context.
- Show basic current-event information.
- Handle reconnects without killing the station.

Do not add multi-node orchestration yet.

## Milestone 3 — Real radio stream

Turn the generated audio into a real shared stream.

- Select codec/bitrate.
- Select streaming server/protocol, likely Icecast or similar.
- Encode continuous audio.
- Make browser playback reliable.
- Verify multiple listeners hear the same station.
- Keep carrier running through quiet periods and event-feed reconnects.

## Milestone 4 — Multiple nodes

Add multiple tcpnoise sensors.

- Node registry/configuration.
- Online/offline state.
- Rough node location/display information.
- Per-node event identity.
- UI node list.
- Per-node listening-port display.
- Audio variation influenced by node without creating a cacophony.

## Milestone 5 — Requested ports

Implement interactive port requests.

- Choose node.
- Enter TCP port.
- Optional sound-family selection.
- `Surprise me` default.
- Validate request.
- Start listener through narrow node-control API.
- Show requested port in UI.
- Track request order.
- Enforce `MaxRequestedPorts`.
- Evict oldest requested port when full.
- Never evict permanent listeners.
- Disable requests for offline nodes.
- Add cooldown/rate limiting.
- Add recognizable requested-port-hit audio and UI treatment.

Initial low-port policy can be conservative and configurable.

## Milestone 6 — Richer station information

Add useful context carefully.

Possible additions:

- hit counts
- events/minute
- top ports
- protocol detection
- masked public source IP
- source ASN/country
- HTTP User-Agent
- safe payload classification/excerpts
- current/last requested-port hit
- rotating stream metadata

Do not blindly display attacker-controlled strings.

## Milestone 7 — Audio polish

Expand the sound vocabulary while preserving a coherent station identity.

- More sound families and variants.
- Tune carrier variation.
- Tune node identity.
- Tune requested-port accent.
- Better burst behavior.
- Dynamic mixing/ducking.
- Long-duration listening tests.

The test is not “does every event make a sound?” It is “does this remain interesting to listen to?”

## Milestone 8 — Public/community nodes

Only after the core system works:

- authenticated third-party node registration
- capability/version negotiation
- operator-defined request-port policy
- node health
- contributor/sponsor attribution
- documentation for running a public node
- donation/sponsorship path for new VPS locations

## First implementation experiment

Start with audio, not orchestration.

Build a local process that:

1. Produces the evolving carrier continuously.
2. Accepts synthetic connection events.
3. Chooses sounds from several coherent families.
4. Layers events over the carrier.
5. Limits simultaneous sounds.
6. Survives a simulated high-rate burst.
7. Returns naturally to the carrier afterward.

If that does not sound good, no amount of distributed-node architecture will make the station worthwhile.
