# VethConnect-Network-Namespace-Interconnection-with-Veth-Cable
Create two network namespace named red and green  

```
1.sudo ip netns add red
2.sudo ip netns add green
```

Now create a bridge so that you can connect two name space with bridge

```
3.sudo ip link add br0 type bridge
```

Initially it is down. So you have to make it up and assign an ip address for bridge

```
4.sudo ip link set br0 up
5.sudo ip addr add 192.168.0.1/16 dev br0
```

Now we will create veth cable which has two sides one side will be connected with namespace and the other side will be connected with bridge. By this way we will connect our name space with bridge

```
6.sudo ip link add vethcab0 type veth peer name red0
7.sudo ip link set vethcab0 master br0
8.sudo ip link set red0 netns red
```
