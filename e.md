# Replication

_Replication_ means keeping a copy of same data on multiple machines that are connected via network. There are three popular replication algorithms:

1. Single leader
2. Multi-leader
3. Leaderless

## Single leader

It is also known as _active-passive or_ _master-slave_ replication_._ Each node that store a copy of data is called_ replica_. One of the replicas is the_ leader_. When clients want to save the data to the database, they must first send the request to the leader. Leader will save the data and forward the changes to the rest of the replicas, which are called _followers_. The followers will apply the changes in the same order as they were processed on the leader. When clients want to read data from the database, they can query from leader or followers.

### Setting up new followers

1. Take a snapshot of leader.
2. Copy the snapshot to the new follower.
3. Connect the new follower to leader and request all the changes since the snapshot was taken.

### Handling node outages

#### Follower failure: catch-up recovery

Each follower keeps a log of data changes it has received from leader. Thus the follower can identify the last change from the log and request all the data changes since that point from leader.

#### Leader failure: failover

123

