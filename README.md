<p align="center">
    <a href="https://cassandra.apache.org/">
        <img width="70%" src="https://upload.wikimedia.org/wikipedia/commons/5/5e/Cassandra_logo.svg">
    </a>
</p>

# Cassandra for Developers

Following the Pluralsight course [Cassandra for Developers](https://app.pluralsight.com/library/courses/cassandra-developers/table-of-contents) by [Paul O'Fallon](https://app.pluralsight.com/profile/author/paul-ofallon).

## Starting a node

```bash
docker-compose up -d n1
```

## Checking the cluster status

```bash
docker-compose exec n1 nodetool status
```

## Checking the cluster ring

```bash
docker-compose exec n1 nodetool ring
```

## Inspecting a node's configuration file

```bash
docker-compose exec n1 more /etc/cassandra/cassandra.yaml
```

## Shutting down the cluster

```bash
docker-compose down
```
