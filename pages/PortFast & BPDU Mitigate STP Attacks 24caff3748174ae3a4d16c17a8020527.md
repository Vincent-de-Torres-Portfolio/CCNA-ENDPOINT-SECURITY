# PortFast & BPDU:  Mitigate STP Attacks

. To mitigate Spanning Tree Protocol (STP) manipulation attacks, use `PortFast` and `Bridge Protocol Data Unit (BPDU) Guard:`

- **`PortFast-`** PortFast immediately brings an interface configured as an access port to the forwarding state from a blocking state, bypassing the listening and learning states. Apply to all end-user ports. PortFast should only be configured on ports attached to end devices.
- **`BPDU Guard`** BPDU guard immediately error disables a port that receives a BPDU. Like PortFast, BPDU guard should only be configured on interfaces attached to end devices.

## `CONFIGURATION`

---

```
S1(config)# spanning-tree portfast default
```

```
S1(config-if)# spanning-tree bpduguard enable
S1(config-if)#  exit
! 
!  to enable bpdu on all portfast enable ports
! 
S1(config)# spanning-tree portfast bbpduguard  default
```