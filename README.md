# About

Yamabiko is a middleware that track insert/update/delete event from MySQL database and multiple table replication into Elasticsearch nodes.  
In the future, it's comming support replicate to another RDB/noSQL.

This product is based on Fluentd which maintained at [fluentd project](http://github.com/fluent/).

# Requirement

* openssl
* pbuilder
* mysql-devel

# Prepare

This script will create the debian environments by pbuilder-dist.

# Build

## deb

For the first time, you need to prepare chroot environments for each distribution. This takes a time.

```bash
$ ./make-deb-init.sh
```

Then, execute make-deb.sh.

```bash
$ ./make-deb.sh
```

## rpm

```bash
$ ./make-rpm.sh
```

If you use specific revision of fluentd...

```bash
$ ./make-rpm.sh e20137ce1f92c4e364d304969f56bf41e713ee07
```

# Notice

yamabiko consists of the following components, and packaged as rpm/deb.

* ruby (1.9.3-p194): http://www.ruby-lang.org/en/
* jemalloc: http://www.canonware.com/jemalloc/
* fluentd: https://github.com/fluent/fluentd
* fluent-plugin-mysql-replicator: https://github.com/y-ken/fluent-plugin-mysql-replicator

yamabiko depends on those packages.

* openssl, readline, libxslt, libxml2
* [yamabiko-libyaml](https://github.com/y-ken/yamabiko-libyaml) (rpm) or libyaml (deb)

# TODO

Pull requests are very welcome like below!!

* support debian
* support homebrew

# Copyright

* Kentaro Yoshida ([@yoshi_ken](https://twitter.com/yoshi_ken))
* (TreasureData; Original td-agent author) https://github.com/treasure-data/td-agent

# License

Released under the Apache2 license.
