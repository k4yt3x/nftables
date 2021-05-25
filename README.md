# K4YT3X's Template nftables Script

This repository hosts my personal `nftables.conf` script. This file is intended to be used as a template for creating nftables profiles for hosts. The default settings in this script are suitable for endpoint devices (e.g., workstations). This configuration file is also compliant with [RFC4890](https://tools.ietf.org/html/rfc4890). Below is an abstract of what is allowed in the profile by default.

- **IPv4**
  - Established/related segments accepted
  - DHCP accepted
  - ICMP necessary debugging types accepted
- **IPv6**
  - Established/related segments accepted
  - DHCPv6 accepted
  - ICMPv6 types in [RFC4890](https://tools.ietf.org/html/rfc4890) accepted

**Please review the script file carefully before applying it.** You are responsible for actions done to your system.

## Applying Template Rules

You may run this script with the `nft` command to load the rules in this profile. You can also install this profile to the system, so the `nftables.service` will automatically load this profile upon service restart or reboot. If you would like to load this configuration once, simply run `nft -f nftables.conf` or `./nftables.conf`. If you would like to make the changes persist after a reboot, you will have to overwrite contents of the `nftables.conf` which the `nftables.service` reads rules from. The location of this file varies depending on your distribution. Below are some examples.

- CentOS/RHEL: `/etc/sysconfig/nftables.conf`
- Debian: `/etc/nftables.conf`

After editing the files, run the appropriate command to restart the nftables service to apply the rules (e.g., `systemctl restart nftables`). Once you finish loading the rules, you can verify the changes using the command `nft list ruleset`.

## Opening Ports

This script provides a convenient way of opening ports. In order to open ports, you only need to edit the `ACCEPT_TCP_PORTS` variable and the `ACCEPT_UDP_PORTS` variable. You will find these two variables at the beginning of the script.

```properties
# TCP ports to accept (both IPv4 and IPv6)
#define ACCEPT_TCP_PORTS = {}

# UDP ports to accept (both IPv4 and IPv6)
#define ACCEPT_UDP_PORTS = {}
```

To open a port, uncomment the corresponding line and add the comma-separated list of ports into the brackets at the end of the line. The example below opens TCP ports 22 and 80 and UDP port 53.

```properties
# TCP ports to accept (both IPv4 and IPv6)
define ACCEPT_TCP_PORTS = { 22, 80 }

# UDP ports to accept (both IPv4 and IPv6)
define ACCEPT_UDP_PORTS = { 53 }
```

Do not forget to reload `nftables.service` to apply the changes after finish editing the script file.

## Short URL for Downloading `nftables.conf`

For convenience, I have pointed the URL `https://k4t.io/nft` to the `nftables.conf` file. You may therefore download the `nftables.conf` file with the following command. However, be sure to check the file's integrity after downloading it if you choose to download using this method.

```shell
curl -L k4t.io/nft -o nftables.conf
```
