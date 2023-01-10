# Dynamic Arp Inspection

---

## Guidelines

---

- Enable DHCP snooping globally.
- Enable DHCP snooping on selected VLANs.
- Enable DAI on selected VLANs.
- Configure trusted interfaces for DHCP snooping and ARP inspection.

---

```
S1(config)#ip dhcp snooping
S1(config)#ip dhcp snooping vlan 10
S1(config)#ip arp inspection vlan 10
S1(config)#interface fa0/24
S1(config-if)#ip dhcp snooping trust
S1(config-if)#ip arp inspection trust
```

**`ip arp inspection validate src-mac dst-mac ip`**

---

![dhcp.svg](DHCP%20Snooping%20Mitigate%20DHCP%20Attacks%20c84616f04c5141e79fd36fee94c86452/dhcp.svg)

You are currently logged into S1. Enable DHCP snooping globally for the switch.

```
S1(config)#ip dhcp snooping
```

Trust the interfaces for both DHCP snooping and DAI

```
S1(config)#interface range g0/1 - 2 
S1(config-if-range)#ip dhcp snooping trust
S1(config-if-range)#ip arp inspection trust
S1(config-if-range)#exit
```

Enable DHCP snooping and DAI for VLANs **10,20,30-49**.

```
S1(config)#ip dhcp snooping vlan 10,20,30-49
S1(config)#ip arp inspection vlan 10,20,30-49
```

---