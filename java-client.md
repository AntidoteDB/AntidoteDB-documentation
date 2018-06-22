---
description: The tutorial serves as a guide for different client interfaces of Antidote.
---

# Java Client

As a running example, we use a collaborative editing application where multiple users can concurrently update a shared todo list. We discuss how Antidote handles conflicting updates by multiple users based on its CRDT data model.

### Requirements

* Build AntidoteDB using [Docker ](https://docs.docker.com/install/)
* Clone the [Todolist application](https://github.com/shraddhabarke/antidote-todolist) from github and follow the setup instructions from [README](https://github.com/shraddhabarke/antidote-todolist/blob/master/README.md)

 In this tutorial, you will learn the following concepts:

* Model data of Java application in AntidoteDB 
* Update and read CRDT objects in AntidoteDB 
* Run two AntidoteDB nodes concurrently 
* Test the behavior of CRDT objects under concurrent access.

### Connecting to AntidoteDB

Before getting into the details of data modelling of the application in Antidote, we look at how to run the application commands on top of Antidote. We start two AntidoteDB node instances and run two separate application instances on top of each node. This enables us to see how Antidote tackles inconsistencies arising due to concurrent updates in different replicas.

Start two antidote node instances using the start\_antidote script in setup folder.

```bash
./start_antidote.sh
```

 In two separate terminals, start two application instances on the antidote nodes:

```bash
./app1.sh
./app2.sh
```

 Now connect to antidote on each of the terminal:

```bash
connect antidote1 8087
connect antidote2 8087
```

 We now have an AntidoteDB cluster with two replicas!

![](https://syncfree.github.io/antidote/images/replicas.jpeg)

### Running the Application

The application commands for interacting with Antidote can be found in the application README

Antidote resolves conflicts by giving preference to update operations over delete operations. This is referred to as add wins semantics. In case of a register data type, the latest update gets priority, intuitively called the last writer wins semantics. Throughout the application model, we follow add wins and last writer wins semantics.

We simulate real world network partition between two replicas of AntidoteDB. Here, we mimic two users running the application in terminal 1 and terminal 2. Initially the two replicas are connected and Antidote replicates data across them. In case of a network failure, Antidote will resolve conflict according to add wins and last writer wins semantics.

Create a new board with name “boardname” in terminal 1. A unique boardid is returned which is later used to perform operations on the board.

```bash
createboard boardname
```

Now, run the disconnect script that disrupts the communication between the two replicas. This simulates the disconnect which might happen due to network failuers. Now the scenario is equivalent to two users performing operations concurrently.

```bash
./disconnect.sh
```

Rename the board previously created, replacing board\_id in the command with the id returned.

```bash
renameboard board_id newnamet1
```

 Rename the same board to a different name using terminal 2.

```text
renameboard board_id newnamet2
```

 Run getboard command on both terminals, the outputs are as follows

```text

```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

```
// Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```



