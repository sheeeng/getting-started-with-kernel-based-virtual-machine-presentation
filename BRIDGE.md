# Bridge

```console
➜  ~ sudo virsh net-list --all
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes

➜  ~ sudo virsh net-dumpxml default
<network>
  <name>default</name>
  <uuid>258ae95f-f434-45e8-aa8c-09fb2d9735d0</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:a8:72:47'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.254'/>
    </dhcp>
  </ip>
</network>

➜  ~
➜  ~
➜  ~ ip link show type bridge
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:a8:72:47 brd ff:ff:ff:ff:ff:ff
➜  ~
➜  ~ ip link show dev virbr0
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:a8:72:47 brd ff:ff:ff:ff:ff:ff
➜  ~

```


```console
➜  ~ sudo ip link show type bridge
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:a8:72:47 brd ff:ff:ff:ff:ff:ff
➜  ~
➜  ~ sudo ip link add br0 type bridge
➜  ~
➜  ~ sudo ip link show type bridge
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:a8:72:47 brd ff:ff:ff:ff:ff:ff
5: br0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 2e:7e:59:50:a5:33 brd ff:ff:ff:ff:ff:ff
➜  ~



```
