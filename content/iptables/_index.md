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