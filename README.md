# About

Yamabiko is a middleware that track insert/update/delete event from MySQL database and multiple table replication into Elasticsearch/Solr nodes.  
In the future, it's comming support replicate to another RDB/noSQL.

This product is based on Fluentd which maintained at [fluentd project](http://github.com/fluent/).

# Configuration

Please refer the core plugin (fluent-plugin-mysql-replicator) repository page.  
https://github.com/y-ken/fluent-plugin-mysql-replicator

# Download

RPM package has released at this page.  
https://github.com/y-ken/yamabiko/releases

# Notice

yamabiko consists of the following components, and packaged as rpm/deb.

* ruby (1.9.3-p194): http://www.ruby-lang.org/en/
* jemalloc: http://www.canonware.com/jemalloc/
* fluentd: https://github.com/fluent/fluentd
* fluent-plugin-mysql-replicator: https://github.com/y-ken/fluent-plugin-mysql-replicator

yamabiko depends on those packages.

* openssl, readline, libxslt, libxml2, mysql-devel
* [yamabiko-libyaml](https://github.com/y-ken/yamabiko-libyaml) (rpm) or libyaml (deb)

# Articles

* Fluentdベースのミドルウェア"Yamabiko"でMySQLのテーブルをElasticsearchへレプリケートする話  
http://www.slideshare.net/y-ken/yamabiko-replicate-mysql-table-to-elasticsearch

# TODO

Pull requests are very welcome.

* support debian
* support homebrew

# Copyright

* Kentaro Yoshida ([@yoshi_ken](https://twitter.com/yoshi_ken))
* (TreasureData; Original td-agent author) https://github.com/treasure-data/td-agent

# License

Released under the Apache2 license.
