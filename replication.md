# Replication

_Replication_ means keeping a copy of same data on multiple machines that are connected via network. There are three popular replication algorithms:

1. Single leader
2. Multi-leader
3. Leaderless

## Single Leader

It is also known as _active-passive or_ _master-slave_ replication_._ Each node that store a copy of data is called_ replica_. One of the replicas is the_ leader_. When clients want to save the data to the database, they must first send the request to the leader. Leader will save the data and forward the changes to the rest of the replicas, which are called _followers_. The followers will apply the changes in the same order as they were processed on the leader. When clients want to read data from the database, they can query from leader or followers.

### Setting up new followers

1. Take a snapshot of leader.
2. Copy the snapshot to the new follower.
3. Connect the new follower to leader and request all the changes since the snapshot was taken.

### Handling node outages

#### Follower failure: catch-up recovery

Each follower keeps a log of data changes it has received from leader. Thus the follower can identify the last change from the log and request all the data changes since that point from leader.

#### Leader failure: failover

If the leader failed, one of the followers need to be promoted to be the new leader. Clients need to be reconfigured to send requests to the new leader and followers need to start consume data changes from the new leader.

### Implementation of replication

#### Statement-based replication

The leader sends write statement request \(e.g. INSERT, UPDATE, or DELETE\) to its followers.

#### Write-ahead log \(WAL\) shipping

The leader sends the write-ahead logs \(e.g. low level details such as disk blocks changes\) to its followers. This makes replication closely coupled to storage engine, which makes it impossible to run different versions of leader and followers.

#### Logical \(row-based\) log replication

The leader sends actual data changes at row level to its followers. Logical logs are decoupled from the storage engine internals. Therefore its possible to run different versions of leader and followers.

#### Trigger-based replication

Register a _trigger_ \(e.g. custom application code\) that will only replicate a subset of the data to its followers.



