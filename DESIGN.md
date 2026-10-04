# DESIGN.md

# Internet Noise FM design

## Station model

Internet Noise FM is one shared continuous station.

All participating tcpnoise nodes feed events into the same audio generator. The website may let listeners inspect individual nodes, but the primary listening experience is one combined stream.

The audio is generated centrally rather than independently in each browser. Everyone should hear the same station.

## Idle carrier

Silence is not the idle state.

When no connection events are arriving, the station plays a soft evolving carrier/drone.

The carrier should change slowly enough to stay alive without becoming distracting. Possible modulation includes:

- slight pitch drift
- slow phase movement
- very faint static/noise
- gentle filter movement
- occasional quiet sweeps or tonal shifts

The carrier should remain recognizable so the station audibly settles back into its idle state after activity.

## Event sound palette

Connection events need substantially more variety than the current desktop tray application.

Do not assign one fixed sample to every port.

Instead, use a limited number of coherent sound families, for example:

- clicks / relays
- knocks
- short synth plucks
- modem-like chirps
- static bursts
- terminal-style tones
- metallic ticks
- soft bells or resonant pings

Each family should contain multiple variants.

Selection may be influenced by:

- node
- destination port
- protocol guess
- whether the port was listener-requested
- controlled randomness

Small variations in pitch, gain, timbre, timing, and stereo position can keep repeated events interesting.

The target is variation without chaos. Avoid turning the stream into a novelty soundboard.

## Burst handling

A large scan or botnet burst must not cause unlimited overlapping sounds.

The audio engine should use bounded polyphony and burst handling.

Possible approaches include:

- per-time-window event limits
- event coalescing
- shorter or quieter sounds during bursts
- a special dense-burst texture
- compression/limiting
- preserving representative events rather than literally sounding every connection

The sound should become more intense when traffic increases, but remain listenable and stable.

## Requested ports

The website displays nodes and allows a listener to request a TCP port on a chosen node.

Requested listeners are separate from permanent/operator-configured listeners.

Per node:

- `MaxRequestedPorts` is configurable.
- A requested port remains active until explicitly removed, the node restarts, or it is evicted.
- When the pool is full, a new request evicts the oldest requested port.
- Permanent listeners are never evicted by this mechanism.
- A node may define allowed/denied port ranges.
- Low/privileged ports are a policy decision and may initially be disabled.

A requested-port hit should get a recognizable audio accent and visible UI treatment.

When requesting a port, the listener may be allowed to choose a sound family, with `Surprise me` as the default. The family controls the general character, while individual hits still vary within that family.

## Node model

A node should expose enough state for the central service to display:

- node ID
- display name
- rough location
- online/offline state
- permanent listening ports
- requested listening ports
- request-slot capacity
- activity/hit counts
- capabilities/policy relevant to port requests

Offline nodes remain visible but requests are disabled.

For the MVP, requested listeners do not need to survive a node restart. The UI can simply reflect the state reported by the node after reconnect.

## Node control

The central service may ask a node to start or stop a TCP listener and, where required, arrange firewall access.

This control path must be narrow and explicit.

A node control agent should accept operations such as:

- list active listeners
- start requested listener on validated TCP port
- stop requested listener
- report health/state

It must not expose arbitrary remote shell execution.

Node-specific policy always wins. A public/community-operated node must be able to choose:

- whether port requests are enabled
- maximum requested ports
- allowed/denied port ranges
- whether privileged ports are allowed
- request rate limits or other local limits

## Anonymous interaction

Port requests are intended to be available without user accounts.

Abuse controls should include some combination of:

- per-client cooldown
- rate limiting
- per-node global request rate
- port validation
- denied-port rules
- requested-port pool size
- optional CAPTCHA/challenge only if abuse actually requires it

Do not add accounts merely to solve hypothetical abuse.

## Public connection metadata

The public interface should expose useful event context where safe.

Initial fields may include:

- node
- destination port
- protocol guess
- requested-port status
- partially masked source address

Do not disguise public addresses as private addresses.

Suggested public masking:

- IPv4: preserve the network portion and mask the final octet, e.g. `203.0.113.x`
- IPv6: display a truncated prefix, e.g. `2001:db8:abcd:1234::/64`

The operator/admin side may retain fuller event information.

Possible later public metadata:

- source ASN
- source country
- HTTP User-Agent
- payload classification
- carefully sanitized payload excerpts

Attacker-controlled strings must be sanitized and length-limited before public display or station metadata.

## Streaming

The target is a real Internet-radio-style continuous stream.

Likely shape:

tcpnoise nodes -> event ingest -> audio engine -> encoder -> streaming server -> listeners

Icecast is a likely candidate, but the exact streaming/encoding stack is not decided.

The web UI should be a station interface around the shared stream, not a separate per-browser synthesis engine.

## Community/public nodes

Longer term, Internet Noise FM may support nodes funded through donations or operated by other people.

Possible model:

- donate toward a VPS/node
- sponsor a location
- run a public node and connect it to the station

This is not required for the MVP.

Before third-party nodes are accepted, define:

- node authentication
- event trust boundaries
- version/capability negotiation
- operator-controlled port-request policy
- abuse handling
- how node identity/location is verified or presented
