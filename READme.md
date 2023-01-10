# NETWORK ENDPOINT SECURITY

---

[LAN Security Concepts](pages/LAN%20Security%20Concepts%20e2f59c407c61415082c14579532379c3.md)

[Switchport Security Configuration](pages/Switchport%20Security%20Configuration%200b97df41fd3e4ee698f0a04522625958.md)

[Mitigate VLAN Attacks](pages/Mitigate%20VLAN%20Attacks%20d4d708d0173844be9aafd7b54da97b02.md)

[DHCP Snooping : Mitigate DHCP Attacks](pages/DHCP%20Snooping%20Mitigate%20DHCP%20Attacks%20c84616f04c5141e79fd36fee94c86452.md)

[Dynamic Arp Inspection](pages/Dynamic%20Arp%20Inspection%20ad73bbb97c4346178df1171053bc2117.md)

[PortFast & BPDU:  Mitigate STP Attacks](pages/PortFast%20&%20BPDU%20Mitigate%20STP%20Attacks%2024caff3748174ae3a4d16c17a8020527.md)

## SUMMARY

- [SWITCHPORT SECURITY](pages/Switchport%20Security%20Configuration%200b97df41fd3e4ee698f0a04522625958.md)

---

- By default, Layer 2 switch ports are set to dynamic auto (trunking on).
    - The switch can be configured to learn about MAC addresses on a secure port in one of three ways: `manually configured`, `dynamically learned`, and `dynamically learned – sticky`.
- All switch ports (interfaces) should be secured before the switch is deployed for production use. The simplest and most effective method to prevent MAC address table overflow attacks is to enable `port security`.
- Port security aging can be used to set the aging time for `static` and `dynamic secure` addresses on a port.
- Two types of aging are supported per port: `absolute` & `inactivity`.
- If the MAC address of a device attached to the port differs from the list of secure addresses, then a `port violation` occurs.
    - By default, the port enters the `error-disabled state`. When a port is shutdown and placed in the error-disabled state, no traffic is sent or received on that port. To display port security settings for the switch, use the `**show port-security**` command.

---

To mitigate `VLAN Hopping` attacks:

- **Step 1.** Disable DTP negotiations on non-trunking ports.
- **Step 2.** Disable unused ports.
- **Step 3.** Manually enable the trunk link on a trunking port.
- **Step 4.** Disable DTP negotiations on trunking ports
- **Step 5.** Set the native VLAN to a VLAN other than VLAN 1.

---

The goal of a DHCP starvation attack is to create a Denial of Service (DoS) for connecting clients. DHCP spoofing attacks can be mitigated by using DHCP snooping on trusted ports.

`DHCP snooping` determines whether DHCP messages are from an administratively-configured trusted or untrusted source. 

- **Step 1.** Enable DHCP snooping
- **Step 2.** On trusted ports, use the **`ip dhcp snooping trust`** interface configuration command.
- **Step 3.** Limit the number of DHCP discovery messages that can be received per second on untrusted ports.
- **Step 4.** Enable DHCP snooping by VLAN, or by a range of VLANs

---

`Dynamic ARP inspection (DAI)` requires `DHCP snooping and helps prevent ARP attacks by:`

- Not relaying invalid or gratuitous ARP Replies out to other ports in the same VLAN.
- Intercepting all ARP Requests and Replies on untrusted ports.
- Verifying each intercepted packet for a valid IP-to-MAC binding.
- Dropping and logging ARP Replies coming from invalid to prevent ARP poisoning.
- Error-disabling the interface if the configured DAI number of ARP packets is exceeded.

---

To mitigate the chances of ARP spoofing and ARP poisoning, follow these DAI implementation guidelines:

- Enable DHCP snooping globally.
- Enable DHCP snooping on selected VLANs.
- Enable DAI on selected VLANs.
- Configure trusted interfaces for DHCP snooping and ARP inspection.

As a general guideline, configure all access switch ports as untrusted and all uplink ports that are connected to other switches as trusted.

DAI can also be configured to check for both destination or source MAC and IP addresses:

- **Destination MAC** - Checks the destination MAC address in the Ethernet header against the target MAC address in ARP body.
- **Source MAC** - Checks the source MAC address in the Ethernet header against the sender MAC address in the ARP body.
- **IP address** - Checks the ARP body for invalid and unexpected IP addresses including addresses 0.0.0.0, 255.255.255.255, and all IP multicast addresses.

To mitigate Spanning Tree Protocol (STP) manipulation attacks, use PortFast and Bridge Protocol Data Unit (BPDU) Guard:

- **`PortFast**` - PortFast immediately brings an interface configured as an access or trunk port to the forwarding state from a blocking state, bypassing the listening and learning states. Apply to all end-user ports. PortFast should only be configured on ports attached to end devices. PortFast bypasses the STP listening and learning states to minimize the time that access ports must wait for STP to converge. If PortFast is enabled on a port connecting to another switch, there is a risk of creating a spanning-tree loop.
- **`BPDU Guard`** - BPDU guard immediately error disables a port that receives a BPDU. Like PortFast, BPDU guard should only be configured on interfaces attached to end devices. BPDU Guard can be enabled on a port by using the **`spanning-tree bpduguard enable`** interface configuration command.
- Use the **`spanning-tree portfast bpduguard default`** global configuration command to globally enable BPDU guard on all PortFast-enabled ports.
