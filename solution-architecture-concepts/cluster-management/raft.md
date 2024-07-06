# RAFT

### Raft Consensus Algorithm

Raft is a consensus algorithm designed to be easy to understand compared to alternatives like Paxos. It allows a cluster of servers to agree on a sequence of values, providing fault tolerance and consistency in distributed systems.The key components of Raft are:**Leader Election**

* One server is elected as the leader, responsible for handling client requests and replicating log entries to followers.
* Followers periodically check for heartbeats from the leader. If the leader fails, followers start a new election to choose a new leader.

**Log Replication**

* The leader appends new log entries and replicates them to the followers.
* Once a majority of nodes have replicated the entry, it is considered committed and can be applied to the state machine.

**Safety Properties**

* Raft ensures safety properties like "election safety" (only one leader per term) and "log matching" (logs of nodes match up to a point).

### Raft in Distributed Systems

Raft is commonly used in distributed databases, message queues, and other fault-tolerant distributed systems:

* **Distributed Databases**: Raft is used to achieve consensus on the state of the database, ensuring consistency even if some nodes fail.
* **Message Queues**: Raft is used to replicate the message log across nodes, allowing the queue to tolerate failures.
* **Coordination Services**: Tools like etcd and Consul use Raft to coordinate configuration, discovery, and other distributed state.

The key benefits of using Raft in these systems are:

* **Simplicity**: Raft has a simpler design than alternatives like Paxos, making it easier to understand and implement.
* **Fault Tolerance**: Raft can continue operating as long as a majority of nodes are available, tolerating failures.
* **Scalability**: Raft scales well to large clusters, making it suitable for growing distributed systems.

Overall, Raft provides a robust and practical consensus algorithm that has become a standard building block for modern distributed applications.

{% embed url="https://youtu.be/IujMVjKvWP4?si=8NsUEGrspi5ZshRe" %}

{% embed url="https://youtu.be/P9Ydif5_qvE?si=L_cx04PwHx1IiHjO" %}
