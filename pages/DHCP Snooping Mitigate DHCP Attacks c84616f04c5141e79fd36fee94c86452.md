# DHCP Snooping : Mitigate DHCP Attacks

---

## `DHCP Snooping`

DHCP snooping determines whether DHCP messages are from an administratively configured trusted or untrusted source.

- It then filters DHCP messages and rate-limits DHCP traffic from untrusted sources.

### Use the following steps to enable DHCP snooping:

---

- **Step 1**. Enable DHCP snooping by using the **`ip dhcp` snooping** global configuration command.
- **Step 2**. On trusted ports, use the `**ip dhcp snooping trust**` interface configuration command.
- **Step 3**. Limit the number of DHCP discovery messages that can be received per second on untrusted ports by using the**`ip dhcp snooping limit rate**` interface configuration command.
- **Step 4**. Enable DHCP snooping by VLAN, or by a range of VLANs, by using the **`ip dhcp snooping** *vlan*` global configuration command.

---

```
S1(config)#ip dhcp snooping
S1(config)#interface f0/1
S1(config-if)#ip dhcp snooping trust
S1(config-if)#exit
S1(config)#interface range f0/5 - 24
S1(config-if-range)#ip dhcp snooping limit rate 6
S1(config-if-range)#exit
S1(config)#ip dhcp snooping vlan 5,10,50-52
S1(config)#end
```

## **`DHCP Snooping Configuration`**

---

![dhcp.svg](DHCP%20Snooping%20Mitigate%20DHCP%20Attacks%20c84616f04c5141e79fd36fee94c86452/dhcp.svg)

---

- Enable DHCP snooping globally for the switch.

```
S1(config)#ip dhcp snooping
```

---

- Trust the ports connecting to the DCP Server and the Multilayer Switch. `**Gi 0/1,**` `**Gi0/2**`

```
S1(config)#interface range g0/1 - 2
S1(config-if-range)#ip dhcp snooping trust
S1(config-if-range)#exit
```

---

- Limit the DHCP messages to no more than 10 per second for the rest of the ports.

```
S1(config)#interface range f0/1 - 24
S1(config-if-range)#ip dhcp snooping limit rate 10
S1(config-if-range)#exit
```

---

- Enable DHCP snooping for VLANs **10, 20, 30-49**.

```
S1(config)# ip dhcp snooping vlan 10,20,30-
```

## **`Verify DHCP Snooping Configuration`**

---

```
**show ip dhcp snooping**
```

**`SAMPLE OUTPUT`**

```
S1 # show ip dhcp snooping

Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10,20,30-49
DHCP snooping is operational on following VLANs:
none
DHCP snooping is configured on the following L3 Interfaces:
Insertion of option 82 is enabled
   circuit-id default format: vlan-mod-port
   remote-id: 0cd9.96d2.3f80 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:
Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------   
GigabitEthernet0/1         yes        yes             unlimited
  Custom circuit-ids:
GigabitEthernet0/2         yes        yes             unlimited
  Custom circuit-ids:
FastEthernet0/1            no         no              10         
  Custom circuit-ids:
```

---

```
show ip dhcp snooping binding
```

**`SAMPLE OUTPUT`**

```
S1#show ip dhcp snooping binding
MacAddress         IpAddress       Lease(sec) Type          VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
00:03:47:B5:9F:AD  10.0.0.10       193185     dhcp-snooping 5    FastEthernet0/1
S1#
```

---