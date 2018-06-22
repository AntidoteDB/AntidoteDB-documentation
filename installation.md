---
description: Getting started with Antidote.
---

# Installation

## Quickstart with Docker {#quickstart-with-docker}

### Prerequisites {#prerequisites}

* Working recent version of Docker \(1.12 and up recommended\)

### Starting a local node {#starting-a-local-node}

 Start a local node with the command:

```bash
docker run -d --name antidote -p "8087:8087" antidotedb/antidote
```

This should fetch the Antidote image automatically. For updating to the latest version use the command `docker pull antidotedb/antidote`.

Wait until Antidote is ready. The current log can be inspected with `docker logs antidote`. Wait until the log message `Application antidote started on node 'antidote@127.0.0.1'` appears.

Antidote should now be running on port 8087 on localhost.

## Building Antidote releases {#building-antidote-releases}

#### Prerequisites {#prerequisites-1}

* An UNIX-like OS
* Erlang 18.3 or later

#### Getting Antidote

The source code is available open-source at [Github](https://github.com/SyncFree/antidote).

You can clone it to your machine via

```text
git clone git@github.com:SyncFree/antidote.git
```

####  Building a single node cluster

 Antidote uses rebar3 for building releases.

```bash
make rel
```

Rebar3 will now pull all the dependencies it needs from Github, and build the application, and make an Erlang “release” of a single node. If all went well \(if it didn’t, contact us at [info@antidotedb.com](mailto:info@antidotedb.com)\), then you should be able to start a node:

```erlang
15:55:05:antidote $ make console
(elided)
Eshell V5.10.3  (abort with ^G)
(antidote@127.0.0.1)1>
```

 Type `Ctrl-g` and `q` to quit the shell and stop the node.

#### Reading from and writing to a CRDT object stored in antidote: {#reading-from-and-writing-to-a-crdt-object-stored-in-antidote}

Antidote can store and manage different [crdts](https://github.com/SyncFree/antidote_crdt). Here we see how to update and read a counter.

Start a node \(if you haven’t done it yet\):

```bash
make console
```

##### Writing








