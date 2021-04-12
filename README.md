# c9300-apphosting

See guide @ https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/prog/configuration/174/b_174_programmability_cg/application_hosting.html#id_96211

## Greengrass on Management port:

```
app-hosting appid Greengrass
 app-vnic management guest-interface 0
  guest-ipaddress 10.85.134.98 netmask 255.255.255.192
 app-default-gateway 10.85.134.126 guest-interface 0
 app-resource docker
  run-opts 1 "--entrypoint /greengrass-entrypoint.sh"
 app-resource package-profile custom
 app-resource profile custom
  cpu 5000
  memory 1000
  persist-disk 8192
  name-server0 10.85.134.66
```

## Management port configuration:
```
Building configuration...

Current configuration : 155 bytes
!
interface GigabitEthernet0/0
 vrf forwarding Mgmt-vrf
 ip dhcp client client-id ascii cisco-70d3.79be.8580-Gi0/0
 ip address dhcp
 negotiation auto
end
```

## Greengrass on Front Panel VLAN Port
```
C9300#show ru | s Gr
app-hosting appid Greengrass
 app-vnic AppGigabitEthernet trunk
  vlan 48 guest-interface 0
   guest-ipaddress 10.1.1.9 netmask 255.255.255.0
 app-default-gateway 10.1.1.3 guest-interface 0
 app-resource docker
  run-opts 1 "--entrypoint /greengrass-entrypoint.sh"
 app-resource package-profile custom
 app-resource profile custom
  cpu 5000
  memory 1000
  persist-disk 8192
 name-server0 10.1.1.3
```

## Interfaces configuration:
```
C9300#show int status  | e notc

Port         Name               Status       Vlan       Duplex  Speed Type
Gi1/0/24                        connected    trunk      a-full a-1000 10/100/1000BaseTX
Ap1/0/1                         connected    trunk      a-full a-1000 App-hosting port
C9300#

C9300#show run int Ap1/0/1
Building configuration...

Current configuration : 106 bytes
!
interface AppGigabitEthernet1/0/1
 switchport trunk allowed vlan 48,100-200
 switchport mode trunk
end

C9300#show run int Gi1/0/24
Building configuration...

Current configuration : 104 bytes
!
interface GigabitEthernet1/0/24
 switchport trunk allowed vlan 48,100-200
 switchport mode trunk
end

C9300#show run int vlan 48
Building configuration...

Current configuration : 59 bytes
!
interface Vlan48
 ip address 10.1.1.5 255.255.255.0
end
```

## Common commands:

```
app-hosting install appid Greengrass package usbflash1:Greengrass.tar
app-hosting stop appid Greengrass
app-hosting deactivate appid Greengrass
app-hosting activate appid Greengrass
app-hosting start appid Greengrass
app-hosting connect appid Greengrass session

$ scp Greengrass.tar admin@10.1.1.5:usbflash1:/Greengrass.tar
```
