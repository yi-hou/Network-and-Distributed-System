 ###Highly approach
 This is a greatly challenging project, and the design matters a lot for efficiently running the program.
 We designed a class, named server, which represents each server. In this class, there are many variables, 
 mainly aiming to keeping track of current status of the server. Each server has a local database which is 
 a dictionary for storing committed values, and a list of log entries, storing commands received from clients.
 There are other fields like commit id, last log index, and next index to track current status of appending
 log entries and committing log entries.
 
 It first starts a election. The server that starts election changes its type to candidate, increments 
 term number, adds itself into votes set which is a set containing all server ids who votes for this server,
 and sends requestVote RPC to other followers to ask vote. One follower each term could only votes once.
 There is a variable named votedFor, which represents the server id the follower votes for this term.
 And there is another variable named voted_terms, which is a set storing the terms that the server
 votes. Only when votedFor is None, and current term is not in voted_terms, the server could vote.
 When other followers receive requestVote RPC, they compare the log number, commit id, and last log 
 term of candidate with itself. If candidate status is at least up-to-date as itself, it votes to 
 the candidate by sending requestVote response back to the candidate. If one candidate receives more
 than half of followers that vote for it, it updates its type to leader, and send heartbeat to all
 followers, claiming that it is the leader now, all candidates return back to follower type. If clients
 send msg to followers, they should direct these msg to the leader. When election is processing, 
 there is no leader. If clients send msgs to servers, servers store these msgs into a local variable
 names clients_msgs. Once a candidate becomes a leader, it first processes the clients_msgs that it
 receives during election. When other followers receive the first heartbeat sent by the new leader,
 it redirects all msgs stored in client_msgs to clients and ask clients send these msgs to current leader.
 
 We set a random time for each time running a while loop, if not time out, according to the type
 of the server, which is one of 'leader', 'candidate', and 'follower' to handle the msg received from other
 replicas or clients. There are major three methods, leader_handle_msg, candidate_handle_msg, follower_handle_msg.
 In each method, according to different types of msg, process differently. If leader receives get msg
 from client, get the msg from its database directly. If it receives put msg, it replicas to other followers by
 sending APRPC. When followers and candidates receives APRPC which contains the newest log entry, 
 they first compare the index of the log entry to determine if this is the next one they should receive.
 If it is, they append the new log, and send APRPC back to the leader. When more than half followers
 send APRPC response back to the leader, the leader commits the log, and send ready to commit to all followers.
There is a local variable named last_log_entries which is a dictionary to store each server's last 
log index. Leader periodically checks the last log index of each follower and send them the entries 
they don't have according to last_log_entries. If a follower finds that it has less entries than leader,
it sends updateEntries request to the leader ask leader to send the entries it doesn't have. If a 
follower finds that it has more entries than leader, it removes the previous entries, appends the new
entry in APRPC and sends back APRPC response back to the leader. If leader receives requestVote RPC, or
APRPC which has higher term than current term, it returns back to follower type, updates current term
and resets votes. After time out, the server restarts new election.

### challenges
There were lots of challenges we have faced. Since this is a programming running 5 servers together, for
keeping consistency of 5 servers, there are lots of variables needed to track server status and we need to 
carefully update each variable. We need to consider each edge case into our code which is greatly difficult.

### test code
We tested our code by running each test case.