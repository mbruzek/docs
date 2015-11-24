Title: Metadata.yaml reference

The only file that must be present in a charm is `metadata.yaml`, in the root
directory. A metadata file must be a valid yaml dictionary, containing at least
the following fields:

### name
is the charm name, which is used to form the charm URL.

  - It can only contain `a-z`, `0-9`, and `-`; must start with `a-z`; must not
    end with a `-`; and may only end with digits if the digits are _not_
    directly preceded by a space. Stick with names like `foo` and `foo-bar-baz`
    and you needn't pay further attention to the restrictions.

```yaml
name: vanilla
```

### summary

is a one-line description of the charm.

```yaml
summary: Deploy the latest Vanilla Bulletin Board software on php5/apache
```

### maintainer

is the name and email address for the main point of contact
for the development and maintenance of the charm. The maintainer field
should be in the format

```yaml
maintainer: Charm Author Name <author@email>
```

### maintainers
is a list of people who maintain the charm. Use the yaml
sequence format when there are more than one person maintaining the project.

```yaml
maintainers:
  - Wiley Werewolf <wiley@ubuntu.com>
  - Gutsy Gibbon <gutsy@ubuntu.com>
```
### description

is a long-form description of the charm and its features.
It will also appear in the juju GUI.

```yaml
description: |
  Deploys an up-to-date Apache2 from the ubuntu archives, additionally configures
  php5-fpm. The latest stable version of vanilla will be cloned from github and
  placed in a virtualenv ready for serving on http://public-ip
```

### tags

is a descriptive tag that is used to sort the charm in the store.

Recommended tags to help keep the charm store organized is outlined below:

- analytics
- big_data
- ecommerce
- openstack
- cloudfoundry
- cms
- social
- streaming
- wiki
- ops
- backup
- identity
- monitoring
- performance
- audits
- security
- network
- storage
- database
- cache-proxy
- application_development
- web_server


### subordinate
boolean value denoting if the charm is a [`subordinate`](#TODO) if the charm is
subordinate, it must contain at least one `requires` relation with container
scope.

### provides

yaml formatted list of relations that the charm provides to the model.

```yaml
provides:
  website:
    interface: http
```

### requires

```yaml
requires:
  database:
    interface: mysql
```

### peers

peer relationships are special in the sense that they are implied anytime you
`juju add unit` to a service. If anything needs to be configured between the two
units that may be running on separate hosts - such as sharing an NFS filesystem
can be invoked over the peer relationship.

```yaml
peers:
  peer-media:
    interface: nfs
```

## Example Metadata.yaml

  ```yaml
name: mongodb
summary: An open-source document database, and the leading NoSQL database
description: |
  MongoDB is a high-performance, open source, schema-free document- oriented
  data store that's easy to deploy, manage and use. It's network accessible,
  written in C++ and offers the following features:  
  - Collection oriented storage
  - easy storage of object-style data
  - Full index support, including on inner objects
  - Query profiling
  - Replication and fail-over support
  - Efficient storage of binary data including large objects (e.g. videos)
  - Auto-sharding for cloud-level scalability (Q209) High performance,
  scalability, and reasonable depth of functionality are the goals for the
  project.
  tags:
      - databases
  provides:
    nrpe-external-master:
      interface: nrpe-external-master
      scope: container
    database:
      interface: mongodb
    configsvr:
      interface: shard
    data:
      interface: block-storage
      scope: container
      optional: true
    benchmark:
      interface: benchmark
  requires:
    mongos-cfg:
      interface: shard
    mongos:
      interface: mongodb
  peers:
    replica-set:
      interface: mongodb-replica-set
  ```

## Note on other field names

  Other field names should be considered to be reserved; please don't use any not
  listed above to avoid issues with future versions of Juju.
