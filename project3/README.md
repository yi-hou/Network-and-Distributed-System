In this project, we have a sender and a receiver to transmit data. Data that is printed out from receiver side should be the same as the input from the sender side.
In sender, it sends data in a window size, which is 10 initially. It has a buffer to store sent packets. 
If there's congestion(packets are dropped) or timeout, the window size multiplicative decrease, otherwise addicative increase.
It tries to receive same number of packets as it sent to the receiver. If the ack number of received packet is in the buffer, remove it from the buffer.
There is also a global counter to record how many times receiver send same ack to a packet.
If the time is greater than or equal to 3, which means the packet is always dropped through transmission, then send the same packet twice.
Every time, when the sender send packet to the receiver, it checks if there is packet in buffer. If buffer is not empty, which means there's some packet need to be retransmitted. 
Then, the sender will retransmit packets in buffer first, and then send next readin data.
It continues sending data until all data is sent.
In receiver, there is a out-order-packets dictionary, which stores data that is not in the order that is supposed to receive.
It has another list to store sequence number that is received so far.
Every time when the receiver receives a packet, it first checks if it the sequence number of the packet is already in the received sequence number list.
If it is, then just ack it, but not write it out.
If it is not and it is in a order , which means it's the first time to receive it, ack it and write it out immediately.
Then, go through each packet in out-order-packets to check if there are packets that are in following order by this packet.
If there is, write them out as well.
If it is not in a order, ack it and put it into out-order-packets. 
After write out all data, close socket. 

There are lots of challenges we faced during this project. We were first not familiar with how UDP and TCP works.
Through this project, we fully understand how they are supposed to work.

We tested our code through the test files.