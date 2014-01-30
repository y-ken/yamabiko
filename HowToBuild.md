# Yamabiko

# Build Requirement

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

# Copyright

* Kentaro Yoshida ([@yoshi_ken](https://twitter.com/yoshi_ken))
* (TreasureData; Original td-agent author) https://github.com/treasure-data/td-agent

# License

Released under the Apache2 license.
