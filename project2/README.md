### Design of Code
This is a BGP Router class, handling moving packets around its neighbors.  
In this class, we have several global variables: updates, revokes, routes, relations, sockets.
Updates is a list to store route annoucements from neighbors.
Routes is a list to store current forwarding table, containing attributes of each network.
Revokes is a list to store packets that are revoked. 
Relations is a dictionary to store the relationship between neighbor networks.
Sockets is a dictionary to store the socket connection of each network.
 
We implemented following main functions in the class:
1. Update, which copies route announcements received from neighbors into updates list, and according to the required rules, sends the announcements to neighbors.
2. Forward, which applies a series of rules to find the best route in current forwarding table to forward data among neighbors.
3. Revoke, which copies revoke packet into revokes list and removes the revoked network from current forwarding table. We implemented rebuild function which uses updates list and revokes list to rebuild a forwarding table after revoke.
4. Coalesce, which aggregates routes that have same network(except for the last bit), same netmask, same next-hop, and same attributes. This method is called after each time update method is called.
5. Disaggregate, which removes revoked packet from current forwarding table. It rebuilds forwarding table according to updates list and revokes list. Since updates list and revokes list are never modified, by removing revoked packet from updates list, we can construct a new forwarding table.
6. Dump, which returns current forwarding table with only network, netmast, and next-hop fields.
7. If it doesn't find a proper path to send packet, it returns no route message to source router.

### Challenges we faced
The most annoying trouble we faced is that converting decimal network/netmask to binaries and vise versa. We wroute many helpers to deal with converting when comparing each network and netmask.
For aggregation and disaggregation, it's challenging that to check the networks, making sure that they are same except for the last bit, and to change the last bit of networks and netmasks.

### How tested code
By running tests.  
