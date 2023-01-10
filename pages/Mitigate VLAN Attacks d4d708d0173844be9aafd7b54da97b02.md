# Mitigate VLAN Attacks

---

- Disable **DTP** negotiations on **non-trunking** ports by using the `**switchport mode access**` interface configuration command.

```
S1(config)# interface range fa0/1 - 16
S1(config-if-range)# switchport mode access
S1(config-if-range)# exit
```

---

- Disable unused ports and put them in an unused VLAN.

```
S1(config)# interface range fa0/17 - 20
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 1000
S1(config-if-range)# shutdown
S1(config-if-range)# exit
```

---

- Manually enable the trunk link on a trunking port by using the **`switchport mode trunk**` command.

```
S1(config)# interface range fa0/21 - 24
S1(config-if-range)# switchport mode trunk
...
```

---

- Disable DTP (auto trunking) negotiations on trunking ports by using the **`switchport nonegotiate`** command.

---

```
...
S1(config-if-range)# switchport nonegotiate

...
```

---

---

- Set the native VLAN to a VLAN other than VLAN 1 by using the `**switchport trunk native vlan** <*vlan_number>*` command.

```
...
S1(config-if-range)# switchport trunk native vlan 999
S1(config-if-range)# end
S1#

```

# **APPLICATION**

---

**`OBJECTIVE`**: Implement Security Measures Against VLAN Hopping Attacks 

---

`**SCENARIO**`

```
* FastEthernet ports 0/1 through 0/4 are used for trunking with other switches.
* FastEthernet ports 0/5 through 0/10 are unused.
* FastEthernet ports 0/11 through 0/24 are active ports currently in use.
```

---

**`I. Trunks`**

- Configure the interfaces as nonnegotiating trunks assigned to default VLAN 99.

---

```
S1(config)#interface range fa0/1 - 4
S1(config-if-range)#switchport mode trunk
S1(config-if-range)#switchport nonegotiate
S1(config-if-range)#switchport trunk native vlan 99
S1(config-if-range)# exit
```

**`II. Unused Ports`**

- Configure the unused ports as access ports, assign them to VLAN 86, and shutdown the ports.

---

```
S1(config)#interface range fa0/5 - 10
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 86
S1(config-if-range)#shutdown
S1(config-if-range)# exit

```

**`III. Access Ports`**

- Configure them to prevent trunking.
    
    ---
    
    ```
    S1(config)#interface range fa0/11 - 24
    S1(config-if-range)#switchport mode access
    ```