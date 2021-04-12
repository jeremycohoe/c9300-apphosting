# c9300-apphosting

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

## AppGigE ?

```
Put example here :)
```
