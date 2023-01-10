# Switchport Security Configuration

# PORT SECURITY

---

```
**SW1#** interface Fa0/1
**SW1(config-if)#** switchport mode access
**SW1(config-if)#** switchport port-security
!
! ALLOW MAXIMUM OF 2 MAC ADDRESSES TO BE LEARNED.
! 
**SW1(config-if)**# switchport port-security maximum 2
!
! STATICLY ASSIGN SECURE MAC
!
**SW1(config-if)**# switchport security mac-address aaaa.bbbb.1234
!
! "STICK" the mac address to the running config
**SW1(config-if)**# switchport mac-address sticky
```

## **Limit and Learn MAC Addresses**

---

<aside>
<img src="https://www.notion.so/icons/command-line_gray.svg" alt="https://www.notion.so/icons/command-line_gray.svg" width="40px" /> Set the maximum number of MAC addresses allowed on a port.

</aside>

```
switchport port-security *<maximum value>*
```

### MAC Address Learning

---

1. **Manually Configured**

`Switch(config-if)#`

```
switchport port-security mac-address *<mac-addresss>*
```

---

1. **Dynamically Learned**
    
     `**Current source MAC**` for the device connected to the port is automatically secured but is **not added to the startup configuration**.
    
    ---
    
     If the switch is rebooted, the port will have to re-learn the device’s MAC address
    

---

1. **Dynamically Learned-`Sticky`**

`Switch(config-if)#`

```
switchport port-security mac-address sticky
```

---

## Validating Configuration

---

- Use `**show port-security interface**` and the **`show port-security address`** command to verify the configuration.
    
    ---
    
    ```
    **show port-security interface *<interface-id>***
    ```
    
    **`SAMPLE OUTPUT`**
    
    ```
    S1# **show port-security interface fa0/1**
    Port Security              : Enabled
    Port Status                : Secure-up
    Violation Mode             : Shutdown
    Aging Time                 : 0 mins
    Aging Type                 : Absolute
    SecureStatic Address Aging : Disabled
    Maximum MAC Addresses      : 2
    Total MAC Addresses        : 2
    Configured MAC Addresses   : 1
    Sticky MAC Addresses       : 1
    Last Source Address:Vlan   : a41f.7272.676a:1
    Security Violation Count   : 0
    ```
    
    ---
    
    ```
    show port-security address
    ```
    
    **`SAMPLE OUTPUT`**
    
    ```
    S1# show port-security address
                   Secure Mac Address Table
    -----------------------------------------------------------------------------
    Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                       (mins)    
    ----    -----------       ----                          -----   -------------
    1    a41f.7272.676a    SecureSticky                  Fa0/1        -
    1    aaaa.bbbb.1234    SecureConfigured              Fa0/1        -
    -----------------------------------------------------------------------------
    Total Addresses in System (excluding one mac per port)     : 1
    Max  Addresses limit in System (excluding one mac per port) : 8192
    ```
    

---

## **Port Security Aging**

---

- **`Absolute`** - The secure addresses on the port are deleted after the specified aging time.
- **`Inactivity`** - The secure addresses on the port are deleted only if they are inactive for the specified aging time.

```
switchport port-security aging type ******<type>******
```

```
switchport port-security aging time *<time>* 
```

**`SAMPLE IMPLEMENTATION`**

```
...
S1(config)# interface fa0/1
S1(config-if)# switchport port-security aging time 10 
S1(config-if)# switchport port-security aging type inactivity 
S1(config-if)# end
S1# show port-security interface fa0/1
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 10 mins
Aging Type                 : Inactivity
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 2
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : a41f.7272.676a:1
Security Violation Count   : 0
```

---

## **Port Security Violation Modes**

---

```
Switch(config-if)# switchport port-security violation ***<*>***

```

- `* { protect | restrict | shutdown}`
    
    ---
    
    | Violation Mode | Discards Offending Traffic | Sends Syslog Message | Increase Violation Counter | Shuts Down Port |
    | --- | --- | --- | --- | --- |
    | shutdown | Y | Y | Y | Y |
    | restrict | Y | Y | Y | N |
    | protect | Y | N | N | N |

[https://www.notion.so/vdetorres-it/02631-icon-service-Logic-Apps-f13c25b0442e4820b9b2037d7a61f893](https://www.notion.so/02631-icon-service-Logic-Apps-f13c25b0442e4820b9b2037d7a61f893)