# Internet Noise FM

A live Internet radio station generated from real unsolicited TCP traffic.

When the Internet is quiet, the station settles into an evolving carrier tone: a soft drone with slow pitch drift, phase movement, faint static, and other subtle variation. When scanners, bots, probes, and other background Internet noise hit participating tcpnoise nodes, those events become sound.

The goal is not to turn every packet into an obnoxious beep. Internet Noise FM should stay listenable while still reflecting what the public Internet is actually doing.

## Core idea

- One continuous shared radio station.
- Multiple tcpnoise nodes feed live connection events into it.
- Each connection becomes a sound chosen from a coherent palette.
- Node, port, protocol, and other event metadata may influence the sound.
- Quiet periods return to an evolving carrier rather than silence.
- Bursts should become controlled musical/noise texture instead of clipping chaos.

## Interactive ports

The station website should show participating nodes and the TCP ports they are currently listening on.

Listeners can choose a node and request an additional port. If the request is allowed and the port is available, that node begins listening on it.

Requested ports are separate from operator-configured permanent listeners.

Each node has a configurable maximum number of requested ports. When the pool is full, requesting another port evicts the oldest requested listener. Permanent listeners are never evicted by the radio UI.

A hit on a listener-requested port should get special visual and audio treatment so the person who threw a new fishing line into the Internet can hear when something bites.

Requested ports may optionally let the listener choose a sound family, with a default `Surprise me` option.

## Nodes

The UI should show useful node information such as:

- name
- rough location
- online/offline state
- permanent listening ports
- listener-requested ports
- activity/hit counts

Offline nodes remain visible but cannot accept new port requests.

Longer term, new nodes could be funded by donations or operated by other people as public Internet Noise FM nodes.

## Public event information

The public UI should expose as much useful connection context as is reasonable without blindly reflecting attacker-controlled data.

Initial information may include:

- node
- destination port
- protocol guess
- partially masked source address
- requested-port indicator

Possible later additions include source ASN/country, HTTP User-Agent, and carefully sanitized excerpts or classifications from payloads.

For public display, source addresses should be truncated rather than replaced with fake private addresses. For example, an IPv4 address may be shown with the final octet masked, and an IPv6 address may be reduced to its network prefix. Full values may remain available to the operator where appropriate.

## Sound design

The audio should have enough variation to remain interesting without sounding like a novelty soundboard or circus.

The working direction is:

- a persistent evolving carrier underneath quiet periods
- a limited set of coherent sound families
- several variants inside each family
- small controlled differences in pitch, timbre, gain, timing, or stereo position
- node/port/protocol metadata influencing selection
- bounded polyphony and burst handling
- a recognizable accent for requested-port hits

Exact sound mapping is intentionally not frozen yet.

## Streaming

The intended experience is a real continuous Internet radio stream generated centrally so everyone hears the same station at roughly the same point in time. Icecast or a similar streaming setup is a likely direction, but the transport is not yet selected.

## Status

Early design and planning.

See:

- [`DESIGN.md`](DESIGN.md) for current behavior and audio design decisions.
- [`PLAN.md`](PLAN.md) for implementation milestones.
- [`AGENTS.md`](AGENTS.md) for implementation guardrails.
