<p align="center">
    <a href="https://cassandra.apache.org/">
        <img width="70%" src="https://upload.wikimedia.org/wikipedia/commons/5/5e/Cassandra_logo.svg">
    </a>
</p>

# Cassandra for Developers

Following the Pluralsight course [Cassandra for Developers](https://app.pluralsight.com/library/courses/cassandra-developers/table-of-contents) by [Paul O'Fallon](https://app.pluralsight.com/profile/author/paul-ofallon).

## What is Cassandra?

### A Basic Cassandra Cluster in Docker

Go to the single data center directory.

```bash
cd single-data-center
```

#### Starting a node

```bash
docker-compose up -d n1
```

#### Checking the cluster status

```bash
docker-compose exec n1 nodetool status
```

#### Checking the cluster ring

```bash
docker-compose exec n1 nodetool ring
```

#### Inspecting a node's configuration file

```bash
docker-compose exec n1 more /etc/cassandra/cassandra.yaml
```

#### Shutting down the cluster

```bash
docker-compose down
```

## Replication and Consistency

### Tunable Consistency

#### Single Data Center

@TODO

#### Multi Data Center

Go to the `multi-data-center`directory.

```bash
cd multi-data-center
```

Launch the multi data center cluster.

```bash
docker-compose up -d
```

Run `cqlsh`on `n1`.

```bash
docker-compose exec n1 cqlsh
```

Create a keyspace.

```bash
cqlsh> create keyspace lordoftherings with replication = { 'class':'NetworkTopologyStrategy', 'DC1':2, 'DC2':1 };
```

Use the keyspace created.

```bash
cqlsh> use lordoftherings;
```

Create a table.

```bash
cqlsh:lordoftherings> create table hobbits (id varchar primary key);
```

Check the current consistency level.

```bash
cqlsh:lordoftherings> consistency;
```

Change it to `LOCAL_ONE`.

```bash
cqlsh:lordoftherings> consistency local_one;
```

Insert new content in the table. It succeeds, as we would expect it to.

```bash
cqlsh:lordoftherings> insert into hobbits (id) values ('Bilbo');
```

Now let's exit `cqlsh`.

```bash
cqlsh:lordoftherings> exit;
```

Let's shutdown our one node in `DC2`. This will be equivalent to losing networking connectivity to `n3`.

```bash
docker-compose stop n3;
```

Return to `cqlsh`on `n1`, use the keyspace, and set the consistency level to èach_quorum`.

```bash
docker-compose exec n1 cqlsh;
cqlsh> use lordoftherings;
cqlsh:lordoftherings> consistency each_quorum;
```

This will require that we achieve a quorum in `DC1` and `DC2`. Which we expect to fail.

```bash
cqlsh:lordoftherings> insert into hobbits (id) values ('Sam');
```

As expected, the quorum in `DC2` is not met, and the inster fails.

Now, let's chage the consistency level to `LOCAL_QUORUM`.

```bash
cqlsh:lordoftherings> consistency local_quorum;
```

An try our insert statement again.

```bash
cqlsh:lordoftherings> insert into hobbits (id) values ('Sam');
```

It succeeds :)
