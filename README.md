# VethConnect-Network-Namespace-Interconnection-with-Veth-Cable

What we are going to build

<img width="377" alt="Screenshot 2023-06-23 052930" src="https://github.com/nobelrakib/VethConnect-Network-Namespace-Interconnection-with-Veth-Cable/assets/53372696/59946583-9b7d-40ca-8769-56d0207396a8">

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

In the above command we have connected our red namespace with bridge. Now lets enter our red namespace and check interface.

```
9.sudo ip netns exec red bash
10.ip link

```

You will see two interface.

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
4: red0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether f2:9b:d5:07:b1:a9 brd ff:ff:ff:ff:ff:ff

```

One is loop back interface and another is our red0 interface. Initially both of them will be down. You have to make both of them up and assign an ip for our red0 namespace.

```
11.ip link set lo up
12.ip link set ceth0 up
13.ip addr add 192.168.0.2/16 dev red0
```

Now exit from this name space by typing exit and come to root namespace and make sure to up the interface which is connected to brigde.

```
15.sudo ip link set vethcab0 up

```

So we have successfully added our red namespace with bridge.

Now we will follow the same steps for our green namespace.

```
16. sudo ip link add vethcab1 type veth peer name green0
17. sudo ip link set vethcab1 master br0
18. sudo ip link set green0 netns green
19.sudo ip netns exec green bash
20.ip link set lo up
21.ip link set green0 up
22.ip addr add 192.168.0.3/16 dev green0
23.exit
24.sudo ip link set vethcab1 up

```

Now enter into red name space and ping green name space ip address

```
sudo ip netns exec red bash
ping 192.168.0.3
```

<img width="437" alt="Screenshot 2023-06-23 134411" src="https://github.com/nobelrakib/VethConnect-Network-Namespace-Interconnection-with-Veth-Cable/assets/53372696/d53ef237-76d1-454f-915c-febacbb3dba1">


So our namespace is connected by bridge. For this reason we can ping one from another.
