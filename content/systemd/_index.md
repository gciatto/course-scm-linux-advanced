+++

title = "Systemd"
description = "Practical introduction to systemd"
outputs = ["Reveal"]

[reveal_hugo.custom_theme_options]
targetPath = "css/custom-theme.css"
enableSourceMap = true

+++

# Systemd

Giovanni Ciatto

---

## References

- [`systemd` on ArchLinux Wiki](https://wiki.archlinux.org/title/Systemd)
- [DigitalOcean's tutorial on `systemd`](https://www.digitalocean.com/community/tutorials/systemd-essentials-working-with-services-units-and-the-journal)
- [Lucas Nussbaum's tutorial on `systemd`](https://www.slideshare.net/slideshow/systemd-46731240/46731240)
- ["Linux explained part 2 : Bootloader, Init and Shell"](https://zedas.fr/posts/linux-explained-2-init-and-shell/)
- ["The current state of init systems"](https://phndiaye.github.io/the-current-state-of-init-systems.html)

---

## What is `systemd`? (pt. 1)

> `systemd` $\approx$ today's most common __init system__ for Linux systems

- but what's an _init system_ in the first place?

---

## What happens when you boot a Linux system?

### Overview

![](./boot-complex.png)

---

## What happens when you boot a Linux system?

### The role of the _init system_

{{% multicol %}}
{{% col %}}
![](./boot-init.png)
{{% /col %}}
{{% col %}}
The init system is the _first process_ started by the _kernel_
<br>
(this is why it has __PID=1__)

###
### Responsibilities

- _Starting_ all other _processes_ (namely, the __services__):
    + including the _display server_ (e.g. X, Wayland)
    + including the _window manager_ (e.g. Gnome, KDE)
    + including _daemons_ and _services_ (e.g. `sshd`, `httpd`)

- _Mounting_ the __file systems__
    + e.g. the _home_ partition, the _swap_ partition, etc.

- _Setting up_ the __network__
    + e.g. starting the _network manager_, hence connecting to the _default networks_, etc.

- Suspending/hibernating (and resuming), rebooting, and shutting down the system
{{% /col %}}
{{% /multicol %}}

---

## What happens when you boot a Linux system?

### Once the system is up and running...

{{% multicol %}}
{{% col %}}
![](./boot-architecture.png)
{{% /col %}}
{{% col top-margin="30vh" %}}
The init system has:
1. started all the system services
1. mounted all the file systems
1. set up the network
1. started the display server
1. started the window manager
1. started the user session
{{% /col %}}
{{% /multicol %}}

---

## Init systems

- [SysVinit](https://en.wikipedia.org/wiki/Init) (1980s—early 2000s) first family of init systems for Unix-like systems

- [Upstart](https://en.wikipedia.org/wiki/Upstart_(software)) (2006—2014) developed by Canonical for Ubuntu

- [runit](https://en.wikipedia.org/wiki/Runit) (2004—present) used by niche Linux distributions

- [OpenRC](https://en.wikipedia.org/wiki/OpenRC) (2007—present) used by Gentoo and Alpine Linux

- [`launchd`](https://en.wikipedia.org/wiki/Launchd) (2005—present) used by macOS, developed by Apple

- [`systemd`](https://en.wikipedia.org/wiki/Systemd) (2010—present) inspired by `launchd`, used by most Linux distributions nowadays

- Comparison here: <https://wiki.gentoo.org/wiki/Comparison_of_init_systems>

---

## What is `systemd`? (pt. 2)

> `systemd` is a _suite_ of system management __daemons__, __libraries__, and __utilities__ designed as a _central management and configuration platform_ for the Linux OS

{{< image src="systemd-components.svg" width="100%" >}}

---

## Examples

- Timer: duckdns
- SSHD service
- DockerD service
