# docker-network-namespace
Create route between two network namespace and ping one another.

# PREREQUISITES
### * _**CentOS/Ubuntu**_
### * _**Linux CLI**_
### * _**Knowledge on networking**_

**Step 1: Create two different namespace**\
ip netns add red\
ip netns add blue

**Step 2: Create virtual peering between both namespace**\
ip link add veth-red type veth peer veth-blue

**Step 3: Assign each peer to each namespace**\
ip link set veth-red netns red\
ip link set veth-blue netns blue

**Step 4: Assign IP to each namespace**\
ip netns exec red ip addr add 192.168.15.1/32 dev veth-red\
ip netns exec blue ip addr add 192.168.15.2/32 dev veth-blue

**Step 5: Active each namespace**\
ip netns exec blue ip link set veth-blue up\
ip netns exec red ip link set veth-red up

**Step 6: Create route between two namespace**\
ip netns exec red route add 192.168.15.2/32 dev veth-red\
ip netns exec blue route add 192.168.15.1/32 dev veth-blue

**Step 7: Check ping between namespaces**\
ip netns exec red ping 192.168.15.2\
ip netns exec blue ping 192.168.15.1
