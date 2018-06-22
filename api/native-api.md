---
description: This page describes the native Erlang API of Antidote.
---

# Native API

Clients can invoke these functions via RPC. A more convenient but restricted way for client applications to interact with Antidote is the [protocol buffer interface](https://syncfree.github.io/api.html).

### CRDTs {#crdts}

Antidote provides a library of CRDTs. The interface of these CRDTs specify the operations and the parameters that can be used for inspection and modification of shared objects. In the following, we specify the supported `{operation(), op_param()}` pair for each of the supported CRDTs. The first element in the tuple specifies the update operation, and the second item indicates the corresponding parameters.

antidote\_crdt\_counter\_pn

```erlang
{increment, integer()}
{decrement, integer()}
```

antidote\_crdt\_set\_aw

```erlang
{add, term()}
{remove, term()}
{add_all, [term()]}
{remove_all, [term()]}
```

antidote\_crdt\_gset

```erlang
{add, {term(), actor()}}
{remove, {term(), actor()}}
{add_all, {[term()], actor()}}
{remove_all, {[term()], actor()}}
```

antidote\_crdt\_register\_lww

```erlang
{assign, {term(), non_neg_integer()}}
{assign, term()}.
```

antidote\_crdt\_map\_rr

```erlang
{update, {[map_field_update() | map_field_op()], actorordot()}}.

-type actorordot() :: riak_dt:actor() | riak_dt:dot().
-type map_field_op() ::  {remove, field()}.
-type map_field_update() :: {update, field(), crdt_op()}.
-type crdt_op() :: term(). %% Valid riak_dt updates
-type field() :: term()
```

antidote\_crdt\_register\_mv

```erlang
{assign, {term(), non_neg_integer()}}
{assign, term()}
{propagate, {term(), non_neg_integer()}}
```

antidote\_crdt\_rga

```erlang
{addRight, {any(), non_neg_integer()}}
{remove, non_neg_integer()}
```

### Transactions {#transactions}

A unit of operation in Antidote is a transaction. A client should first start a transaction, then read and/or update several objects, and finally commit the transaction.

There are two types of transactions: interactive transactions and static transactions.

