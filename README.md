# STP
Spanning Tree Protocol (STP)
The Spanning Tree Protocol (STP) is an IEEE 802.1 standard that uses a software-based spanning-tree algorithm in bridge devices, including switches, to block some ports and allow only one active connection between any LAN segment (collision domain). It also prevents loops that can occur with multiple active paths between stops.
The spanning-tree algorithm is used in bridge and switch-based networks to determine the best path for traffic to travel from source to destination. This algorithm considers all backup paths and activates only one of them at any given time.
In networks actively using the Spanning Tree Protocol, there is one root bridge for each network, one root port for each non-root bridge, and one designated port assigned for traffic to pass through in each segment.


Some Spanning Tree Terminology:

Bridge ID: The MAC address of a switch is its bridge ID. It is important for selecting the root bridge in the network.
Non-root bridge: All bridges except the root bridge are non-root bridges.
Root port: The root port is always the port that is directly connected to the root bridge or closest to it.
Designated port cost: Considered when there are multiple non-root connections between two switches. It is calculated based on the bandwidth.
Bridge Protocol Data Unit (BPDU): All switches and bridges included in the same local area network (LAN) communicate with each other via BPDU messages in the spanning-tree protocol. BPDU messages contain information such as the switch priority, port priority, port value, and MAC address. The spanning-tree protocol uses this information to select the root bridge, root port, and designated port.
Convergence: Convergence occurs when all ports of switches and bridges transition from blocking state to forwarding state. No data is transmitted until convergence is complete. All devices must be updated before data transmission can resume. Convergence is important to ensure that all devices have the same database, but it takes some time.


Root Bridge Selection

The root bridge is the logical center of the spanning-tree topology in switched networks. Each bridge on the topology claims to be the root by sending "hello BPDU" messages to each other. These messages contain:

Bridge ID (BID): Initially, this value is the bridge's own ID since each bridge assumes itself to be the root bridge.
Priority: Belongs to the root bridge. Again, since each bridge assumes itself to be the root bridge, this value is the bridge's own priority.
Cost to reach the root: Initially, this value is zero.
The bridge with the lowest priority becomes the root bridge in the root bridge selection process. If the priorities are equal, the bridge with the lowest ID becomes the root.





Bandwidth 	 STP Cost
 4 Mbps	        250
 10 Mbps	      100
 16 Mbps	       62
 45 Mbps	       39
 100 Mbps	       19
 155 Mbps	       14
 622 Mbps      	 6
 1 Gbps	         4
 10 Gbps	       2
 
 
 
 
 Responses to Network Changes

Root bridges send "hello" BPDUs every two seconds to indicate that they are operational. All other switches and bridges receive these BPDUs. If hellos are received on the path where data is being transmitted, the path to the root is still operational. If there is a delay in receiving hellos, the spanning-tree process restarts. The "hello" BPDU defines the time that switches need to wait before responding to network changes. This time is defined by the "hello time," the "max age," and the "forward delay."

"Hello time" specifies how often the root sends periodic "hello" BPDUs that will be transmitted by switches/bridges in sequence. The default time is two seconds.
"Max age" is the time that switches/bridges must wait after not receiving a "hello" before changing the STP topology. The default time is 20 seconds.
"Forward delay" is the time that it takes for an interface to transition from the blocking state to the forwarding state.


In a stable network, the STP process works as follows.

The root sends "hello" BPDUs from all of its interfaces (with a cost of 0 for these BPDUs).
Neighboring switches/bridges forward the "hello" BPDUs, with their own cost added to the cost from non-root designated ports.
Each switch/bridge repeats step 2 every time it receives a "hello" BPDU.
Each bridge repeats step 1 at each "hello time".
If a switch/bridge does not receive a "hello" BPDU within the "hello time" interval, it continues to function normally for the maximum wait time, and if it still hasn't received a BPDU, it reacts to change the STP topology.


**The Role of Spanning Tree Protocol
The spanning-tree algorithm puts each bridge and switch port in one of the blocking or forwarding states to prevent loops from occurring. These port states are:

Blocking state: Frames cannot be sent or received on the ports and only BPDU messages are listened to. The purpose of this state is to prevent loops from occurring. When switches are powered on, all ports are in blocking state by default.

Listening state: Connection points listen to BPDU messages to ensure there are no loops in the network before forwarding frames. Connection points in this state prepare to transmit data without forwarding frames, and start building the MAC address table.

Learning state: Connection points listen to BPDU messages and learn all paths in the network. Connection points in this state start building the MAC address table, but do not forward frames yet.

Forwarding state: The ports are considered to be part of the active spanning-tree topology. All forwarding ports can send and receive frames.
