+++

title = "Advanced network configuration with `iptables`"
description = "Practical introduction to `iptables` and its functioning"
outputs = ["Reveal"]

[reveal_hugo.custom_theme_options]
targetPath = "css/custom-theme.css"
enableSourceMap = true

+++

# Advanced network configuration <br> with `iptables`

Giovanni Ciatto

---

## References

- [Illustrated introduction to Linux `iptables`](https://iximiuz.com/en/posts/laymans-iptables-101/)
- [`iptables` Processing Flowchart (Updated Often)](https://stuffphilwrites.com/2014/09/iptables-processing-flowchart/)
- Also the English [Wikipedia page on Netfilter](https://en.wikipedia.org/wiki/Netfilter) is quite informative

---

## Network packets stepping through the Linux OS

{{< image src="./simplified-flow-1.png" width="100%" max-h="60vh" >}}

1. __Ingoing__ packets, coming _from_ the Internet, are received by _some_ local _network interface_ (e.g., `eth0`)
    1. these may be __directed__ towards some _local process_ willing to __receive__ them
    2. ... or towards __another machine__ in the _local_ network, via another local network interface (e.g., `eth1`)

2. __Outgoing__ packets are sent by _some_ local _network interface_ (e.g., `eth1`) _towards_ the Internet
    1. due to either some _local process_ __sending__ them...
    2. ... or some __forwarding__

---

## What's forwarding?

{{< image src="./routing.svg" width="100%" max-h="60vh" >}}

- While __sending__ and __receiving__ packets is more common for _end-hosts_ (e.g. clients or servers)...
- ... __forwarding__ is more common _infrastructural_ hosts (e.g. routers, load balancers, etc.)

---

## The need for customization (pt. 1)

{{< image src="./simplified-flow-2.png" width="100%" max-h="60vh" >}}

#### One may want to __alter__ the _default_ behaviour of the Linux OS in handling packets, e.g. to perform:

- __firewalling__, e.g. _blocking_ packets from/to some IP addresses, or ports for _security_ reasons
- __translating network addresses__, e.g. _translating_ packets' source/destination IP addresses or ports, e.g. for the sake of _access control_
    * a.k.a. *NAT*ting, _masquerading_
- __load balancing__, e.g. _distributing_ packets among multiple servers
- __traffic shaping__, e.g. _prioritizing_ packets based on their content, or _limiting_ their bandwidth

---

## The need for customization (pt. 2)

{{< image src="./simplified-flow-2.png" width="100%" max-h="60vh" >}}

#### This implies __attaching__ some _custom_ logic to specific _points_ in the _packet processing_ flow, e.g.:

- __firewalling__: may be achieved by _dropping_ packets in the `INPUT` phase
- __NATting__: may be achieved by _modifying_ packets in the `POSTROUTING` phase
- __load balancing__: may be achieved by _modifying_ packets in the `FORWARD` phase
- __traffic shaping__: may be achieved by _marking_ packets in the `PREROUTING` or `INPUT` phase 

---

## Netfilter and `iptables`

[Netfilter](https://en.wikipedia.org/wiki/Netfilter) is the _framework_ in the Linux kernel responsible for _handling packets_

{{< image src="https://upload.wikimedia.org/wikipedia/commons/d/dd/Netfilter-components.svg" width="100%" max-h="60vh" >}}

- it allows for attaching _custom_ logic to specific _points_ in the _packet processing_ flow
- you can do so, via _high-level_ user-space tools, such as `iptables`

---

## Important aspects of Netfilter/`iptables` (pt. 1)

1. __Chain__: a _list_ of _rules_ to be _applied_ to a packet to _determine_ its _fate_, with _decreasing_ priority
    + they are usually named after the _phase_ in the _packet processing_ flow they are attached to 
        + e.g. `INPUT`, `FORWARD`, `OUTPUT`, `PREROUTING`, `POSTROUTING`

2. __Rule__ (of a chain): a _decision_ on what to do with a packet _matching_ a given _criterion_

3. __Matching criteria__ (of a rule): are _conditions_ a packet must _satisfy_ to be _matched_ by a rule
    + they could be like `--source`, `--destination`, `--protocol`, `--sport`, `--dport`, etc.

3. __Target__ (of a rule): rules' decisions are called "targets"
    + they could be like `ACCEPT`, `DROP`, `LOG`, `RETURN` to some prior chain, or _jump_ to another chain

4. __Policy__ (of a chain): the _default_ action to be taken if no rule in the _matches_ a packet

---

## Important aspects of Netfilter/`iptables` (pt. 2)

> __Suggestion__: think about
> - chains as _functions_ accepting packets as _arguments_, and returning a _decision_
> - rules as _if-then-else_ statements
> - policies as the _else_ part of an _if-then-else_ statement

{{< image src="./chains.png" width="100%" max-h="60vh" >}}

---

## Important aspects of Netfilter/`iptables` (pt. 3)

5. __Tables__: predefined _groups_ of _chains_ with a specific _purpose_
    + `filter`: contains chains for _filtering_ packets
    + `nat`: contains chains for _translating_ packets' addresses/ports
    + `mangle`: contains chains for _modifying_ packets
    + `raw`: contains chains for low-level _packet processing_
    + `security`: contains chains for _SELinux_ (SE $\equiv$ Security-Enhanced) rules

---

## Default chains, per table

| Table        | Chains                                                    |
|-------------:|:----------------------------------------------------------|
| **filter**   | `INPUT`, `OUTPUT`, `FORWARD`                              |
| **nat**      | `PREROUTING`, `POSTROUTING`, `OUTPUT`                     |
| **mangle**   | `PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING` |
| **raw**      | `PREROUTING`, `OUTPUT`                                    |
| **security** | `INPUT`, `OUTPUT`, `FORWARD`                              |

<br>

- `INPUT`: handles packets _directed to_ the local system
- `OUTPUT`: handles packets _originating from_ the local system
- `FORWARD`: handles packets _routed through_ the system
- `PREROUTING`: handles packets _before_ they are routed
- `POSTROUTING`: handles packets _after_ they are routed

---

## Default flow (pt. 1)

### Overview

{{< image src="./linux-processing.webp" width="100%" max-h="80vh" >}}

think about chains as _hooks_ to which you can _attach_ your _custom_ logic

---

## Default flow (pt. 2)

### Input and output flows

{{< image src="./flow-node.png" width="100%" max-h="80vh" >}}

---

## Default flow (pt. 3)

### Input and output flows

{{< image src="./flow-router.png" width="100%" max-h="80vh" >}}

---

## Admissible targets, per table (pt. 1)

| Table        | Target            |
|-------------:|:------------------|
| **filter**   | `ACCEPT`\*, `DROP`\*, `REJECT`\*, `LOG`, `RETURN`, `ULOG` |
| **nat**      | `DNAT`, `SNAT`, `MASQUERADE`, `REDIRECT`, `RETURN`  |
| **mangle**   | `MARK`, `TOS`, `DSCP`, `TTL`, `SECMARK`, `CONNMARK`, `RETURN` |
| **raw**      | `NOTRACK`, `RETURN` |
| **security** | `SECMARK`, `RETURN` |

\* interrupts the _processing_ of the _current_ packet

---

## Admissible targets, per table (pt. 2)

{{% multicol %}}
{{% col %}}
- `ACCEPT`: _accept_ the packet
- `CONNMARK`: _enables_ a packet for __connection tracking__
- `DNAT`: changes the _destination_ address of a packet
- `DROP`: _drop_ the packet
- `DSCP`: sets the Differentiated Services Code Point (_DSCP_) header of the packet
- `LOG`\*\*: logs the packet details to the _system log_ 
- `MARK`\*\*: marks the packet for further processing (e.g., for QoS)
- `MASQUERADE`: dynamically modifies the source IP address/port based on the outgoing interface's IP (used for dynamic IPs)
{{% /col %}}
{{% col %}}
- `NOTRACK`: _disables_ connection tracking for a packet
- `REDIRECT`: _redirects_ the packet to a different _port_ on the _same machine_
- `REJECT`: _drops_ the packet and sends an _error response_ back
- `RETURN`: jump back to the _calling_ chain
- `SECMARK`: _marks_ packets for use with _security_ modules 
- `SNAT`: changes the _source_ address of a packet
- `TOS`: sets the _Type of Service_ (_TOS_) header of the packet
- `TTL`: sets the _Time to Live_ (_TTL_) header of the packet
- `ULOG`: logs the packet details to _userspace_ 
{{% /col %}}
{{% /multicol %}}

---

# Use cases

---

## Use case: Firewalling at the service level (pt. 1)

- __Goal__: block _all_ ingoing packets, except the ones directed towards a specific _$port$_
- __Assumption__: the _$port$_ corresponds to a _service_ you want to _expose_ to the Internet
- __Issue 1__: ingoing packets for connections _initiated_ from the local system should _not_ be blocked
- __Issue 2__: the loop-back interface should _not_ be blocked

{{% fragment %}}
### Solution

1. Set `REJECT` as the default _policy_ for the `INPUT` and `FORWARD` chains of the `filter` table
2. Set `ACCEPT` as the _policy_ for the `OUTPUT` chain of the `filter` table
3. Add a rule to the `INPUT` chain of the `filter` table, _matching_ packets directed towards the _$port$_, and `ACCEPT`ing them
4. Add a rule to the `INPUT` chain of the `filter` table, _matching_ packets for _established connections_ and `ACCEPT`ing them
5. Add a rule to the `INPUT` chain of the `filter` table, _matching_ packets for the _loop-back_ interface and `ACCEPT`ing them
{{% /fragment %}}

---

## Use case: Firewalling at the service level (pt. 2)

### Code Example (for SSH on port 22)

```bash
# Flush existing rules (optional, to start fresh)
sudo iptables -t filter -F

# Set default policies to DROP for the INPUT and FORWARD chains, and ACCEPT for OUTPUT
sudo iptables -t filter -P INPUT DROP
sudo iptables -t filter -P FORWARD DROP
sudo iptables -t filter -P OUTPUT ACCEPT

# Allow packets for established and related connections
sudo iptables -t filter -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow incoming SSH traffic (port 22)
sudo iptables -t filter -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow loopback traffic
sudo iptables -t filter -A INPUT -i lo -j ACCEPT
```
