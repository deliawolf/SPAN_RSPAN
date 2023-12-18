# SPAN & RSPAN
## SPAN (Switch port Analyzer)

1. The source port is a port that is monitored for traffic analysis.
2. The traffic is copied to the destination (also called monitor) port.


### Local SPAN Configuration
#### Basic Configuration
```
SW1(config)# monitor session 1 source interface Gigabit0/1
SW1(config)# monitor session 1 destination interface Gigabit0/2
```
#### Simple Example: Monitor only received (rx) traffic on GigabitEthernet0/1 and send to GigabitEthernet0/2.
```
Router> enable
Router# configure terminal
Router(config)# monitor session 1 source interface GigabitEthernet0/1 rx
Router(config)# monitor session 1 destination interface GigabitEthernet0/2
Router(config)# end
```
#### Intermediate Example: Monitor only transmitted (tx) traffic on GigabitEthernet0/1 and send to GigabitEthernet0/2.
```
Router> enable
Router# configure terminal
Router(config)# monitor session 1 source interface GigabitEthernet0/1 tx
Router(config)# monitor session 1 destination interface GigabitEthernet0/2
Router(config)# end
```
#### Complex Example: Monitor both received (rx) and transmitted (tx) traffic on GigabitEthernet0/1 and GigabitEthernet0/2. Send all this data to GigabitEthernet0/3.
```
Router> enable
Router# configure terminal
Router(config)# monitor session 1 source interface GigabitEthernet0/1 both
Router(config)# monitor session 1 source interface GigabitEthernet0/2 both
Router(config)# monitor session 1 destination interface GigabitEthernet0/3
Router(config)# end
```

## RSPAN (Remote Switch port Analyzer)

RSPAN supports source and destination ports on different switches, while local SPAN supports only source and destination ports on the same switch.

### RSPAN Configuration

In the example below, VLAN 100 is configured to support RSPAN, the interface GigabitEthernet 0/1 as the source port in session 2, and VLAN 100 as the destination in session 2.
```
SW1(config)# vlan 100
SW1(config-vlan)# name SPAN-VLAN
SW1(config-vlan)# remote-span
SW1(config)# monitor session 2 source interface Gig0/1
SW1(config)# monitor session 2 destination remote vlan 100
```
```
SW2(config)# vlan 100
SW2(config-vlan)# name SPAN-VLAN
SW2(config-vlan)# remote-span
SW2(config)# monitor session 3 destination interface Gig0/2
SW2(config)# monitor session 3 source remote vlan 100
```
For the intermediate switch, you just need to define the RSPAN VLAN and allow it through the trunk links. You don't need to set up a source or destination session on this switch because it's not directly involved in the monitoring – it just needs to pass the RSPAN VLAN traffic between the source and destination switches (SW1 and SW2 in your case).

Remember that the trunk link configurations between the switches should also allow VLAN 100 to pass through:
```
SW(config)# vlan 100
SW(config-vlan)# name SPAN-VLAN
SW(config-vlan)# remote-span

SW(config)# interface Gig0/1
SW(config-if)# switchport trunk allowed vlan add 100
```

## Verify Configuration
```
SW1# show monitor
```


