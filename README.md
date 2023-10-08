### CAP Theorem:

- **Consistency**: Every read receives the most recent write. So, all users see the same data, no matter which node they connect to.

- **Availability**: Every request (whether read or write) gets a response. The response can be success, an error, or the most recent data, but you always get an answer.

- **Partition Tolerance**: The system continues to work even if communication breaks down between nodes (like if there's a network glitch).

The CAP Theorem, often attributed to Eric Brewer, asserts that in the presence of a network partition, a system can either remain consistent or available, but not both. Graphically, it's often represented as a triangle where each vertex represents one of the CAP properties. A system can be:

### CA (Consistency and Availability) 
- **Description**: All nodes see the same data simultaneously (Consistency), and every request receives a response (Availability). This combination is rare in real-world distributed systems since handling network partitions (P) is essential for large-scale systems.
- **Example**: Traditional relational databases (RDBMS) like MySQL or PostgreSQL running in a single-node configuration could be seen as CA. In practice, distributed systems usually need to handle partitions, making pure CA systems rare in large-scale scenarios.


### CP (Consistency and Partition tolerance)  
- **Description**: Systems ensure that all nodes see the same data simultaneously (Consistency) and can handle network partitions (Partition Tolerance). However, they might sacrifice Availability during partitions.
- **Example**: ZooKeeper, used for configuration management, naming service, and coordination, is a system that emphasizes consistency and partition tolerance. If a partition occurs, some ZooKeeper nodes might become unavailable to ensure that data remains consistent across the system.


### AP (Availability and Partition tolerance)
- **Description**: Systems ensure every request gets a response (Availability) and can handle network partitions (Partition Tolerance). However, they might return stale or data might not be consistent across all nodes.
- **Example**: Cassandra, a NoSQL database, is designed to prioritize availability and partition tolerance. In the event of network partitions, Cassandra might serve stale data, choosing to be available over being immediately consistent. Over time, it uses mechanisms like hinted handoff and read repair to restore consistency.


***Why can't we have all three at once?***

Imagine two nodes in a system, Node A and Node B. Suddenly, there's a network glitch, and A can't talk to B.

If you prioritize Consistency:

You won't allow any writes unless both A and B can communicate. Why? Because you want to make sure any new data is immediately reflected on both nodes. This means you're sacrificing availability; not all requests will get valid responses.
If you prioritize Availability:

Both A and B will continue to accept writes, even if they can't talk to each other. This way, users always get responses. However, this can lead to inconsistencies. Imagine if two users write conflicting data—one to A and one to B. Now, your data is inconsistent between the nodes.

Given that network glitches (partitions) are a reality in distributed systems, the CAP theorem states that when such a partition occurs, a system must choose between Consistency and Availability—it can't ensure both. Once the partition is resolved, systems can take steps to become consistent again, but during the partition, the trade-off is unavoidable.

In summary, the CAP theorem highlights the tough choices in designing distributed systems. When faced with the inevitable network partition, the system can either:

Make sure all data is consistent but might not respond to all requests (prioritizing C over A).
Respond to all requests but risk having inconsistent data (prioritizing A over C).



