---
description: Getting started with Antidote.
---

# Installation

## Quickstart with Docker <a id="quickstart-with-docker"></a>

### Prerequisites <a id="prerequisites"></a>

* Working recent version of [Docker](https://docs.docker.com/install/) \(1.12 and up recommended\)

### Starting a local node <a id="starting-a-local-node"></a>

Start a local node with the command:

```bash
docker run -d --name antidote -p "8087:8087" antidotedb/antidote
```

This should fetch the Antidote image automatically. For updating to the latest version use the command `docker pull antidotedb/antidote`.

Wait until Antidote is ready. The current log can be inspected with `docker logs antidote`. Wait until the log message `Application antidote started on node 'antidote@127.0.0.1'` appears.

Antidote should now be running on port 8087 on localhost.

## Building Antidote releases <a id="building-antidote-releases"></a>

#### Prerequisites <a id="prerequisites-1"></a>

* An UNIX-like OS
* Erlang 18.3 or later

#### Getting Antidote

The source code is available open-source at [Github](https://github.com/AntidoteDB/antidote).

You can clone it to your machine via

```text
git clone git@github.com:AntidoteDB/antidote.git
```

#### Building a single node cluster

Antidote uses rebar3 for building releases.

```bash
make rel
```

Rebar3 will now pull all the dependencies it needs from Github, and build the application, and make an Erlang “release” of a single node. If all went well \(if it didn’t, contact us at [info@antidotedb.com](mailto:info@antidotedb.com)\), then you should be able to start a node:

```erlang
15:55:05:antidote $ make shell
(elided)
Eshell V5.10.3  (abort with ^G)
(antidote@127.0.0.1)1>
```

Type `Ctrl-g` and `q` to quit the shell and stop the node.

#### Reading from and writing to a CRDT object stored in antidote: <a id="reading-from-and-writing-to-a-crdt-object-stored-in-antidote"></a>

Antidote can store and manage different [crdts](https://github.com/AntidoteDB/antidote_crdt). Here we see how to update and read a counter.

Start a node \(if you haven’t done it yet\):

```bash
make shell
```

**Writing**

 Perform a write operation \(example\):

```erlang
CounterObj = {my_counter, antidote_crdt_counter_pn, my_bucket}.
{ok, TxId} = antidote:start_transaction(ignore, []).
ok = antidote:update_objects([{CounterObj, increment, 1}], TxId).
{ok, _CommitTime} = antidote:commit_transaction(TxId).
```

 You can also update objects with a single call as follows:

```erlang
CounterObj = {my_counter, antidote_crdt_counter_pn, my_bucket}.
{ok, _CommitTime} = antidote:update_objects(ignore, [], [{CounterObj, increment, 1}]).
```

**Reading**

 Perform a read operation \(example\):

```erlang
CounterObj = {my_counter, antidote_crdt_counter_pn, my_bucket}.
{ok, TxId} = antidote:start_transaction(ignore, []).
{ok, [CounterVal]} = antidote:read_objects([CounterObj], TxId).
{ok, _CommitTime3} = antidote:commit_transaction(TxId).
```

 Or in a single call:

```erlang
CounterObj = {my_counter, antidote_crdt_counter_pn, my_bucket}.
{ok, Res, _CommitTime} = antidote:read_objects(ignore, [], [CounterObj]).
[CounterVal] = Res.
```

 More about the API is described here :
  
 {% page-ref page="api/native-api.md" %}

###  Running Tests

#### Run only single dc tests <a id="run-single-dc-tests"></a>

```bash
make singledc
```

#### Run only multi dc tests <a id="run-multi-dc-tests"></a>

```bash
make multidc
```

#### Run all tests <a id="run-all-tests"></a>

```bash
make systests
```

#### Running specific tests <a id="running-specific-tests"></a>

 Tests are located in `$ANTIDOTE/test/`

```bash
make systests SUITE=xxx_SUITE
```
