---
description: This guide will walk you through the core features of AntidoteDB.
---

# Quick Start Guide

We will use Docker to run AntidoteDB nodes. Please refer to the complete installation guide below if you wish to build the nodes from source:

{% page-ref page="overview/installation.md" %}

 In this guide, you will accomplish the following steps:

* Start two AntidoteDB nodes and connect them to each other;
* Start and commit a transaction;
* Use the Bounded Counter data type.

### Requirements <a id="requirements"></a>

* Install the latest version of Docker \([Installation Guide](https://docs.docker.com/engine/installation/)\).

### Create a cluster <a id="create-a-cluster"></a>

> If you prefer to skip the steps to build the cluster manually, there is a Docker Compose file available at: https://github.com/mweberUKL/antidote\_dev/tree/master/docker\_dcs.

 Fetch AntidoteDB docker image by typing:

```bash
docker pull antidotedb/antidote
```

 Create a virtual network interface:

```bash
docker network create --driver bridge default_ntwk
```

 Start two AntidoteDB node instances which will be called ‘antidote1’ and ‘antidote2’:

```text
docker run -i -t -d --name antidote1 --network default_ntwk -e SHORT_NAME=true -e NODE_NAME=antidote@antidote1 antidotedb/antidote

docker run -i -t -d --name antidote2 --network default_ntwk -e SHORT_NAME=true -e NODE_NAME=antidote@antidote2 antidotedb/antidote
```

 Connect to the console of one node:

```bash
docker exec -it antidote1 /opt/antidote/bin/env attach
```

 Run the following lines of code to connect the two nodes:

```erlang
rpc:call(antidote@antidote1, inter_dc_manager, start_bg_processes, [stable]),
rpc:call(antidote@antidote2, inter_dc_manager, start_bg_processes, [stable]),
{ok, Desc1} = rpc:call(antidote@antidote1, inter_dc_manager, get_descriptor, []),
{ok, Desc2} = rpc:call(antidote@antidote2, inter_dc_manager, get_descriptor, []),
Descriptors = [Desc1, Desc2],
rpc:call(antidote@antidote1, inter_dc_manager, observe_dcs_sync, [Descriptors]),
rpc:call(antidote@antidote2, inter_dc_manager, observe_dcs_sync, [Descriptors]).
```

 We now have an AntidoteDB cluster with two replicas!

### Connecting to AntidoteDB

 In two separate terminals, connect to the console of each node:

```bash
docker exec -it antidote1 /opt/antidote/bin/env attach
docker exec -it antidote2 /opt/antidote/bin/env attach
```

> If you used the Compose file before, you connect to the console of a replica by typing:

```bash
docker-compose exec antidote1 /opt/antidote/bin/env attach
```

### Interacting with AntidoteDB <a id="interacting-with-antidotedb"></a>

Start a new transaction and increment the value of a bounded counter. By default, if the counter does not exist, one is created automatically.

```erlang
{ok, TxId} = antidote:start_transaction(ignore, []).
antidote:update_objects([{ {counter_key, antidote_crdt_counter_b, test_bucket}, increment, {10, client1}}], TxId).
antidote:commit_transaction(TxId).
# output: {ok, ...}
```

 The counter can be decremented in the replica where it was created:

```erlang
{ok, TxId1} = antidote:start_transaction(ignore, []).
antidote:update_objects([{ {counter_key, antidote_crdt_counter_b, test_bucket}, decrement, {1, client1}}], TxId1).
antidote:commit_transaction(TxId1).
# output: {ok, ...}
```

 And the value of the counter can be read on the other replica \(use the other terminal\):

```erlang
{ok, TxId} = antidote:start_transaction(ignore, []).
{ok, [Obj]} = antidote:read_objects([{counter_key, antidote_crdt_counter_b, test_bucket}], TxId).
antidote_crdt_counter_b:permissions(Obj).
# output: 9
```

 But cannot be decremented:

```erlang
{error, _} = antidote:update_objects([{ {counter_key, antidote_crdt_counter_b, test_bucket}, decrement, {1, client2}}], TxId).
# output: {error,{aborted, ...}}
```

 At this point, the transaction aborts and the system will automatically fetch permissions from another replica. After the resources are transfered between nodes, it is possible to decrement the counter.

```erlang
{ok, TxId1} = antidote:start_transaction(ignore, []).
antidote:update_objects([{ {counter_key, antidote_crdt_counter_b, test_bucket}, decrement, {1, client2}}], TxId1).
antidote:commit_transaction(TxId1).
# output: {ok, ...}
```

Look at the documentation to see more examples of AntidoteDB in action. We are working on new features for AntidoteDB, contact us at info@antidotedb.com if you want to know more.

